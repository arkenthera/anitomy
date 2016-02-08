# //----------------------------------------------------------------------------
# //Anitomy
# //Copyright (C) erengy
# //This program is free software; you can redistribute it and/or modify
# //it under the terms of the GNU General Public License as published by
# //the Free Software Foundation; either version 2 of the License, or
# //(at your option) any later version.
# //This program is distributed in the hope that it will be useful,
# //but WITHOUT ANY WARRANTY; without even the implied warranty of
# //MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# //GNU General Public License for more details.
# //You should have received a copy of the GNU General Public License along
# //with this program; if not, write to the Free Software Foundation, Inc.,
# //51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.*/
# //----------------------------------------------------------------------------
cmake_minimum_required(VERSION 2.8.12.2)
set(CMAKE_CONFIGURATION_TYPES Debug Release)
project(Anitomy)

set(ANITOMY_SOURCES
	anitomy/anitomy.cpp
	anitomy/anitomy.h
	anitomy/element.cpp
	anitomy/element.h
	anitomy/keyword.cpp
	anitomy/keyword.h
	anitomy/options.h
	anitomy/parser.cpp
	anitomy/parser.h
	anitomy/parser_helper.cpp
	anitomy/parser_number.cpp
	anitomy/string.cpp
	anitomy/string.h
	anitomy/token.cpp
	anitomy/token.h
	anitomy/tokenizer.cpp
	anitomy/tokenizer.h
	anitomy/config.h)

# Determine the platform.
if("${CMAKE_SYSTEM_NAME}" STREQUAL "Darwin")
  set(OS_MACOSX 1)
  set(OS_POSIX 1)
elseif("${CMAKE_SYSTEM_NAME}" STREQUAL "Linux")
  set(OS_LINUX 1)
  set(OS_POSIX 1)
elseif("${CMAKE_SYSTEM_NAME}" STREQUAL "Windows")
  set(OS_WINDOWS 1)
endif()

add_definitions(-DBuildingAnitomy)

if(NOT DEFINED PROJECT_ARCH)
  if(CMAKE_SIZEOF_VOID_P MATCHES 8)
    set(PROJECT_ARCH "x86_64")
  else()
    set(PROJECT_ARCH "x86")
  endif()

  if(OS_MACOSX)
    # PROJECT_ARCH should be specified on Mac OS X.
    message(WARNING "No PROJECT_ARCH value specified, using ${PROJECT_ARCH}")
  endif()
endif()

if(NOT CMAKE_BUILD_TYPE AND
   (${CMAKE_GENERATOR} STREQUAL "Ninja" OR ${CMAKE_GENERATOR} STREQUAL "Unix Makefiles"))
  # CMAKE_BUILD_TYPE should be specified when using Ninja or Unix Makefiles.
  set(CMAKE_BUILD_TYPE Release)
  message(WARNING "No CMAKE_BUILD_TYPE value selected, using ${CMAKE_BUILD_TYPE}")
endif()

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

if(BUILD_SHARED_LIBS)
	set(ANITOMY_LIB_TYPE SHARED)
else()
	set(ANITOMY_LIB_TYPE STATIC)
endif()

add_library(Anitomy ${ANITOMY_LIB_TYPE} ${ANITOMY_SOURCES})
