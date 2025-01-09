cmake_minimum_required(VERSION 3.10)
project(uds LANGUAGES C)

# Define the source and header files
#file(GLOB_RECURSE UDS_SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/src/*.c)
#file(GLOB_RECURSE UDS_HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/include/*.h)

set(UDS_SOURCES PUBLIC 
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/deps/bitfield-c/src/bitfield/8byte.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/deps/bitfield-c/src/bitfield/bitarray.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/deps/bitfield-c/src/bitfield/bitfield.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/deps/bitfield-c/src/canutil/read.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/deps/bitfield-c/src/canutil/write.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/deps/bitfield-c/tests/8byte_tests.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/deps/bitfield-c/tests/bitfield_tests.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/deps/bitfield-c/tests/read_tests.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/deps/bitfield-c/tests/write_tests.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/src/isotp/allocate.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/src/isotp/isotp.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/src/isotp/receive.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/src/isotp/send.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/tests/common.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/tests/test_allocate.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/tests/test_core.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/tests/test_receive.c
	${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/tests/test_send.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/uds/extras.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/uds/uds.c
)


# Define common include directories
set(COMMON_INCLUDE_UDS PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c
    ${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/deps/bitfield-c/src
    ${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/deps/bitfield-c/src/bitfield
    ${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/deps/bitfield-c/src/canutil
    ${CMAKE_CURRENT_SOURCE_DIR}/deps/isotp-c/src
    ${CMAKE_CURRENT_SOURCE_DIR}/src/uds  # Add the src directory for uds/
)

# Add the UDS library as a static library
add_library(uds STATIC ${UDS_SOURCES})

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

# # Post-build command to display file size
# add_custom_command(TARGET uds
#     POST_BUILD
#     COMMAND ${CMAKE_COMMAND} -E size ${CMAKE_CURRENT_BINARY_DIR}/lib/libuds.a
# )

# Install target (optional)
#install(TARGETS uds
#    ARCHIVE DESTINATION lib
#    PUBLIC_HEADER DESTINATION include
#)
