cmake_minimum_required(VERSION 3.22)
project(mpqbuilder)
set( Boost_USE_STATIC_LIBS ON )
set(CMAKE_CXX_STANDARD 17)
FIND_PACKAGE( Boost 1.60 COMPONENTS filesystem REQUIRED )
add_executable(mpqbuilder mpqbuilder.cpp)
add_executable(luaxmlreader luaxmlreader.cpp)

include (FetchContent)
FetchContent_Declare(
  storm
  GIT_REPOSITORY https://github.com/ladislav-zezula/StormLib.git
  # Needs at least https://github.com/ladislav-zezula/StormLib/commit/026e9b23ad7fd105b2b171cce0ea4ea965a30dd9 to remove
  # undeclared function call that breaks clang 12+
  # See https://github.com/ladislav-zezula/StormLib/issues/222#issuecomment-841698004
  GIT_TAG v9.24
)
FetchContent_GetProperties (storm)
if (NOT storm_POPULATED)
  message (STATUS "Installing StormLib...")
  FetchContent_Populate (storm)
endif()
add_subdirectory (${storm_SOURCE_DIR} ${storm_BINARY_DIR} EXCLUDE_FROM_ALL)
set (storm_warning_disable_flags "")
TARGET_LINK_LIBRARIES(mpqbuilder
  Boost::filesystem
  storm
)
TARGET_LINK_LIBRARIES(luaxmlreader
  Boost::filesystem
  storm
)
