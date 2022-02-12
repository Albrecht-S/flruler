#
# CMakeLists.txt file to build flruler with CMake
#
# This file is experimental and not yet ready for production,
# test and debug statements are enabled ...

cmake_minimum_required (VERSION 3.2.3)

project (flruler)

# (1) Find FLTK
# Note: FLTK_DIR *MUST* be defined on the commandline or in the CMake GUI, e.g.
# cmake -G Ninja -DFLTK_DIR="/path/to/fltk/build" ..

set (FLTK_DIR "" CACHE FILEPATH "FLTK installation or build directory")
find_package (FLTK REQUIRED NO_MODULE)

# (2) Debugging (optional): debug == true (1): yes, else: no

set (debug true)
if (debug)
  message (STATUS "--- Project: ${CMAKE_PROJECT_NAME} ---")
  message (STATUS "FLTK_VERSION          = '${FLTK_VERSION}'")
  message (STATUS "FLTK_INCLUDE_DIRS     = '${FLTK_INCLUDE_DIRS}'")
  message (STATUS "FLTK_INCLUDE_DIR      = '${FLTK_INCLUDE_DIR}'")
  message (STATUS "FLTK_LIBRARIES        = '${FLTK_LIBRARIES}'")
  message (STATUS "FLTK_FLUID_EXECUTABLE = '${FLTK_FLUID_EXECUTABLE}'")
  message (STATUS "FLTK_USE_FILE         = '${FLTK_USE_FILE}'")
endif ()

# Overwrite FLTK_LIBRARIES which is empty in current FLTKConfig.cmake:
# Add all required libraries like "fltk_images fltk_gl fltk".
# This project needs only "fltk"

set (FLTK_LIBRARIES fltk)

# add fltk_cairo if FLTK was built with Cairo support

if (TARGET fltk_cairo)
  set (FLTK_LIBRARIES fltk_cairo ${FLTK_LIBRARIES})
endif ()

if (debug)
  message (STATUS "FLTK_LIBRARIES        = '${FLTK_LIBRARIES}'")
endif ()

# (3) Build flruler executable

set (SOURCES flruler.C)
add_definitions (-DVERSION=\"1.06\")
add_executable (flruler WIN32 MACOSX_BUNDLE ${SOURCES})
target_include_directories (flruler PRIVATE ${FLTK_INCLUDE_DIRS})
target_link_libraries (flruler ${FLTK_LIBRARIES})
if (APPLE)
  target_link_libraries(flruler "-framework cocoa")
endif (APPLE)
