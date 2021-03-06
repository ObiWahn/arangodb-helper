cmake_minimum_required(VERSION 2.8.12)
project(perftest CXX C)
set(CMAKE_CXX_STANDARD 14)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14")

find_package(Boost COMPONENTS program_options filesystem system REQUIRED)
find_package(Threads REQUIRED)
find_package(ZLIB REQUIRED)
find_package(BZip2 REQUIRED)

add_executable(perf
    src/main.cpp
    src/wiredtiger.cpp
)
target_link_libraries(perf PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/rocksdb/librocksdb.a
    ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/wiredtiger-build/.libs/libwiredtiger.a
    ${ZLIB_LIBRARIES}
    ${BZIP2_LIBRARIES}
    snappy
    -ldl
    ${Boost_LIBRARIES}
    ${CMAKE_THREAD_LIBS_INIT}
)

target_include_directories(perf PRIVATE
    src
    ${Boost_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/
    ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/rocksdb/include
)

