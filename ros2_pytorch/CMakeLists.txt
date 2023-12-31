cmake_minimum_required(VERSION 3.5.0)

project(ros2_pytorch)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
    set(CMAKE_CXX_STANDARD 14)
endif()

set(CMAKE_PREFIX_PATH /libtorch)

find_package(ament_cmake REQUIRED)
find_package(example_interfaces REQUIRED)
find_package(rcl REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rcutils)
find_package(rmw REQUIRED)
find_package(std_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(cv_bridge REQUIRED)
find_package(Torch REQUIRED)

find_package(OpenCV REQUIRED)

include_directories(include ${cv_bridge_INCLUDE_DIRS})


function(custom_executable target)
    add_executable(${target} src/${target}.cpp
    src/${PROJECT_NAME}_node.cpp)
    ament_target_dependencies(${target}
    "example_interfaces"
    "rclcpp"
    "sensor_msgs"
    "rcutils"
    "std_msgs"
    "cv_bridge"
    "OpenCV"
    )
    install(TARGETS ${target}
    DESTINATION lib/${PROJECT_NAME})
endfunction()


install(DIRECTORY
launch
DESTINATION share/${PROJECT_NAME}/)

custom_executable(ros2_pytorch)

target_link_libraries(ros2_pytorch "${TORCH_LIBRARIES}")


ament_package()
