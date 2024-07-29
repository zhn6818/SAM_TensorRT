cmake_minimum_required(VERSION 3.5)

project(sam LANGUAGES CXX)
add_definitions(-std=c++11)


set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
option(CUDA_USE_STATIC_CUDA_RUNTIME OFF)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_BUILD_TYPE Debug)

find_package(CUDA REQUIRED)

add_definitions(-O0 -g)

include_directories(${PROJECT_SOURCE_DIR}/include)
# include and link dirs of cuda and tensorrt, you need adapt them if yours are different
# cuda
include_directories(/usr/local/cuda/include)
link_directories(/usr/local/cuda/lib64)
# tensorrt
include_directories(/data1/virgo_3rdparty/TensorRT-8.4.1.5/include/)
link_directories(/data1/virgo_3rdparty/TensorRT-8.4.1.5/lib/)
include_directories(/data1/virgo_3rdparty/opencv/include/opencv4)
include_directories(/data1/virgo_3rdparty/opencv/include/opencv4/opencv2)

link_directories(/data1/virgo_3rdparty/opencv/lib)
set(OpenCV_LIBRARIES   
libopencv_core.so.4.5 
libopencv_imgproc.so.4.5 
libopencv_imgcodecs.so.4.5 
libopencv_videoio.so.4.5  
libopencv_video.so.4.5 
libopencv_calib3d.so
libopencv_features2d.so
libopencv_highgui.so
)



# include_directories(
#     ${OpenCV_INCLUDE_DIRS}/include
#     ${CMAKE_SOURCE_DIR}/include/
#     ${CUDA_INCLUDE_DIRS}
# )

# file(GLOB SOURCE
#     ./include/*.h
#     ./src/*.cpp
# )

add_executable(sam ${SOURCE} main.cpp include/samDecoder.h include/samEncoder.h src/samDecoder.cpp src/samEncoder.cpp )
target_link_libraries(sam ${OpenCV_LIBRARIES} cudart nvinfer cudart nvonnxparser nvinfer_plugin)
