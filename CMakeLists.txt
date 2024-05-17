cmake_minimum_required(VERSION 3.8)

# set the project name
project(cs2_rcs LANGUAGES C CXX)

# add the executable
add_executable(cs2_rcs
 cs2_rcs.cpp MemMan.cpp Game.cpp
 )