#-----------------------------------------------------------------------------------------------
# CMake file for the MRPT example:  /gui_fbo_render_example
#
#  Run with "ccmake ." at the root directory, or use it as a template for
#   starting your own programs
#-----------------------------------------------------------------------------------------------
SET(sampleName gui_fbo_render_example)
PROJECT(EXAMPLE_${sampleName})

CMAKE_MINIMUM_REQUIRED(VERSION 3.1)
set (CMAKE_CXX_STANDARD 14)  # Require C++14
cmake_policy(SET CMP0003 NEW)  # Required by CMake 2.7+

# ---------------------------------------------------------------------------
# Set the output directory of each example to its corresponding subdirectory
#  in the binary tree:
# ---------------------------------------------------------------------------
SET(EXECUTABLE_OUTPUT_PATH ".")

# The list of "libs" which can be included can be found in:
#  http://www.mrpt.org/Libraries
# Add the top-level dependencies only.
# --------------------------------------------------------------------------
FIND_PACKAGE(MRPT REQUIRED gui)

# Define the executable target:
ADD_EXECUTABLE(${sampleName} test.cpp  )

SET_TARGET_PROPERTIES(
	${sampleName}
	PROPERTIES
	PROJECT_LABEL "(EXAMPLE) ${sampleName}")

# Add special defines needed by this example, if any:
SET(MY_DEFS )
IF(MY_DEFS) # If not empty
	ADD_DEFINITIONS("-D${MY_DEFS}")
ENDIF()

# Add the required libraries for linking:
TARGET_LINK_LIBRARIES(${sampleName}
	${MRPT_LIBRARIES}  # This is filled by FIND_PACKAGE(MRPT ...)
	)

# Set optimized building:
IF((${CMAKE_CXX_COMPILER_ID} STREQUAL "Clang" OR CMAKE_COMPILER_IS_GNUCXX) AND NOT CMAKE_BUILD_TYPE MATCHES "Debug")
	add_compile_options(-O3)
ENDIF()

# This part can be removed if you are compiling this program outside of
#  the MRPT tree:
IF(${CMAKE_PROJECT_NAME} STREQUAL "MRPT") # Fails if build outside of MRPT project.
	DeclareAppDependencies(${sampleName} mrpt-gui) # Dependencies
ENDIF()
