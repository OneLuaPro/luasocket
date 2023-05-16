

# ------------------------------------------------------------------------------
# General definitions
cmake_minimum_required(VERSION 3.25 FATAL_ERROR)

# ------------------------------------------------------------------------------
# Installation prefix directory - defaults to C:\Apps\lua-X.Y.Z
# Needs to be defined before projectdefinition statement - for whatever reason

# execute_process(
#   COMMAND lua "-v"
#   RESULT_VARIABLE FOO
#   OUTPUT_VARIABLE luaVer
# )
# message(STATUS "Lua Version: ${luaVer}")
# FIXME: and find_package stuff here later...
if(NOT CMAKE_INSTALL_PREFIX)
  if(WIN32)
    set(CMAKE_INSTALL_PREFIX "C:/Apps/lua-5.4.6")
  else()
    message(FATAL_ERROR "Not yet implemented.")
  endif()
endif()

# ------------------------------------------------------------------------------
# Project defintion
project(luasocket
  LANGUAGES C
)

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
# find lua installation
#find_package(lua 5.4.6 REQUIRED
#  HINTS "c:/Apps/lua-5.4.6"
#)

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
set(DEF_INSTALL_CMAKEDIR share/cmake/${PROJECT_NAME})
set(INSTALL_CMAKEDIR ${DEF_INSTALL_CMAKEDIR} CACHE PATH "Installation directory for CMake files")

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
