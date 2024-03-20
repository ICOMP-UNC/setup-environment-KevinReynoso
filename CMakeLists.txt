cmake_minimum_required(VERSION 3.25 FATAL_ERROR)

project(
    "MyFirstProjectInCMake"
    VERSION 1.0.0
    DESCRIPTION "Plantilla de Proyecto en C con CMake para S.O. II" 
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