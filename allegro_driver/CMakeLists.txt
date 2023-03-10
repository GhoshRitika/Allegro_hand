cmake_minimum_required(VERSION 3.8)
project(allegro_driver)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_srvs REQUIRED)
find_package(std_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(tf2 REQUIRED)
find_package(moveit_common REQUIRED)
find_package(moveit_ros_planning_interface REQUIRED)
find_package(moveit_core REQUIRED)
find_package(moveit_ros_planning REQUIRED)
# find_package(moveit_ros_planning_interface REQUIRED)
# include(${INSTALL_PREFIX}/lib/cmake/allegro_lib.cmake)
find_package(allegro_lib REQUIRED)
find_library(NAMES BHand PATHS home/ritz/WinterQ/Project/AllegroHandManipulation/Allegro_interface/src/libBHand_64/lib/libBHand.so)
# find_library(NAMES PCANBasic PATHS home/ritz/WinterQ/Project/AllegroHandManipulation/Allegro_interface/src/Pcan-Basic_Linux-4.6.2.36/libpcanbasic/pcanbasic/lib/libpcanbasic.so)
# find_package(bhand REQUIRED NO_MODULE)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)
# add_library(bhand SHARED IMPORTED)
# set_property(TARGET bhand PROPERTY
#              IMPORTED_LOCATION "/home/ritz/WinterQ/Project/AllegroHandManipulation/Allegro_interface/src/bhand/lib/BHand.so")
add_executable(allegro_driver src/allegro_driver.cpp)
add_executable(hello_moveit src/hello_moveit.cpp)
add_executable(plan_hand src/plan_hand.cpp)
# target_link_libraries(allegro_driver bhand)
# target_include_directories(allegro_driver PUBLIC
#   $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
#   $<INSTALL_INTERFACE:include>)
target_link_libraries(allegro_driver BHand) #PCANBasic
ament_target_dependencies(allegro_driver rclcpp std_srvs std_msgs sensor_msgs allegro_lib)
ament_target_dependencies(hello_moveit rclcpp geometry_msgs moveit_ros_planning_interface tf2)
ament_target_dependencies(plan_hand rclcpp std_msgs moveit_ros_planning_interface moveit_core moveit_ros_planning)

install(TARGETS allegro_driver hello_moveit plan_hand DESTINATION lib/${PROJECT_NAME})

install(DIRECTORY
  launch
  config
  meshes
  urdf
  DESTINATION share/${PROJECT_NAME}/
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()


ament_package()
