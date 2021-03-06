cmake_minimum_required(VERSION 2.6)

project (libcrush)
set(CMAKE_BUILD_TYPE Release)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
set (crush_SRCS hash.c mapper.c crush.c client.cc)

add_library(crush SHARED ${crush_SRCS})
target_link_libraries(crush rados)

add_executable(example-simple example-simple.cc ${crush_SRCS})
target_link_libraries(example-simple rados)
add_executable(example-crush-tester example-crush-tester.cc ${crush_SRCS})
target_link_libraries(example-crush-tester crush rados)

set(CMAKE_CXX_COMPILER "g++")
set(CMAKE_CXX_FLAGS_DEBUG "-O0")
set(CMAKE_CXX_FLAGS_RELEASE "-g -O0 -finline-limit=1000 --std=c++11")
set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR})

string(TOUPPER ${CMAKE_BUILD_TYPE} BUILD_TYPE)
message(STATUS "CXX_FLAGS = " ${CMAKE_CXX_FLAGS} " " ${CMAKE_CXX_FLAGS_${BUILD_TYPE}})

install(TARGETS crush DESTINATION lib)
file(GLOB HEADERS "*.h")
install(FILES ${HEADERS} DESTINATION include/crush)

