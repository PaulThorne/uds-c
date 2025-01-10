cmake_minimum_required(VERSION 3.10)
project(uds LANGUAGES C)

include(AddGitSubmodule.cmake)

# Define the source files
set(UDS_SOURCES 
	${CMAKE_CURRENT_SOURCE_DIR}/src/uds/extras.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/uds/uds.c
)


# Define common include directories
set(COMMON_INCLUDE_UDS
    ${CMAKE_CURRENT_SOURCE_DIR}/src/uds  # Add the src directory for uds/
)

add_git_submodule("deps/isotp-c")


# Add the UDS library as a static library
add_library(uds STATIC ${UDS_SOURCES})

target_link_libraries(uds isotp)

# Include directories for UDS
target_include_directories(uds PUBLIC ${COMMON_INCLUDE_UDS})

# Compiler flags
target_compile_options(uds PRIVATE
    -w                  # Disable all warnings
    -std=c99            # Use C99 standard
    -O0                 # No optimization
)

# Set the output directory for the library
set_target_properties(uds PROPERTIES 
    ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/lib
)

