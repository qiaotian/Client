CMAKE_MINIMUM_REQUIRED(VERSION 3.2 FATAL_ERROR)

PROJECT(MClient)

#### GTK
FIND_PACKAGE(PkgConfig REQUIRED)
PKG_CHECK_MODULES(GTK3 REQUIRED gtk+-3.0)
SET(CMAKE_C_STANDARD 11)
INCLUDE_DIRECTORIES(${GTK3_INCLUDE_DIRS})
LINK_DIRECTORIES(${GTK3_LIBRARY_DIRS})
ADD_DEFINITIONS(${GTK3_CFLAGS_OTHER})

#### VTK
SET(VTK_SOURCE_DIR /home/qiao/VTK)
if(NOT VTK_SOURCE_DIR)
    MESSAGE(ERROR " Cannot build Rendering examples without VTK_SOURCE_DIR")
endif()

FIND_PACKAGE(VTK REQUIRED)
vtk_module_config(VTK
    vtkCommonCore
    vtkFiltersSources
    vtkInteractionStyle
    vtkRenderingOpenGL2
)
INCLUDE(${VTK_USE_FILE})
ADD_EXECUTABLE(Cylinder MACOSX_BUNDLE Cylinder.cxx)
TARGET_LINK_LIBRARIES(Cylinder ${VTK_LIBRARIES})

#### WebRTC
FIND_PACKAGE(LibWebRTC REQUIRED)
INCLUDE(${LIBWEBRTC_USE_FILE})
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR/bin})

INCLUDE_DIRECTORIES("../include")
INCLUDE_DIRECTORIES("/home/qiao/libwebrtc/include/webrtc/")
INCLUDE_DIRECTORIES("/home/qiao/libwebrtc/webrtc/src/third_party/jsoncpp/source/include/") # json/json.h
INCLUDE_DIRECTORIES("/home/qiao/libwebrtc/webrtc/src/third_party/libyuv/include/")

LINK_DIRECTORIES("/home/qiao/libwebrtc/webrtc/src/out/Release/obj/webrtc/") # libwebrtc.a
LINK_DIRECTORIES("/home/qiao/libwebrtc/webrtc/src/out/Release/obj/webrtc/") # libwebrtc_common.a
LINK_DIRECTORIES("/home/qiao/libwebrtc/webrtc/src/build/linux/debian_jessie_amd64-sysroot/usr/lib/") # libjsoncpp.a
LINK_DIRECTORIES("/home/qiao/libwebrtc/webrtc/src/out/Release/obj/webrtc/base/") # librtc_base.a


#FIND_LIBRARY(JSONCPP_LIBRARIES jsoncpp HINTS /usr/lib/x86_64-linux-gnu)
#MESSAGE("JSONCPP_LIBRARIES: " ${JSONCPP_LIBRARIES})

SET(SOURCE_FILES main.cc gtk_main_wnd.cc defaults.cc peer_connection_client.cc conductor.cc)
SET(WEBRTC_LIBS libwebrtc.a libwebrtc_common.a librtc_base.a)
SET(JSONCPP_LIBS libjsoncpp.a)

ADD_EXECUTABLE(MClient ${SOURCE_FILES})
TARGET_LINK_LIBRARIES(MClient ${GTK3_LIBRARIES})
TARGET_LINK_LIBRARIES(MClient ${WEBRTC_LIBS})
TARGET_LINK_LIBRARIES(MClient ${LIBWEBRTC_LIBRARIES})
TARGET_LINK_LIBRARIES(MClient ${JSONCPP_LIBS})
TARGET_LINK_LIBRARIES(MClient ${JSONCPP_LIBRARIES})
