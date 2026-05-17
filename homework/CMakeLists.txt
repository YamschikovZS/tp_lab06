cmake_minimum_required(VERSION 3.10)
project(solver LANGUAGES CXX)

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

target_compile_definitions(solver_lib PRIVATE
    _USE_MATH_DEFINES
)
target_compile_options(solver_lib PRIVATE
    -include cmath
)

add_executable(solver
    solver_application/equation.cpp
)

target_link_libraries(solver PRIVATE formatter_ex_lib solver_lib)

if(UNIX)
    target_link_libraries(solver PRIVATE m)
endif()
