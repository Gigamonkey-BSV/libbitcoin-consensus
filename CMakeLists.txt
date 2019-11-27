cmake_minimum_required(VERSION 3.1...3.14)

# Back compatibility for VERSION range
if(${CMAKE_VERSION} VERSION_LESS 3.12)
    cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
endif()

project(libbitcoin-consensus 	VERSION 4.0.0
		DESCRIPTION "Bitcoin Consensus Library (optional)"
		LANGUAGES CXX)

# Set cmake as import path for Find*.cmake files
set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})

### Require out-of-source builds
file(TO_CMAKE_PATH "${PROJECT_BINARY_DIR}/CMakeLists.txt" LOC_PATH)
if(EXISTS "${LOC_PATH}")
    message(FATAL_ERROR "You cannot build in a source directory (or any directory with a CMakeLists.txt file). Please make a build subdirectory. Feel free to remove CMakeCache.txt and CMakeFiles.")
endif()

# Include libraries

# Boost settings
set(Boost_LIB_PREFIX            "lib"       CACHE STRING "")
set(Boost_USE_MULTITHREADED     ON          CACHE BOOL "") # '-mt' flag
set(Boost_USE_STATIC_LIBS       OFF          CACHE BOOL "")
set(Boost_USE_STATIC_RUNTIME    OFF          CACHE BOOL "") # '-s' tag
set(Boost_USE_DEBUG_RUNTIME     ON          CACHE BOOL "") # '-g' tag
set(Boost_COMPILER              "-mgw49"    CACHE STRING "")

# Include Boost
find_package(Boost 1.62.0 COMPONENTS system unit_test_framework REQUIRED)

if(Boost_FOUND)

    message(STATUS "Boost_INCLUDE_DIRS: ${Boost_INCLUDE_DIRS}")
    message(STATUS "Boost_LIBRARIES: ${Boost_LIBRARIES}")
    message(STATUS "Boost_VERSION: ${Boost_VERSION}")

    include_directories(${Boost_INCLUDE_DIRS})
    add_definitions("-DHAS_BOOST")

endif()



# Find LibBitcoin
#set(ENV{PKG_CONFIG_PATH} "/usr/local/lib/pkgconfig/:$ENV{PKG_CONFIG_PATH}")

#find_package(PkgConfig REQUIRED)
#pkg_check_modules(LIB_BITCOIN REQUIRED libbitcoin-system)
#include_directories(${LIB_BITCOIN_INCLUDE_DIRS})

# Find libsecp256k1
find_package(Secp256k1 REQUIRED)
if(SECP256K1_FOUND)
	message(STATUS "Secp256k1_INCLUDE_DIR: ${Secp256k1_INCLUDE_DIR}")
	message(STATUS "Secp256k1_LIBRARY: ${Secp256k1_LIBRARY}")
	
endif()
## Enable testing
include(CTest)

if(PACKAGE_TESTS)
    enable_testing()	
endif()


#  Add Library source files here

ADD_LIBRARY(libbitcoin-consensus STATIC
src/consensus/consensus.cpp
)        

target_include_directories(libbitcoin-consensus PUBLIC src/consensus/)

# Set C++ version
target_compile_features(libbitcoin-consensus PUBLIC cxx_std_17)
set_target_properties(libbitcoin-consensus PROPERTIES CXX_EXTENSIONS OFF)

