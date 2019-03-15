cmake_minimum_required(VERSION 2.8.3)
project(moveit_chomp_optimizer_adapter)

add_compile_options(-std=c++14)

find_package(catkin REQUIRED COMPONENTS
  moveit_core
  chomp_motion_planner
  pluginlib
  )

catkin_package()

include_directories(${catkin_INCLUDE_DIRS})

if(WIN32)
  find_package(Eigen3 REQUIRED)
  include_directories(include "${EIGEN3_INCLUDE_DIR}/..")
endif()

add_library(${PROJECT_NAME} src/chomp_optimizer_adapter.cpp)
set_target_properties(${PROJECT_NAME} PROPERTIES VERSION "${${PROJECT_NAME}_VERSION}")
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})

install(FILES chomp_optimizer_adapter_plugin_description.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

if(WIN32)
  install(TARGETS ${PROJECT_NAME}
    ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    RUNTIME DESTINATION ${CATKIN_GLOBAL_LIB_DESTINATION}
    CONFIGURATIONS ${CMAKE_BUILD_TYPE}
  )
else()
  install(TARGETS ${PROJECT_NAME}
    ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
  )
endif()