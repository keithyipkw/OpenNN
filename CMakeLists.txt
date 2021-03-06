
cmake_minimum_required(VERSION 2.6)
project(OpenNN)

option (OPENNN_MPI "Use MPI" FALSE)
option (OPENNN_OMP "Use OpenMP" FALSE)
option (OPENNN_EXAMPLES "Build examples" TRUE)
option (OPENNN_TESTS "Build tests" TRUE)


set (CMAKE_CXX_STANDARD 11)
SET(CPACK_GENERATOR "TGZ")

if(__OPENNN_MPI__)
   message(FATAL_ERROR "Please use OPENNN_MPI instead of __OPENNN_MPI__.")
endif()

if(__OPENNN_OMP__)
   message(FATAL_ERROR "Please use OPENNN_OMP instead of __OPENNN_OMP__.")
endif()

if(OPENNN_MPI)
   find_package(MPI)
   if(MPI_FOUND)
        message("Using MPI")
        set(CMAKE_CXX_COMPILER mpicxx)
        set(CMAKE_C_COMPILER mpicc)
        set(CMAKE_CXX_COMPILE_FLAGS "${CMAKE_CXX_COMPILE_FLAGS} ${MPI_COMPILE_FLAGS}")
        set(CMAKE_CXX_LINK_FLAGS "${CMAKE_CXX_LINK_FLAGS} ${MPI_LINK_FLAGS}")
        add_definitions(-D__OPENNN_MPI__ )
    endif()
endif()


if(OPENNN_OMP)
    message("Using OpenMP")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fopenmp")#${OpenMP_C_FLAGS}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fopenmp")#${OpenMP_CXX_FLAGS}")
    set(CMAEK_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

# Uncomment next line to compile without using C++11
#add_definitions(-D__Cpp11__)

add_subdirectory(tinyxml2)
add_subdirectory(eigen)
add_subdirectory(opennn)
include_directories(opennn)

if(OPENNN_EXAMPLES)
    add_subdirectory(examples)
    add_subdirectory(blank)
endif()

if(OPENNN_TESTS)
    add_subdirectory(tests)
endif()

include(CPack)
