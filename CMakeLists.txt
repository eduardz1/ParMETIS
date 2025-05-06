cmake_minimum_required(VERSION 3.14...3.28)
project(ParMETIS LANGUAGES C)

# --- need METIS first
include(cmake/CPM.cmake)

CPMAddPackage(
  NAME GKlib
  GITHUB_REPOSITORY "scivision/GKlib"
  GIT_TAG 078042614ff5192ab4c9bde0575bfef0d4572687
  OPTIONS
    "CMAKE_C_FLAGS -D_POSIX_C_SOURCE=199309L" # see https://github.com/KarypisLab/GKlib/issues/24
)

CPMAddPackage(
  NAME metis_fc
  GITHUB_REPOSITORY "scivision/metis"
  VERSION 5.2.1.1
)

if(NOT DEFINED REALTYPEWIDTH)
  set(REALTYPEWIDTH 32)
endif()
if(NOT DEFINED IDXTYPEWIDTH)
  set(IDXTYPEWIDTH 32)
endif()
add_compile_definitions(REALTYPEWIDTH=${REALTYPEWIDTH} IDXTYPEWIDTH=${IDXTYPEWIDTH})

message(STATUS "${PROJECT_NAME} ${PROJECT_VERSION} CMake ${CMAKE_VERSION}
REALTYPEWIDTH=${REALTYPEWIDTH} IDXTYPEWIDTH=${IDXTYPEWIDTH}")
message(STATUS "METIS ${metis_fc_SOURCE_DIR}")
message(STATUS "GKlib ${GKlib_SOURCE_DIR}")

find_package(MPI REQUIRED)

include(./conf/gkbuild.cmake)

install(FILES include/parmetis.h TYPE INCLUDE)

add_subdirectory(libparmetis)
add_subdirectory(programs)

# This is for testing during development and is not being distributed
#add_subdirectory(test)
