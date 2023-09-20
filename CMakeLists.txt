
cmake_minimum_required(VERSION 3.12)
project(remixdb C)


##--------------------- Version file ---------------------------------------##

# Specified the language standard
set(CMAKE_C_STANDARD 99)
set(CMAKE_CXX_STANDARD 17)

##--------------------- Compile Options ------------------------------------##
# Configure compile options
add_compile_options(-Wall) # -Wextra -pedantic -Wshadow -Wstrict-prototypes --all-warnings -Wconversion # -Werror
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -pipe -std=c99")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pipe -std=c++17")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -march=native -mtune=native")

# Set build type
# set(CMAKE_BUILD_TYPE Debug)  # Use `cmake -DCMAKE_BUILD_TYPE=Debug` more better
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")

# Compile options for Debug variant
set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -g -O0")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g -O0")

# Compile options for Release variant
set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -O2")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O2")

set(THREADS_PREFER_PTHREAD_FLAG ON)
add_definitions(-DGFLAGS=1)

set(CMAKE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}")
# set(src_files "lib.c kv.c wh.c blkio.c sst.c xdb.c")
add_library(remixdb STATIC lib.c kv.c wh.c blkio.c sst.c xdb.c)
set_target_properties(remixdb PROPERTIES LINKER_LANGUAGE C)
target_link_libraries(remixdb PUBLIC m rt pthread)

add_executable(xdbdemo ${CMAKE_SOURCE_DIR}/xdbdemo.c)
target_link_libraries(xdbdemo PRIVATE m rt pthread remixdb)
