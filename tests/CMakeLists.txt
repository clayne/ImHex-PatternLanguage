cmake_minimum_required(VERSION 3.16)

project(pattern_language_tests)


# Add new tests here #
set(AVAILABLE_TESTS
        DocComments
        Strings
        Placement
        Structs
        Unions
        Enums
        Literals
        Padding
        Matching
        SucceedingAssert
        FailingAssert
        Bitfields
        ReversedBitfields
        Math
        RValues
        Namespaces
        Include
        Import
        ExtraSemicolon
        Pointers
        Arrays
        NestedStructs
        Attributes
        Pragmas
        PragmasFail
        Format
        RValuesAssignmentInStruct
)


add_executable(pattern_language_tests
    source/main.cpp
    source/tests.cpp
        include/test_patterns/test_pattern_import.hpp
)


# ---- No need to change anything from here downwards unless you know what you're doing ---- #

target_include_directories(pattern_language_tests PRIVATE include)
target_link_libraries(pattern_language_tests PRIVATE libpl libpl-gen fmt::fmt-header-only)

set_target_properties(pattern_language_tests PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

add_custom_command(TARGET pattern_language_tests
        POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different "${CMAKE_CURRENT_SOURCE_DIR}/test_data" ${CMAKE_BINARY_DIR}/bin)

add_custom_command(TARGET pattern_language_tests
        POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different "${CMAKE_CURRENT_SOURCE_DIR}/files/export/json.json" ${CMAKE_BINARY_DIR}/bin/files/export/json.json)

add_custom_command(TARGET pattern_language_tests
        POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different "${CMAKE_CURRENT_SOURCE_DIR}//files/export/yaml.yml" ${CMAKE_BINARY_DIR}/bin/files/export/yaml.yml)

foreach (test IN LISTS AVAILABLE_TESTS)
    add_test(NAME "PatternLanguage/${test}" COMMAND pattern_language_tests "${test}" WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
endforeach ()
add_dependencies(unit_tests ${PROJECT_NAME})