# MIT License
#
# Copyright (c) 2023 Kritzel Kratzel.
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
# socket DLL
add_library(socket SHARED)
# setup lua include directory
target_include_directories(socket PRIVATE ${LIBLUA_INCLUDEDIR})
# setup platform-specific sources, compile and linker options
if(WIN32 AND NOT MinGW)
  target_sources(socket PRIVATE
    wsocket.c wsocket.h)
  target_compile_options(socket PRIVATE
    /D_USRDLL /D_CRT_SECURE_NO_WARNINGS /D_WINDLL /DLUASOCKET_${LUASOCKET_DEBUG_MODE})
  target_link_options(socket PRIVATE
    /LIBPATH:${LIBLUA_LIBDIR} liblua.lib ws2_32.lib)
else()
  message(FATAL_ERROR "Not yet fully implemented.")
  target_sources(socket PRIVATE usocket.c usocket.h)
endif()
# plattform-independend sources
target_sources(socket
  PRIVATE luasocket.c luasocket.h timeout.c timeout.h buffer.c buffer.h io.c
  io.h auxiliar.c auxiliar.h compat.c compat.h options.c options.h inet.c inet.h
  except.c except.h select.c select.h tcp.c tcp.h udp.c udp.h
)
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
# Install
install(
  TARGETS socket
  RUNTIME DESTINATION ${INSTALL_SOCKET_CDIR}
)

# ------------------------------------------------------------------------------
# mime DLL
add_library(mime SHARED)
# setup lua include directory
target_include_directories(mime PRIVATE ${LIBLUA_INCLUDEDIR})
# setup platform-specific files, compile and linker options
if(WIN32 AND NOT MinGW)
  target_compile_options(mime PRIVATE
    /D_USRDLL /D_CRT_SECURE_NO_WARNINGS /D_WINDLL /DLUASOCKET_NODEBUG)
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

