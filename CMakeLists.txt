cmake_minimum_required(VERSION 3.25 FATAL_ERROR)

project(
    "MyFirstProjectInCMake"
    VERSION 1.1.0
    DESCRIPTION "CMake C Project template for Operative Systems II"
    LANGUAGES C
)

# Habilita el testing y añade los tests
include(CTest)

set(CMAKE_C_STARDARD 17)

# Añade los directorios "include" donde los headers estan ubicados
include_directories(include)

# Para automatizar las descargas de dependencias externas
include(FetchContent)

# Setea una variable con la url de Unity
set(UNITY_GIT_URL "https://github.com/ThrowTheSwitch/Unity.git")

# Habilita debug
set(FETCHCONTENT_QUIET OFF)

FetchContent_Declare(
    Unity
    GIT_REPOSITORY ${UNITY_GIT_URL}
    SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/external/Unity
    UPDATE_COMMAND "" # Deshabilita el comando update 
)

# Asegura que todas las dependencias han sido descargadas correctamente
FetchContent_MakeAvailable(Unity)

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

if(RUN_TESTS EQUAL 1 OR RUN_COVERAGE EQUAL 1)
    add_subdirectory(tests)
endif()    