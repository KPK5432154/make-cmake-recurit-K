cmake_minimum_required(VERSION 3.16)

project(calculator LANGUAGES C)

# TODO: create an executable target named calculator
# Hint: sources are in src/main.c and src/calculator.c
add_executable(calculator src/main.c src/calculator.c )

# TODO: add the include directory for the calculator target
target_include_directories(calculator PRIVATE include)
