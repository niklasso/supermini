cmake_minimum_required(VERSION 2.6 FATAL_ERROR)

#--------------------------------------------------------------------------------------------------
# Build types:

set(CMAKE_CXX_FLAGS_PROFILE "-O3 -g -DNDEBUG -pg")
set(CMAKE_C_FLAGS_PROFILE "-O3 -g -DNDEBUG -pg")
set(CMAKE_EXE_LINKER_FLAGS_PROFILE "-pg")

set(CMAKE_CXX_FLAGS_DEBUGFAST "-O3 -g")
set(CMAKE_C_FLAGS_DEBUGFAST "-O3 -g")

set(CMAKE_CXX_FLAGS_DEBUG "-O0 -g")
set(CMAKE_C_FLAGS_DEBUG "-O0 -g")
#set(CMAKE_CXX_FLAGS_RELEASE "-O3 -g -DNDEBUG")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -g -DNDEBUG -fomit-frame-pointer")
set(CMAKE_C_FLAGS_RELEASE "-O3 -g -DNDEBUG -fomit-frame-pointer")

if(DEFINED CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE ${CMAKE_BUILD_TYPE} CACHE STRING
    "Choose the type of build, options are: None(CMAKE_CXX_FLAGS or CMAKE_C_FLAGS used) Debug DebugFast Profile Release.")
else()
  set(CMAKE_BUILD_TYPE Release CACHE STRING 
    "Choose the type of build, options are: None(CMAKE_CXX_FLAGS or CMAKE_C_FLAGS used) Debug DebugFast Profile Release.")
endif()

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

#--------------------------------------------------------------------------------------------------
# Configurable options:

option(STATIC_BINARIES "Link binaries statically." ON)
option(USE_SORELEASE   "Use SORELEASE in shared library filename." ON)


#--------------------------------------------------------------------------------------------------
# Compile flags:

if(STATIC_BINARIES)
  set(CMAKE_EXE_LINKER_FLAGS "-static")
  #set_property(GLOBAL PROPERTY LINK_SEARCH_START_STATIC TRUE) // Available in the future?
endif()

add_definitions(-D__STDC_FORMAT_MACROS -D__STDC_LIMIT_MACROS)
#add_definitions(-Wall -Wno-parentheses)
#add_definitions(-Wall -Werror)
add_definitions(-Wall)
add_definitions(-DMINISAT_CONSTANTS_AS_MACROS)

#--------------------------------------------------------------------------------------------------
# Submodules: (comment the modules you don't have or need)

project(all)

#add_subdirectory(minisat-examples)
#add_subdirectory(minisat-tests)
#add_subdirectory(msup)

if(EXISTS "${CMAKE_SOURCE_DIR}/minisat/CMakeLists.txt")
  add_subdirectory(minisat)
endif()

if(EXISTS "${CMAKE_SOURCE_DIR}/minisatp/CMakeLists.txt")
  add_subdirectory(minisatp)
endif()

if(EXISTS "${CMAKE_SOURCE_DIR}/mcl/CMakeLists.txt")
  add_subdirectory(mcl)
endif()

if(EXISTS "${CMAKE_SOURCE_DIR}/tip/CMakeLists.txt")
  add_subdirectory(tip)
endif()
