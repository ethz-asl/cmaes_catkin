cmake_minimum_required(VERSION 2.8.3)
project(cmaes_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(cmaes_src
  GIT_REPOSITORY git@github.com:beniz/libcmaes.git
  UPDATE_COMMAND ""
  CONFIGURE_COMMAND cd ../cmaes_src && ./autogen.sh && ./configure --enable-onlylib --prefix=${CATKIN_DEVEL_PREFIX}
  BUILD_COMMAND cd ../cmaes_src && make -j 8
  INSTALL_COMMAND cd ../cmaes_src && make install
)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} cmaes_src)
target_link_libraries(${PROJECT_NAME} ${CATKIN_DEVEL_PREFIX}/lib/libcmaes${CMAKE_SHARED_LIBRARY_SUFFIX})

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  CFG_EXTRAS cmaes-extras.cmake)
