cmake_minimum_required(VERSION 3.10 FATAL_ERROR)  # aggiornato da 3.0.2 a 3.10

project(vostok LANGUAGES CXX)

# Usa C++17 invece di C++0x
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Find external dependencies
find_package(OpenMP)

# Setup compiler flags
# Default is RELWITHDEBINFO
# Change on command line e.g. with "-DCMAKE_BUILD_TYPE=RELEASE"
set(CMAKE_CXX_FLAGS_RELWITHDEBINFO "-O3 -g -DNDEBUG -Wall")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -DNDEBUG -Wall")
set(CMAKE_CXX_FLAGS_DEBUG "-O0 -g3 -Wall")

# RIMOSSA la vecchia opzione obsoleta
# add_definitions( -std=c++0x -m64 )

# Define vostok executable ###################
set(header_files
  src/IrradianceCalc.h
  src/myUtils.h
  src/OctreeRaycaster.h
  src/ProjectConfig.h
  src/ShadowCalc.h
  src/Vec3d.h
  src/octree/Octree.h
  src/octree/OctreeNode.h
  src/octree/OctreeNodeInfo.h
  src/pointCloud/AbstractPointCloud.h
  src/pointCloud/SimpleTextPointCloud.h
  src/solpos/solpos00.h
)

set(source_files
  src/main.cpp
  src/myUtils.cpp
  src/IrradianceCalc.cpp
  src/OctreeRaycaster.cpp
  src/ProjectConfig.cpp
  src/ShadowCalc.cpp
  src/Vec3d.cpp
  src/octree/Octree.cpp
  src/octree/OctreeNode.cpp
  src/octree/OctreeNodeInfo.cpp
  src/pointCloud/AbstractPointCloud.cpp
  src/pointCloud/SimpleTextPointCloud.cpp
  src/solpos/solpos.cpp
)

add_executable(vostok
  ${source_files}
  ${header_files}
)

if(OPENMP_FOUND)
  target_compile_options(vostok PRIVATE ${OpenMP_CXX_FLAGS})
  target_link_libraries(vostok PRIVATE ${OpenMP_CXX_FLAGS})
endif()
##############################################

# Support installation
include(GNUInstallDirs)

install(TARGETS vostok
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

