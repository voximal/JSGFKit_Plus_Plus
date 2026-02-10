cmake_minimum_required(VERSION 3.0)

project(jsgfkitxx
	VERSION 1.1
	LANGUAGES CXX
)

# Build options
option(BUILD_SHARED_LIBS "Build using shared libraries" ON)
option(BUILD_TESTS "Build tests" ON)

# Set C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Source files
set(JSGF_KIT_XX_SOURCES 
    src/Grammar.cpp
    src/Expansion.cpp 
    src/Rule.cpp 
    src/MatchInfo.cpp 
    src/MatchResult.cpp 
    src/Tag.cpp                
    src/UnparsedSection.cpp 
    src/KleeneStar.cpp 
    src/PlusOperator.cpp            
    src/OptionalGrouping.cpp 
    src/RequiredGrouping.cpp 
    src/Sequence.cpp     
    src/AlternativeSet.cpp 
    src/Token.cpp 
    src/RuleReference.cpp
)

# Header files
file(GLOB KIT_HEADERS "include/jsgfkitxx/*.hpp")

# Create library
if(BUILD_SHARED_LIBS)
  add_library(jsgfkitxx SHARED ${JSGF_KIT_XX_SOURCES})
  # Set library version
  set_target_properties(jsgfkitxx PROPERTIES
    VERSION 1.1.0
    SOVERSION 1
  )
else()
  add_library(jsgfkitxx STATIC ${JSGF_KIT_XX_SOURCES})
endif()

# Set include directories
target_include_directories(jsgfkitxx PUBLIC 
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

# Include GNUInstallDirs for standard installation paths
include(GNUInstallDirs)

# Configure .pc file
set(prefix "${CMAKE_INSTALL_PREFIX}")
set(exec_prefix "${CMAKE_INSTALL_PREFIX}")
set(libdir "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}")
set(includedir "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_INCLUDEDIR}")

configure_file(jsgfkitxx.pc.in jsgfkitxx.pc @ONLY)

# Install library
install(TARGETS jsgfkitxx
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

# Install headers
install(FILES ${KIT_HEADERS} 
	DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/jsgfkitxx
)

# Install pkg-config file
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/jsgfkitxx.pc 
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig
)

# Build Tests
if(BUILD_TESTS)
	enable_testing()
	add_subdirectory(test)
	add_test(NAME ParseTest COMMAND test/parsetest)
	add_test(NAME BatchTest COMMAND test/BatchTest/test.sh)
	add_test(NAME ExpansionTypeTest COMMAND test/expansiontypetest)
endif()

# Print install information
message(STATUS "")
message(STATUS "jsgfkitxx Configuration:")
message(STATUS "  Build Type: ${CMAKE_BUILD_TYPE}")
message(STATUS "  Shared Library: ${BUILD_SHARED_LIBS}")
message(STATUS "  Build Tests: ${BUILD_TESTS}")
message(STATUS "  Install Prefix: ${CMAKE_INSTALL_PREFIX}")
message(STATUS "  Library Destination: ${CMAKE_INSTALL_LIBDIR}")
message(STATUS "  Headers Destination: ${CMAKE_INSTALL_INCLUDEDIR}/jsgfkitxx")
message(STATUS "  pkg-config Destination: ${CMAKE_INSTALL_LIBDIR}/pkgconfig")
message(STATUS "")
