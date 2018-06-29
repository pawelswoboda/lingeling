project(lingeling)

cmake_minimum_required(VERSION 2.8.12)

add_definitions(-Wall -O3 -DNLGLOG -DNDEBUG -DNCHKSOL -DNLGLDRUPLIG -DNLGLYALSAT -DNLGLFILES -DNLGLDEMA)

execute_process(COMMAND uname -srmn OUTPUT_VARIABLE OS_VERSION)
string(REGEX REPLACE "\n$" "" OS_VERSION "${OS_VERSION}")
string(TIMESTAMP COMPILE_DATE "%m.%d.%Y %H:%M:%S CEST")
execute_process(COMMAND ${CMAKE_CXX_COMPILER} --version COMMAND head -1 OUTPUT_VARIABLE COMPILER_VERSION)
string(REGEX REPLACE "\n$" "" COMPILER_VERSION "${COMPILER_VERSION}")
get_directory_property( LGL_CFLAGS DIRECTORY ${CMAKE_SOURCE_DIR} COMPILE_DEFINITIONS )
string(REGEX REPLACE ";" " -" LGL_CFLAGS "${LGL_CFLAGS}")
string(REGEX REPLACE "^N" "-N" LGL_CFLAGS "${LGL_CFLAGS}")
message(STATUS ${LGL_CFLAGS})

configure_file(lglcfg.h.in lglcfg.h)
configure_file(lglcflags.h.in lglcflags.h)

include_directories(${CMAKE_CURRENT_BINARY_DIR})

#set(AIGER "")

add_library(lgl lglib lglbnr lgldimacs lglopts)

add_library(lingeling INTERFACE)
target_link_libraries(lingeling INTERFACE lglmain lgl)
target_include_directories(lingeling INTERFACE .)

add_library(lglmain STATIC lglmain.c lglib.h)
add_library(plingeling plingeling.c lglib.h)
target_link_libraries(plingeling lgl pthread) 
add_library(treengeling treengeling.c lglib.h)
target_link_libraries(treengeling lgl pthread) 
add_library(ilingeling ilingeling.c lglib.h)
target_link_libraries(ilingeling lgl pthread) 
#add_library(blimc blimc.c lglib.h ${AIGER}/aiger.h)
#target_link_libraries(blimc lgl) 
add_library(lglmbt lglmbt.c lglib.h)
target_link_libraries(lglmbt lgl) 
add_library(lgluntrace lgluntrace.c lglib.h)
target_link_libraries(lgluntrace lgl)
add_library(lglddtrace lglddtrace.c lglib.h)
target_link_libraries(lglddtrace lgl)

add_library(lglib STATIC lglib.c lglib.h lglconst.h lgloptl.h lglopts.c lglopts.h)
add_library(lgldimacs STATIC lgldimacs.c lgldimacs.h)
add_library(lglbnr STATIC lglbnr.c lglcfg.h lglcflags.h makefile)
add_library(lglopts STATIC lglopts.c lglopts.h lglconst.h lgloptl.h)
