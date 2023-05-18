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
#
# ------------------------------------------------------------------------------
# General definitions
cmake_minimum_required(VERSION 3.25 FATAL_ERROR)
set(LUA_VERSION_MAJOR 5)
set(LUA_VERSION_MINOR 4)
set(LUA_VERSION_PATCH 6)
set(LUA_VERSION ${LUA_VERSION_MAJOR}.${LUA_VERSION_MINOR}.${LUA_VERSION_PATCH})

# ------------------------------------------------------------------------------
# find liblua installation
# set(CMAKE_FIND_DEBUG_MODE ON) 
if(WIN32)
  set(LUA_HINTS "c:/Apps")
else()
  set(LUA_HINTS "")
endif()
find_package(liblua ${LUA_VERSION} EXACT REQUIRED CONFIG HINTS ${LUA_HINTS})
if(liblua_FOUND)
  message(STATUS "Found liblua version " ${LUA_VERSION}.)
  message(STATUS "LIBLUA_INSTALLDIR: ${LIBLUA_INSTALLDIR}")
  message(STATUS "LIBLUA_INCLUDEDIR: ${LIBLUA_INCLUDEDIR}")
  message(STATUS "LIBLUA_LIBDIR: ${LIBLUA_LIBDIR}")
else()
  message(FATAL_ERROR "Unable to find liblua version " ${LUA_VERSION}.)
endif()

# ------------------------------------------------------------------------------
# Installation prefix directory - automatically set from find_package()
# Needs to be defined before project definition statement - for whatever reason
set(CMAKE_INSTALL_PREFIX ${LIBLUA_INSTALLDIR})

# ------------------------------------------------------------------------------
# Project defintion
project(luasocket LANGUAGES C)

# ------------------------------------------------------------------------------
# Other settings
set(CMAKE_VERBOSE_MAKEFILE ON)
if(NOT CMAKE_BUILD_TYPE)
  # Visual Studio generators irgore this setting
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
endif()
# create valiable with upper-case content out of CMAKE_BUILD_TYPE
string(TOUPPER ${CMAKE_BUILD_TYPE} CMAKE_BUILD_TYPE_UC)

# ------------------------------------------------------------------------------
# Setup GNU-alike installatin directories
include (GNUInstallDirs)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/${CMAKE_INSTALL_BINDIR})
set(INSTALL_LIBDIR ${CMAKE_INSTALL_LIBDIR} CACHE PATH "Installation directory for libraries")
set(INSTALL_BINDIR ${CMAKE_INSTALL_BINDIR} CACHE PATH "Installation directory for executables")
set(INSTALL_INCLUDEDIR ${CMAKE_INSTALL_INCLUDEDIR} CACHE PATH "Installation directory for header files")
set(INSTALL_DOCDIR ${CMAKE_INSTALL_DOCDIR} CACHE PATH "Installation directorx for documentation")
set(INSTALL_MANDIR ${CMAKE_INSTALL_MANDIR} CACHE PATH "Installation directorx for manpages")
# Lua-specific installation dirs
set(INSTALL_SOCKET_CDIR ${INSTALL_LIBDIR}/lua/${LUA_VERSION_MAJOR}.${LUA_VERSION_MINOR}/socket)
set(INSTALL_MIME_CDIR ${INSTALL_LIBDIR}/lua/${LUA_VERSION_MAJOR}.${LUA_VERSION_MINOR}/mime)
set(INSTALL_TOP_LDIR ${LIBLUA_INSTALLDIR}/share/lua/${LUA_VERSION_MAJOR}.${LUA_VERSION_MINOR})
set(INSTALL_SOCKET_LDIR ${INSTALL_TOP_LDIR}/socket)

# ------------------------------------------------------------------------------
# Report to user
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
message(STATUS "C compiler flags: ${CMAKE_C_FLAGS} ${CMAKE_C_FLAGS_${CMAKE_BUILD_TYPE_UC}}")
message(STATUS "Prefix directory: ${CMAKE_INSTALL_PREFIX}")
foreach(p BIN INCLUDE LIB DOC MAN CMAKE)
  file(TO_NATIVE_PATH ${CMAKE_INSTALL_PREFIX}/${INSTALL_${p}DIR} _path)
  message(STATUS "Installing ${p} components to ${_path}")
  unset(_path)
endforeach()

# ------------------------------------------------------------------------------
# Dive into subdirs
add_subdirectory(src)
