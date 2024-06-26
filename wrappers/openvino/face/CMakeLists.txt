cmake_minimum_required(VERSION 3.8)

project(RealSenseVinoFaceExample)


# OpenCV is required here
if(NOT DEFINED OpenCV_DIR  OR  NOT IS_DIRECTORY ${OpenCV_DIR})
	set(OpenCV_DIR "" CACHE PATH "The path to the OpenCV Toolkit build directory")
	message( FATAL_ERROR "OpenVINO examples require OpenCV; specify OpenCV_DIR" )
endif()
find_package(OpenCV REQUIRED)
get_property(deps VARIABLE PROPERTY DEPENDENCIES)
set(DEPENDENCIES ${deps} ${OpenCV_LIBS})
include_directories( ../../opencv )


add_executable(rs-face-vino
	rs-face-vino.cpp
	${OPENVINO_FILES}
    ${ELPP_FILES}
)

if(OPENVINO2019)
	target_compile_definitions(rs-face-vino PRIVATE OPENVINO2019)
endif()

if(OPENVINO_NGRAPH)
	target_compile_definitions(rs-face-vino PRIVATE OPENVINO_NGRAPH)
endif()

source_group("OpenVINO" FILES ${OPENVINO_FILES})
source_group("EasyLogging++" FILES ${ELPP_FILES})

set_property(TARGET rs-face-vino PROPERTY CXX_STANDARD 11)
target_link_libraries(rs-face-vino ${DEPENDENCIES})
set_target_properties (rs-face-vino PROPERTIES
	FOLDER "Examples/openvino"
)

install(
	TARGETS

	rs-face-vino

	RUNTIME DESTINATION
	${CMAKE_INSTALL_PREFIX}/bin
)


# Download model files -- these will go into build/wrappers/openvino/face, which is also where the sample
# is run from in Visual Studio
if(OPENVINO2019)
dl_vino_model( "README.txt"                    "789e144d6cafa379c8437d2a6860829b6d935a8d" )
dl_vino_model( "face-detection-adas-0001.bin"  "1625f05ada1d8e78e36856def7124f78cd0a6c2a" )
dl_vino_model( "face-detection-adas-0001.xml"  "11c36f6e4488b2f5437ccff9f330d02545087ab3" )
else()
# Newer model files with IRv10, 2020.1 and later
dl_vino_model( "README.txt"                    "789e144d6cafa379c8437d2a6860829b6d935a8d" )
dl_vino_model( "face-detection-adas-0001.bin"  "065e9015297c7adbc650ebfc17176477cd0a4279" )
dl_vino_model( "face-detection-adas-0001.xml"  "331be642a248213f586ec1d1b71c223435a437e3" )
endif()
