cmake_minimum_required(VERSION 3.25 FATAL_ERROR)

project(
    "MyFirstProjectInCMake"
    VERSION 1.0.0
    DESCRIPTION "CMake C Project template for Operative Systems II"
    LANGUAGES C
)

set(CMAKE_C_STARDARD 17)

# Añade los directorios "include" donde los headers estan ubicados
include_directories(include)

# Le indica donde buscar el codigo fuente
file(GLOB_RECURSE SOURCES "src/*.c")

#set(CMAKE_C_FLAGS)

# Crea el ejecutable en base al nombre del proyecto de project() y src de file() 
add_executable(${PROJECT_NAME} ${SOURCES})

# Compila las librerias
add_subdirectory(lib/libdyn)
add_subdirectory(lib/libsta)

# Incluye los headers de las librerias
target_include_directories(${PROJECT_NAME} PUBLIC lib/libdyn/include)
target_include_directories(${PROJECT_NAME} PUBLIC lib/libsta/include)

# Linkea las librerias
target_link_libraries(${PROJECT_NAME} StaticLib DynamicLib)