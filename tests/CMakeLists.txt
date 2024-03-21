cmake_minimum_required(VERSION 3.25 FATAL_ERROR)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../include)

# Colecta los tests
file(GLOB TESTS_FILES ${CMAKE_CURRENT_SOURCE_DIR}/unit/*.c)
file(GLOB SRC_FILES ${CMAKE_CURRENT_SOURCE_DIR}/../src/logic.c)

# Linkea con los headers de Unity
target_link_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/external/Unity/src)

# Coverage
if(RUN_COVERAGE)
    message("Coverage enabled")
    find_program(LCOV_PATH lcov)
        if(NOT LCOV_PATH)
        message(FATAL_ERROR "lvoc not found! Aborting...")
    endif()
    
    find_program(GCOV_PATH gcov)
        if(NOT GCOV_PATH)
        message(FATAL_ERROR "gvoc not found! Aborting...")
    endif()

    find_program(GENHTML_PATH genhtml)
        if(NOT GENHTML_PATH)
        message(FATAL_ERROR "genhtml not found! Aborting...")
    endif()

    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -fprofile-arcs -ftest-coverage --coverage")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -lgcov --coverage")
endif()

# Crea el ejecutable del test
add_executable(test_${PROJECT_NAME} ${TESTS_FILES} ${SRC_FILES})

# Linkea con Unity
target_link_libraries(test_${PROJECT_NAME} unity)

# Añade el test
add_test(NAME test_${PROJECT_NAME} COMMAND test_${PROJECT_NAME})
