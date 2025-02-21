cmake_minimum_required(VERSION 2.8)

project(ssg)
include_directories(${PROJECT_SOURCE_DIR}/include)
list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake/module)

# Boost
find_package(Boost REQUIRED)
if (Boost_FOUND)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${Boost_C_FLAGS}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${Boost_CXX_FLAGS}")
    include_directories(${Boost_INCLUDE_DIRS})
else()
    message(FATAL_ERROR "Boost dynamic-bitset is required")
endif()

# OpenMP
find_package(OpenMP REQUIRED)
if (OPENMP_FOUND)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
else()
    message(FATAL_ERROR "OpenMP is required")
endif()

# Python binding
add_subdirectory(third_party/pybind11)

# Compile flags
add_definitions(-std=c++11 -O3 -lboost -ltcmalloc_minimal -march=native -Wall -DINFO -g3)

# Add define
add_definitions(-DEVAL_RECALL)
#add_definitions (-DGET_DIST_COMP) # [ARC-SJ] Enable to measure Speedup
#add_definitions (-DPROFILE) # [ARC-SJ] Enable to measure the performance bottleneck
#add_definitions(-DMMAP_HUGETLB) # [ARC-SJ] Enable to allocate huge page instead of malloc
#add_definitions (-DTHREAD_LATENCY) # [ARC-SJ] Enable to measure mean latency & 99% latency

# Source codes
add_subdirectory(src)
add_subdirectory(tests)
