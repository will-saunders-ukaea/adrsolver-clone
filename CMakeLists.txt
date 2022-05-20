include(CMakeDependentOption)
set( CMAKE_VERBOSE_MAKEFILE on )
# probably do not need all these solvers


if(NOT DEFINED ENV{NEKTAR_BUILD_PATH})
    message(FATAL_ERROR "NEKTAR_BUILD_PATH is not set")
endif(NOT DEFINED ENV{NEKTAR_BUILD_PATH})


cmake_minimum_required(VERSION 3.12)

set (CMAKE_CXX_STANDARD 11)

# set the project name
project(CombinedSolver CXX)


FIND_PACKAGE(Nektar++ REQUIRED)
ADD_DEFINITIONS(${NEKTAR++_DEFINITIONS})
#  If you're on a system with CMake >=3.12, also add this ADD_COMPILE_DEFINITIONS line:
ADD_COMPILE_DEFINITIONS(${NEKTAR++_GENERATED_DEFINITIONS})
INCLUDE_DIRECTORIES(${NEKTAR++_INCLUDE_DIRS} ${NEKTAR++_TP_INCLUDE_DIRS})
INCLUDE_DIRECTORIES(./src)
LINK_DIRECTORIES(${NEKTAR++_LIBRARY_DIRS} ${NEKTAR++_TP_LIBRARY_DIRS})


find_package(MPI REQUIRED)
include_directories(${MPI_INCLUDE_PATH})


file(GLOB CFSSOLVER_OBJS "${NEKTAR_BUILD_PATH}/solvers/CompressibleFlowSolver/CMakeFiles/*/*/*.o")
file(GLOB ADRSOLVER_OBJS "${NEKTAR_BUILD_PATH}/solvers/ADRSolver/CMakeFiles/ADRSolver.dir/EquationSystems/*.o")
SET(CombinedSolverSource ./src/CombinedSolver/CombinedSolver.cpp)
add_executable(CombinedSolver ${CombinedSolverSource})
set_target_properties(CombinedSolver PROPERTIES COMPILE_FLAGS "${MPI_COMPILE_FLAGS}")
TARGET_LINK_LIBRARIES(CombinedSolver ${ADRSOLVER_OBJS} ${CFSSOLVER_OBJS} ${NEKTAR++_LIBRARIES} ${NEKTAR++_TP_LIBRARIES} ${MPI_LIBRARIES})



