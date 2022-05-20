include(CMakeDependentOption)
set( CMAKE_VERBOSE_MAKEFILE on )
# probably do not need all these solvers


if(NOT DEFINED ENV{NEKTAR_BUILD_PATH})
    message(FATAL_ERROR "NEKTAR_BUILD_PATH is not set")
endif(NOT DEFINED ENV{NEKTAR_BUILD_PATH})


cmake_minimum_required(VERSION 3.12)

set (CMAKE_CXX_STANDARD 11)

# set the project name
project(ADRSolver CXX)


FIND_PACKAGE(Nektar++ REQUIRED)
ADD_DEFINITIONS(${NEKTAR++_DEFINITIONS})
#  If you're on a system with CMake >=3.12, also add this ADD_COMPILE_DEFINITIONS line:
ADD_COMPILE_DEFINITIONS(${NEKTAR++_GENERATED_DEFINITIONS})
INCLUDE_DIRECTORIES(${NEKTAR++_INCLUDE_DIRS} ${NEKTAR++_TP_INCLUDE_DIRS})
INCLUDE_DIRECTORIES(./src)
LINK_DIRECTORIES(${NEKTAR++_LIBRARY_DIRS} ${NEKTAR++_TP_LIBRARY_DIRS})


find_package(MPI REQUIRED)
include_directories(${MPI_INCLUDE_PATH})


file(GLOB ADRSOLVER_OBJS "${NEKTAR_BUILD_PATH}/solvers/ADRSolver/CMakeFiles/ADRSolver.dir/EquationSystems/*.o")
SET(ADRSolverSource ./src/ADRSolver/ADRSolver.cpp)
add_executable(ADRSolver ${ADRSolverSource})
set_target_properties(ADRSolver PROPERTIES COMPILE_FLAGS "${MPI_COMPILE_FLAGS}")
TARGET_LINK_LIBRARIES(ADRSolver ${ADRSOLVER_OBJS} ${NEKTAR++_LIBRARIES} ${NEKTAR++_TP_LIBRARIES} ${MPI_LIBRARIES})


SET(CompressibleFlowSolverSource ./src/CompressibleFlowSolver/CompressibleFlowSolver.cpp)
add_executable(CompressibleFlowSolver ${CompressibleFlowSolverSource})
set_target_properties(CompressibleFlowSolver PROPERTIES COMPILE_FLAGS "${MPI_COMPILE_FLAGS}")
file(GLOB CFSSOLVER_OBJS "${NEKTAR_BUILD_PATH}/solvers/CompressibleFlowSolver/CMakeFiles/*/*/*.o")
TARGET_LINK_LIBRARIES(CompressibleFlowSolver ${CFSSOLVER_OBJS} ${NEKTAR++_LIBRARIES} ${NEKTAR++_TP_LIBRARIES} ${MPI_LIBRARIES})
































