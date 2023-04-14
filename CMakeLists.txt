cmake_minimum_required(VERSION 2.8)
project(orb_slam3_ros_wrapper)

# Change this to your installation of ORB-SLAM3. Default is ~/
set(ORB_SLAM3_DIR $ENV{HOME}/morb_slam)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "-std=c++14")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -Wall -g")

find_package(
  catkin REQUIRED
  COMPONENTS roscpp
             rospy
             std_msgs
             cv_bridge
             image_transport
             tf
             sensor_msgs
             message_generation
             morb_slam)

find_package(OpenCV REQUIRED)
message("OPENCV VERSION:")
message(${OpenCV_VERSION})

find_package(Eigen3 REQUIRED)
message("Eigen3 VERSION:")
message(${Eigen3_VERSION})

catkin_package(
  CATKIN_DEPENDS
  roscpp
  rospy
  std_msgs
  cv_bridge
  image_transport
  tf
  sensor_msgs
  message_runtime
  morb_slam
  LIBRARIES
  {PROJECT_NAME})

include_directories(${PROJECT_SOURCE_DIR}/include ${EIGEN3_INCLUDE_DIR} ${catkin_INCLUDE_DIRS})

# Build ROS nodes
set(LIBS ${EIGEN3_LIBS} ${catkin_LIBRARIES} ${OpenCV_LIBS})

# mono node
add_executable(${PROJECT_NAME}_mono src/mono_node.cc src/common.cc)
target_link_libraries(${PROJECT_NAME}_mono ${LIBS})

# mono-inertial node
add_executable(${PROJECT_NAME}_mono_inertial src/mono_inertial_node.cc src/common.cc)
target_link_libraries(${PROJECT_NAME}_mono_inertial ${LIBS})

# stereo node
add_executable(${PROJECT_NAME}_stereo src/stereo_node.cc src/common.cc)
target_link_libraries(${PROJECT_NAME}_stereo ${LIBS})

# stereo-inertial node
add_executable(${PROJECT_NAME}_stereo_inertial src/stereo_inertial_node.cc src/common.cc)
target_link_libraries(${PROJECT_NAME}_stereo_inertial ${LIBS})

# RGB-D node
add_executable(${PROJECT_NAME}_rgbd src/rgbd_node.cc src/common.cc)
target_link_libraries(${PROJECT_NAME}_rgbd ${LIBS})
