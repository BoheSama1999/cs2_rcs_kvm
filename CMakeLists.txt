cmake_minimum_required(VERSION 3.8)

# set the project name
project(cs2_rcs VERSION 0.0.1 LANGUAGES C CXX)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# include module
include(ExternalProject)
include(CTest)
include(CPack)

# enable testing 
enable_testing()

# cPack 
set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})

# cxx flags
set(CMAKE_CXX_FLAGS "-g -Wall -Wformat -march=native")
set(CMAKE_BUILD_TYPE "Release")

# Executeable Main file
set(executeList
    cs2_rcs.cpp MemMan.cpp Game.cpp
)

# Feature list
set(FeatureList 
    Features/recoil_control.cpp    
)

# ExternalProject -----start
set(LibsDirectory ${PROJECT_SOURCE_DIR}/Libs)
    # function to add external projects
    function(add_memflow_project name subdir)
        ExternalProject_Add(
           ${name}
            DOWNLOAD_COMMAND ""
            CONFIGURE_COMMAND ""
            BINARY_DIR ${LibsDirectory}/${subdir}/
            BUILD_COMMAND cargo build --release --all-features
            INSTALL_COMMAND cp ${LibsDirectory}/${subdir}/target/release/lib${name}.so ${CMAKE_BINARY_DIR}/
            LOG_BUILD ON
        )
    endfunction()

    # Add external projects
    add_memflow_project(memflow_ffi memflow)
    add_memflow_project(memflow_kvm memflow-kvm)
    add_memflow_project(memflow_qemu memflow-qemu)
    add_memflow_project(memflow_win32 memflow-win32)

    # Find required packages
    find_package(Threads REQUIRED)
# ExternalProject -----end


# add the executable
add_executable(${PROJECT_NAME}
 ${executeList}
 ${FeatureList}
)

# Add dependencies
add_dependencies(cs2_rcs
    memflow_ffi
    memflow_kvm
    memflow_qemu
    memflow_win32
)

# Link libraries
target_link_libraries(cs2_rcs PUBLIC
    ${LibsDirectory}/memflow/target/release/libmemflow_ffi.a
    ${CMAKE_THREAD_LIBS_INIT}
    ${CMAKE_DL_LIBS}
)

# Add include directories
target_include_directories(cs2_rcs PUBLIC
 ${PROJECT_SOURCE_DIR}
 ${LibsDirectory}/memflow/memflow_ffi
 )