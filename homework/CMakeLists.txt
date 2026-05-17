cmake_minimum_required(VERSION 3.10)
project(solver)
set(SOLVER_VERSION_MAJOR 0)
set(SOLVER_VERSION_MINOR 1)
set(SOLVER_VERSION_PATCH 0)
set(SOLVER_VERSION_TWEAK 8)
set(SOLVER_VERSION
	${SOLVER_VERSION_MAJOR}.${SOLVER_VERSION_MINOR}.${SOLVER_VERSION_PATCH}.${SOLVER_VERSION_TWEAK})
set(SOLVER_VERSION_STRING "v${SOLVER_VERSION}")

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter_lib STATIC
    formatter_lib/formatter.cpp
    formatter_lib/formatter.h
)
target_include_directories(formatter_lib PUBLIC formatter_lib)

add_library(formatter_ex_lib STATIC
    formatter_ex_lib/formatter_ex.cpp
    formatter_ex_lib/formatter_ex.h
)
target_include_directories(formatter_ex_lib PUBLIC formatter_ex_lib)
target_link_libraries(formatter_ex_lib PUBLIC formatter_lib)

add_library(solver_lib STATIC
    solver_lib/solver.cpp
    solver_lib/solver.h
)
target_include_directories(solver_lib PUBLIC solver_lib)

add_executable(solver
    solver_application/equation.cpp
)

target_link_libraries(solver PRIVATE formatter_ex_lib solver_lib)

if(UNIX)
    target_link_libraries(solver PRIVATE m)
endif()

include(CPackConfig.cmake)
