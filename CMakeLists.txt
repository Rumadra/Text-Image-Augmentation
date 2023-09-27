set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -Wall -std=c++11")

# Head and source
include_directories(include)
AUX_SOURCE_DIRECTORY(src DIR_SRCS)

# Find necessary packages
find_package( PythonLibs 2.7 REQUIRED )
include_directories( ${PYTHON_INCLUDE_DIRS} )

execute_process(COMMAND python2.7 -c "import numpy; print(numpy.get_include())"
                RESULT_VARIABLE NUMPY_INCLUDE_PATH_RESULT
                OUTPUT_VARIABLE NUMPY_INCLUDE_PATH
                OUTPUT_STRIP_TRAILING_WHITESPACE)
include_directories(${NUMPY_INCLUDE_PATH})

find_package( Boost COMPONENTS python REQUIRED )
include_directories( ${Boost_INCLUDE_DIR} )

find_package( OpenCV REQUIRED )
include_directories( ${OpenCV_INCLUDE_DIR} )

# Define the wrapper library
add_library(Augment SHARED ${DIR_SRCS})
target_link_libraries(Augment ${Boost_LIBRARIES} ${PYTHON_LIBRARIES} ${OpenCV_LIBRARIES} Augment)

# Don't prepend wrapper library name with lib
set_target_properties(Augment PROPERTIES PREFIX "" )
