cmake_minimum_required(VERSION 2.8.3)
project(waypoint_maker)

find_package(catkin REQUIRED COMPONENTS
        gnss
        roscpp
        std_msgs
        geometry_msgs
        nav_msgs
        tf
        waypoint_follower
        vector_map
        lane_planner
        autoware_msgs
        )

find_package(Boost REQUIRED)

catkin_package(
        CATKIN_DEPENDS gnss
        roscpp
        std_msgs
        geometry_msgs
        nav_msgs
        tf
        waypoint_follower
        vector_map
        lane_planner
        autoware_msgs
)

SET(CMAKE_CXX_FLAGS "-std=c++11 -O2 -g -Wall ${CMAKE_CXX_FLAGS}")

include_directories(
        ${catkin_INCLUDE_DIRS}
        ${Boost_INCLUDE_DIRS}
)

add_executable(waypoint_loader nodes/waypoint_loader/waypoint_loader_core.cpp nodes/waypoint_loader/velocity_replanner.cpp nodes/waypoint_loader/waypoint_loader_node.cpp)
target_link_libraries(waypoint_loader ${catkin_LIBRARIES})
add_dependencies(waypoint_loader
        ${catkin_EXPORTED_TARGETS})

add_executable(waypoint_saver nodes/waypoint_saver/waypoint_saver.cpp)
target_link_libraries(waypoint_saver ${catkin_LIBRARIES})
add_dependencies(waypoint_saver
        ${catkin_EXPORTED_TARGETS})

add_executable(waypoint_clicker nodes/waypoint_clicker/waypoint_clicker.cpp)
target_link_libraries(waypoint_clicker ${catkin_LIBRARIES})

add_executable(waypoint_marker_publisher nodes/waypoint_marker_publisher/waypoint_marker_publisher.cpp)
target_link_libraries(waypoint_marker_publisher ${catkin_LIBRARIES})
add_dependencies(waypoint_marker_publisher
        ${catkin_EXPORTED_TARGETS})

add_executable(waypoint_velocity_visualizer nodes/waypoint_velocity_visualizer/waypoint_velocity_visualizer.cpp)
target_link_libraries(waypoint_velocity_visualizer ${catkin_LIBRARIES} ${Boost_LIBRARIES})
add_dependencies(waypoint_velocity_visualizer
        ${catkin_EXPORTED_TARGETS})

install(TARGETS
        waypoint_loader
        waypoint_saver
        waypoint_clicker
        waypoint_marker_publisher
        waypoint_velocity_visualizer
        ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
        )
