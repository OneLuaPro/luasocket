# MIT License
#
# Copyright (c) 2023-2026 Kritzel Kratzel.
#
# Permission is hereby granted, free of charge, to any person obtaining a copy of
# this software and associated documentation files (the "Software"), to deal in 
# the Software without restriction, including without limitation the rights to 
# use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
# the Software, and to permit persons to whom the Software is furnished to do so,
# subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in all 
# copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

# ------------------------------------------------------------------------------
# Notice - for luasec we want socket to be available as static lib with headers
# Common sources
set(SOCKET_C_FILES
  luasocket.c timeout.c buffer.c io.c auxiliar.c compat.c options.c
  inet.c except.c select.c tcp.c udp.c
)
set(SOCKET_H_FILES
  luasocket.h timeout.h buffer.h io.h auxiliar.h compat.h options.h
  inet.h except.h select.h tcp.h udp.h socket.h
)
# Plattform-dependent sources
if(WIN32 AND NOT MinGW)
  list(APPEND SOCKET_C_FILES wsocket.c)
  list(APPEND SOCKET_H_FILES wsocket.h)
else()
  message(FATAL_ERROR "Not yet fully implemented.")
endif()

# Object library definition
add_library(socket_obj OBJECT ${SOCKET_C_FILES})
target_include_directories(socket_obj PRIVATE ${LIBLUA_INCLUDEDIR})
# Add public header file_set
target_sources(socket_obj PUBLIC FILE_SET HEADERS FILES ${SOCKET_H_FILES})
# Compile options
if(WIN32 AND NOT MinGW)
  target_compile_options(socket_obj PRIVATE
    /D_CRT_SECURE_NO_WARNINGS /DLUASOCKET_${LUASOCKET_DEBUG_MODE})
endif()

# socket DLL for normal Lua integration
add_library(socket SHARED $<TARGET_OBJECTS:socket_obj>)
set_target_properties(socket PROPERTIES OUTPUT_NAME "socket")
target_sources(socket PUBLIC FILE_SET HEADERS FILES ${SOCKET_H_FILES})
target_include_directories(socket PRIVATE ${LIBLUA_INCLUDEDIR})
target_link_libraries(socket PRIVATE ${LIBLUA_LIBDIR}/liblua.lib ws2_32.lib)

# socket static lib for optional luasec integration
add_library(socket_static STATIC $<TARGET_OBJECTS:socket_obj>)
set_target_properties(socket_static PROPERTIES OUTPUT_NAME "libluasocket")
target_sources(socket_static PUBLIC FILE_SET HEADERS FILES ${SOCKET_H_FILES})
target_include_directories(socket_static PRIVATE ${LIBLUA_INCLUDEDIR})
if(WIN32)
  target_compile_definitions(socket PRIVATE _USRDLL _WINDLL
    _CRT_SECURE_NO_WARNINGS LUASOCKET_${LUASOCKET_DEBUG_MODE})
  # target_compile_definitions(socket_static PRIVATE LUASOCKET_EXTERNAL)
  target_compile_definitions(socket_static PRIVATE
    _CRT_SECURE_NO_WARNINGS LUASOCKET_${LUASOCKET_DEBUG_MODE})
endif()

# Account for specific directory structure and DLL naming conventions.
# See https://lunarmodules.github.io/luasocket/installation.html, chapter
# "Directory structure". See also:
# https://cmake.cmake.narkive.com/VIv2sdQB/rename-of-target-on-install
set_target_properties(socket
  PROPERTIES ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}/socket)
set_target_properties(socket
  PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/socket)
set_target_properties(socket
  PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/socket)
set_target_properties(socket PROPERTIES OUTPUT_NAME "core")

# Suppress some warnings for install
cmake_policy(SET CMP0177 NEW)

# Install dll
install(
  TARGETS socket
  RUNTIME DESTINATION ${INSTALL_SOCKET_CDIR}
)
# Install static_lib into bootstrap-directory LUA_HINTS
install(
  TARGETS socket_static
  FILE_SET HEADERS DESTINATION "${LUA_HINTS}/include/luasocket"
  ARCHIVE DESTINATION "${LUA_HINTS}/lib"
)

# ------------------------------------------------------------------------------
# mime DLL
add_library(mime SHARED)
# setup lua include directory
target_include_directories(mime PRIVATE ${LIBLUA_INCLUDEDIR})
# setup platform-specific files, compile and linker options
if(WIN32 AND NOT MinGW)
  target_compile_options(mime PRIVATE
    /D_USRDLL /D_CRT_SECURE_NO_WARNINGS /D_WINDLL /DLUASOCKET_${LUASOCKET_DEBUG_MODE})
  target_link_options(mime PRIVATE
    /LIBPATH:${LIBLUA_LIBDIR} liblua.lib ws2_32.lib)
else()
  message(FATAL_ERROR "Not yet fully implemented.")
endif()
# add sources
target_sources(mime
  PRIVATE mime.c mime.h compat.c compat.h
)
# Account for specific directory structure and DLL naming conventions.
# See https://lunarmodules.github.io/luasocket/installation.html, chapter
# "Directory structure". See also:
# https://cmake.cmake.narkive.com/VIv2sdQB/rename-of-target-on-install
set_target_properties(mime
  PROPERTIES ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}/mime)
set_target_properties(mime
  PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/mime)
set_target_properties(mime
  PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/mime)
set_target_properties(mime PROPERTIES OUTPUT_NAME "core")
# Install
install(
  TARGETS mime
  RUNTIME DESTINATION ${INSTALL_MIME_CDIR}
)

# ------------------------------------------------------------------------------
# unix.so (UNIX and alike only)
if(NOT WIN32)
  message(WARNING "luasocket unix.so: not yet integrated into CMakeLists.txt.")
endif()

# ------------------------------------------------------------------------------
# serial.so (UNIX and alike only)
if(NOT WIN32)
  message(WARNING "luasocket serial.so: not yet integrated into CMakeLists.txt.")
endif()

# ------------------------------------------------------------------------------
# install lua files to defined locations
install(
  FILES socket.lua mime.lua ltn12.lua
  DESTINATION ${INSTALL_TOP_LDIR}
)
install(
  FILES http.lua url.lua tp.lua ftp.lua headers.lua smtp.lua
  DESTINATION ${INSTALL_SOCKET_LDIR}
)

