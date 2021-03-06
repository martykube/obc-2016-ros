cmake_minimum_required(VERSION 2.8.3)
project(camera_driver)


find_package(PkgConfig REQUIRED)
find_package(OpenCV REQUIRED)
find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  message_generation
  cv_bridge
  image_transport
  polled_camera
  camera_info_manager
)

pkg_check_modules(libgphoto2 REQUIRED)
pkg_check_modules(libraw REQUIRED)

add_service_files(
  FILES
  SetBlobInfo.srv
)

generate_messages(
  DEPENDENCIES
  std_msgs
)


catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES camera_driver
  CATKIN_DEPENDS roscpp rospy std_msgs message_runtime
#  DEPENDS system_lib
)


include_directories(
  ${catkin_INCLUDE_DIRS}
)


include_directories(include ${catkin_INCLUDE_DIRS} ${OpenCV_INCLUDE_DIRS} ${libgphoto2_INCLUDE_DIRS})

add_executable(camera_image src/camera_image_node.cpp)
add_executable(camera_driver src/camera_driver_node.cpp)
add_executable(camera_video src/camera_video_node.cpp)
#add_executable(camera_v4l2 src/camera_v4l2_node.cpp)  
add_executable(nikon_d5100 src/nikon_d5100.cpp)
add_executable(people_detector src/people_detector.cpp)
add_executable(to_stream src/to_stream.cpp)


target_link_libraries(camera_image ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})
target_link_libraries(camera_driver ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})
target_link_libraries(camera_video ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})
#target_link_libraries(camera_v4l2 ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})
target_link_libraries(nikon_d5100 ${catkin_LIBRARIES} ${OpenCV_LIBRARIES} gphoto2)
target_link_libraries(people_detector ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})
target_link_libraries(to_stream ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})



