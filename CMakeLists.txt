# cmake_minimum_required(VERSION 2.8)
# set(CMAKE_MACOSX_RPATH 1)
#
# project(selective_search)
#
# execute_process(COMMAND python3.4-config --cflags  OUTPUT_VARIABLE PYTHON3_CFLAGS  OUTPUT_STRIP_TRAILING_WHITESPACE)
# execute_process(COMMAND python3.4-config --ldflags OUTPUT_VARIABLE PYTHON3_LDFLAGS OUTPUT_STRIP_TRAILING_WHITESPACE)
# set(BOOST_LIBS "-lboost_system -lboost_python -lboost_numpy")
#
# set(SUPPRESS_WARNING "-Wall -Wno-unused-function -Wno-unused-variable -Wno-unused-local-typedefs")
#
# include_directories("segment")
# set(CMAKE_CXX_FLAGS "-fPIC -O3 -std=c++11 ${PYTHON3_CFLAGS} ${SUPPRESS_WARNING}")
# set(CMAKE_SHARED_LINKER_FLAGS "${PYTHONi3_LDFLAGS} -shared -fPIC")
#
# add_library(segment SHARED segment_py.cpp)
# target_link_libraries(segment ${BOOST_LIBS} segment)
# set_target_properties(segment PROPERTIES PREFIX "" SUFFIX ".so")
cmake_minimum_required(VERSION 3.0)

project(selective_search)

set(CMAKE_CXX_FLAGS "-O3 -std=c++11")

find_package(PythonLibs 3.0 REQUIRED)
if(PYTHONLIBS_FOUND)
    include_directories(${PYTHON_INCLUDE_DIRS})
endif()

find_package(Boost 1.63 COMPONENTS python3 numpy3 REQUIRED)
if(Boost_FOUND)
    include_directories(${CMAKE_SOURCE_DIR}/segment ${CMAKE_CURRENT_SOURCE_DIR} ${Boost_INCLUDE_DIRS})

    add_library(segment SHARED segment_py.cpp)
    target_link_libraries(segment ${Boost_LIBRARIES} ${PYTHON_LIBRARIES})
    set_target_properties(segment PROPERTIES PREFIX "" SUFFIX ".so")
endif()
