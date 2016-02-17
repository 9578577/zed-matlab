cmake_minimum_required(VERSION 2.8)
project(MEX)

set(CMAKE_INSTALL_PREFIX ${CMAKE_SOURCE_DIR})
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)

add_definitions(/DMATLAB_MEX_FILE)
add_definitions(/DMX_COMPAT_32)

find_package(Matlab REQUIRED)

MESSAGE( STATUS "Matlab path $ENV{MATLAB_ROOT}")

IF(WIN32)
SET(ZED_INCLUDE_DIRS $ENV{ZED_INCLUDE_DIRS})
	if (CMAKE_CL_64)
		SET(ZED_LIBRARIES $ENV{ZED_LIBRARIES_64})
	else(CMAKE_CL_64)
		SET(ZED_LIBRARIES $ENV{ZED_LIBRARIES_32})
	endif(CMAKE_CL_64)
SET(ZED_LIBRARY_DIR $ENV{ZED_LIBRARY_DIR})
SET(OPENCV_DIR $ENV{OPENCV_DIR})
ELSE()
find_package(ZED 0.9 REQUIRED)
ENDIF(WIN32)

find_package(CUDA 6.5 REQUIRED)
find_package(OpenCV 2.4 COMPONENTS core highgui imgproc REQUIRED)

INCLUDE_DIRECTORIES(include)

IF(MATLAB_FOUND)
    message(STATUS "MATLAB Found, MATLAB MEX will be compiled.")
    add_subdirectory(mex)
ELSE(MATLAB_FOUND)
    MESSAGE("MATLAB not found...nothing will be built.")
ENDIF(MATLAB_FOUND)