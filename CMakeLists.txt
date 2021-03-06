project(open62541)
cmake_minimum_required(VERSION 3.5)

SET(${PROJECT_NAME}_MAJOR_VERSION 0)
SET(${PROJECT_NAME}_MINOR_VERSION 2)
SET(${PROJECT_NAME}_PATCH_VERSION 2-2)
include(${CMAKE_SOURCE_DIR}/cmake/set_version_numbers.cmake)

include(ExternalProject)
 ExternalProject_Add(external-open62541
   GIT_REPOSITORY "https://github.com/open62541/open62541.git"
   GIT_TAG "v0.2.2"
   #GIT_TAG "93e79321c1b40bbdada49f3bf3d52095b8f64100"
   #GIT_TAG "0.2-rc2" // Array outlier occure
   #GIT_TAG "2cabd7c5d960dadcebc75c194daafc09cfa74ce2" // Tag during the first integration Meeting
   PREFIX "${CMAKE_BINARY_DIR}/open62541_src"
   CMAKE_ARGS
   "-DCMAKE_PROJECT_NAME=${PROJECT_NAME}"
   "-DCMAKE_INSTALL_PREFIX=${CMAKE_BINARY_DIR}/open62541_install"
   "-DUA_ENABLE_AMALGAMATION=On"
   "-DBUILD_SHARED_LIBS=ON"
   "-DUA_ENABLE_METHODCALLS=ON"
   "-DUA_ENABLE_NODEMANAGEMENT=ON"
   "-DUA_ENABLE_SUBSCRIPTIONS=ON"
   "-DUA_LOGLEVEL=300"
   "-DCMAKE_BUILD_TYPE=DEBUG"
   "-D${PROJECT_NAME}_MAJOR_VERSION=${${PROJECT_NAME}_MAJOR_VERSION}"
   "-D${PROJECT_NAME}_MINOR_VERSION=${${PROJECT_NAME}_MINOR_VERSION}"
   "-D${PROJECT_NAME}_PATCH_VERSION=${${PROJECT_NAME}_PATCH_VERSION}"
   "-D${PROJECT_NAME}_BUILDVERSION=${${PROJECT_NAME}_BUILDVERSION}"
  #"-DUA_ENABLE_GENERATE_NAMESPACE0=On"
  #"-DUA_ENABLE_ENABLE_MULTITHREADING=ON"
  PATCH_COMMAND git apply ${PROJECT_SOURCE_DIR}/cmakefile.patch
  ${PROJECT_SOURCE_DIR}/bugfix1.patch
  ${PROJECT_SOURCE_DIR}/bugfix2.patch
  ${PROJECT_SOURCE_DIR}/bugfix3.patch
  ${PROJECT_SOURCE_DIR}/bugfix4.patch
  ${PROJECT_SOURCE_DIR}/bugfix5.patch
  ${PROJECT_SOURCE_DIR}/bugfix6.patch
  ${PROJECT_SOURCE_DIR}/bugfix7.patch
  ${PROJECT_SOURCE_DIR}/connectionState.patch
  )
install(FILES ${CMAKE_BINARY_DIR}/open62541_install/include/open62541.h DESTINATION include)
install(DIRECTORY ${CMAKE_BINARY_DIR}/open62541_install/lib/ DESTINATION lib )

set(${PROJECT_NAME}_INCLUDE_DIRS "${CMAKE_INSTALL_PREFIX}/include")
set(${PROJECT_NAME}_LIBRARIES "${PROJECT_NAME}")
set(${PROJECT_NAME}_LIBRARY_DIRS "${CMAKE_INSTALL_PREFIX}/lib")
include(${CMAKE_SOURCE_DIR}/cmake/create_cmake_config_files.cmake)