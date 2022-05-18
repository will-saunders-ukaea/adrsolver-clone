include(CMakeDependentOption)

# probably do not need all these solvers
SET(ADRSolverSource
    ./src/ADRSolver/ADRSolver.cpp
    ./src/ADRSolver/EquationSystems/EigenValuesAdvection.cpp
    ./src/ADRSolver/EquationSystems/Helmholtz.cpp
    ./src/ADRSolver/EquationSystems/Laplace.cpp
    ./src/ADRSolver/EquationSystems/Poisson.cpp
    ./src/ADRSolver/EquationSystems/Projection.cpp
    ./src/ADRSolver/EquationSystems/SteadyAdvectionDiffusion.cpp
    ./src/ADRSolver/EquationSystems/SteadyAdvectionDiffusionReaction.cpp
    ./src/ADRSolver/EquationSystems/UnsteadyAdvection.cpp
    ./src/ADRSolver/EquationSystems/UnsteadyAdvectionDiffusion.cpp
    ./src/ADRSolver/EquationSystems/UnsteadyDiffusion.cpp
    ./src/ADRSolver/EquationSystems/UnsteadyInviscidBurger.cpp
    ./src/ADRSolver/EquationSystems/UnsteadyReactionDiffusion.cpp
    ./src/ADRSolver/EquationSystems/UnsteadyViscousBurgers.cpp
)

cmake_minimum_required(VERSION 3.10)

# set the project name
project(ADRSolver)

# add the executable
add_executable(ADRSolver ${ADRSolverSource})

FIND_PACKAGE(Nektar++ REQUIRED)
ADD_DEFINITIONS(${NEKTAR++_DEFINITIONS})
#  If you're on a system with CMake >=3.12, also add this ADD_COMPILE_DEFINITIONS line:
ADD_COMPILE_DEFINITIONS(${NEKTAR++_GENERATED_DEFINITIONS})
INCLUDE_DIRECTORIES(${NEKTAR++_INCLUDE_DIRS} ${NEKTAR++_TP_INCLUDE_DIRS})
INCLUDE_DIRECTORIES(./src)
LINK_DIRECTORIES(${NEKTAR++_LIBRARY_DIRS} ${NEKTAR++_TP_LIBRARY_DIRS})
TARGET_LINK_LIBRARIES(ADRSolver ${NEKTAR++_LIBRARIES} ${NEKTAR++_TP_LIBRARIES})


