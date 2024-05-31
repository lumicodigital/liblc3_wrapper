cmake_minimum_required(VERSION 3.12)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_COMPILER "gcc")
set(CMAKE_CXX_COMPILER "g++")

project(liblc3_wrapper VERSION 1.0)

set(liblc3_root ${CMAKE_CURRENT_LIST_DIR}/liblc3)
set(liblc3_src ${liblc3_root}/src)
set(liblc3_include ${liblc3_root}/include)

set(liblc3_srcs ${liblc3_src}/attdet.c 
                ${liblc3_src}/bits.c 
                ${liblc3_src}/bwdet.c
                ${liblc3_src}/energy.c
                ${liblc3_src}/lc3.c
                ${liblc3_src}/ltpf.c
                ${liblc3_src}/mdct.c
                ${liblc3_src}/plc.c
                ${liblc3_src}/sns.c
                ${liblc3_src}/spec.c
                ${liblc3_src}/tables.c
                ${liblc3_src}/tns.c)

add_library(liblc3_wrapper SHARED ${liblc3_srcs} ${CMAKE_CURRENT_LIST_DIR}/decoder.c)
set_target_properties(liblc3_wrapper PROPERTIES SUFFIX ".so")

# Specify private include directories
target_include_directories(liblc3_wrapper PRIVATE
  ${CMAKE_SOURCE_DIR}/liblc3/include
)

# Specify public include directories
target_include_directories(liblc3_wrapper PUBLIC
  $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}>
)

set_target_properties(liblc3_wrapper PROPERTIES
  LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib
)

target_compile_options(liblc3_wrapper PRIVATE -std=c11 -Wall -Wdouble-promotion -O2 -ffast-math)