cmake_minimum_required(VERSION 3.8)

# set the project name
project(cs2_rcs LANGUAGES C CXX)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Executeable Main file
set(executeList
    cs2_rcs.cpp MemMan.cpp Game.cpp
)

# Feature list
set(FeatureList 
    Features/recoil_control.cpp    
)

# add the executable
add_executable(${PROJECT_NAME}
 ${executeList}
 ${FeatureList}
)

target_link_libraries(${PROJECT_NAME} PUBLIC)

# Add include
target_include_directories(${PROJECT_NAME} PUBLIC
 ${PROJECT_SOURCE_DIR}
 )