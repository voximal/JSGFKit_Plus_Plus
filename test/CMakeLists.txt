add_executable(parse-test ParseAndOutputJSGF.cpp)
target_link_libraries(parse-test PRIVATE jsgfkitxx)
target_include_directories(parse-test
    PRIVATE ${CMAKE_SOURCE_DIR}/include
)

add_executable(expansion-type-test ExpansionTypeTest.cpp)
target_link_libraries(expansion-type-test PRIVATE jsgfkitxx)
target_include_directories(expansion-type-test 
    PRIVATE ${CMAKE_SOURCE_DIR}/include
)

add_executable(optional-test IsOptionalTest.cpp)
target_link_libraries(optional-test PRIVATE jsgfkitxx)
target_include_directories(optional-test 
    PRIVATE ${CMAKE_SOURCE_DIR}/include
)

add_executable(jsgf-match MatchingTest.cpp)
target_link_libraries(jsgf-match PRIVATE jsgfkitxx)
target_include_directories(jsgf-match 
    PRIVATE ${CMAKE_SOURCE_DIR}/include
)
