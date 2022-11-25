cmake_minimum_required(VERSION 3.16)
project(orb_slam3_ros_wrapper)

find_package(catkin REQUIRED COMPONENTS
   roscpp
   rospy
   std_msgs
   cv_bridge
   image_transport
   tf
   sensor_msgs
   message_generation
   orb_slam3_core
)

find_package(OpenCV 4.2 REQUIRED)
find_package(Eigen3 3.1.0 REQUIRED)

catkin_package(
   CATKIN_DEPENDS roscpp rospy std_msgs cv_bridge image_transport tf sensor_msgs message_runtime orb_slam3_core
   LIBRARIES {PROJECT_NAME}
)

include_directories(
   ${PROJECT_SOURCE_DIR}/include
   ${EIGEN3_INCLUDE_DIR}
   ${catkin_INCLUDE_DIRS}
)

# Build ROS nodes
set(LIBS
   ${EIGEN3_LIBS}
   ${catkin_LIBRARIES}
   ${OpenCV_LIBS}
)

# mono node
add_executable(${PROJECT_NAME}_mono
   src/mono_node.cc
   src/common.cc
)
target_link_libraries(${PROJECT_NAME}_mono
   ${LIBS}
)

# mono-inertial node
add_executable(${PROJECT_NAME}_mono_inertial
   src/mono_inertial_node.cc
   src/common.cc
)
target_link_libraries(${PROJECT_NAME}_mono_inertial
   ${LIBS}
)

# stereo node
add_executable(${PROJECT_NAME}_stereo
   src/stereo_node.cc
   src/common.cc
)
target_link_libraries(${PROJECT_NAME}_stereo
   ${LIBS}
)

# stereo-inertial node
add_executable(${PROJECT_NAME}_stereo_inertial
   src/stereo_inertial_node.cc
   src/common.cc
)
target_link_libraries(${PROJECT_NAME}_stereo_inertial
   ${LIBS}
)

# RGB-D node
add_executable(${PROJECT_NAME}_rgbd
   src/rgbd_node.cc
   src/common.cc
)
target_link_libraries(${PROJECT_NAME}_rgbd
   ${LIBS}
)