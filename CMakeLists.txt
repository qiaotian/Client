CMAKE_MINIMUM_REQUIRED(VERSION 3.2 FATAL_ERROR)

PROJECT(MClient)

ADD_SUBDIRECTORY(doc)
ADD_SUBDIRECTORY(lib)
ADD_SUBDIRECTORY(src bin) # add src and export exe to bin folder
ADD_SUBDIRECTORY(include)

# define exe export path
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR/bin})

INCLUDE_DIRECTORIES("./include")
LINK_DIRECTORIES("./lib")

ADD_EXECUTABLE(main main.cc)
TARGET_LINK_LIBRARIES(main libwebrtc.a libwebrtc_common.a)
