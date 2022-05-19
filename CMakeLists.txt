include(CMakeDependentOption)
set( CMAKE_VERBOSE_MAKEFILE on )
# probably do not need all these solvers


cmake_minimum_required(VERSION 3.12)

# set the project name
project(ADRSolver CXX)

FIND_PACKAGE(Nektar++ REQUIRED)
ADD_DEFINITIONS(${NEKTAR++_DEFINITIONS})
#  If you're on a system with CMake >=3.12, also add this ADD_COMPILE_DEFINITIONS line:
ADD_COMPILE_DEFINITIONS(${NEKTAR++_GENERATED_DEFINITIONS})
INCLUDE_DIRECTORIES(${NEKTAR++_INCLUDE_DIRS} ${NEKTAR++_TP_INCLUDE_DIRS})
INCLUDE_DIRECTORIES(./src)
LINK_DIRECTORIES(${NEKTAR++_LIBRARY_DIRS} ${NEKTAR++_TP_LIBRARY_DIRS})







# Original link line
# TARGET_LINK_LIBRARIES(ADRSolver ${NEKTAR++_LIBRARIES} ${NEKTAR++_TP_LIBRARIES})



file(GLOB ADRSOLVER_OBJS "${NEKTAR_BUILD_PATH}/solvers/ADRSolver/CMakeFiles/ADRSolver.dir/EquationSystems/*.o")
#file(GLOB ADRSOLVER_OBJS "${NEKTAR_BUILD_PATH}/solvers/ADRSolver/CMakeFiles/ADRSolver.dir/EquationSystems/*.o")
#SET_SOURCE_FILES_PROPERTIES(
#  ${ADRSOLVER_OBJS}
#  PROPERTIES
#  EXTERNAL_OBJECT true
#  GENERATED true
#)
#add_library(libadrsolver OBJECT IMPORTED ${ADRSOLVER_OBJS})

#TARGET_LINK_LIBRARIES(
#    ADRSolver 
#    ${NEKTAR++_LIBRARIES} 
#    ${NEKTAR++_TP_LIBRARIES}
#    ${ADRSOLVER_OBJS}
#)



SET(ADRSolverSource
    ./src/ADRSolver/ADRSolver.cpp
)
add_executable(ADRSolver ${ADRSolverSource})


# TARGET_LINK_LIBRARIES(ADRSolver ${NEKTAR++_LIBRARIES} ${NEKTAR++_TP_LIBRARIES} libadrsolver)
TARGET_LINK_LIBRARIES(ADRSolver ${NEKTAR++_LIBRARIES} ${NEKTAR++_TP_LIBRARIES} ${ADRSOLVER_OBJS})


SET(CompressibleFlowSolverSource
    ./src/CompressibleFlowSolver/CompressibleFlowSolver.cpp
)
add_executable(CompressibleFlowSolver ${CompressibleFlowSolverSource})


file(GLOB CFSSOLVER_OBJS "${NEKTAR_BUILD_PATH}/solvers/CompressibleFlowSolver/CMakeFiles/*/*/*.o")


TARGET_LINK_LIBRARIES(CompressibleFlowSolver ${NEKTAR++_LIBRARIES} ${NEKTAR++_TP_LIBRARIES} ${CFSSOLVER_OBJS})
































