cmake_minimum_required(VERSION 2.8.3)
project(rosjava_custom_srv)

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED message_generation std_msgs)


## Generate services in the 'srv' folder
add_service_files(
   FILES
  CustomService.srv

 )

generate_messages(DEPENDENCIES std_msgs)

catkin_package(
  CATKIN_DEPENDS message_runtime std_msgs
)

###########
## Build ##
###########


include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

set(file_names_to_copy
 
  CustomService
  CustomServiceRequest
  CustomServiceResponse
)

set(generated_header_LOCATION  ${CATKIN_DEVEL_PREFIX}/include/${PROJECT_NAME}/)
set(generated_header_DESTINATION ${PROJECT_SOURCE_DIR}/include/${PROJECT_NAME}/)
foreach(file_name ${file_names_to_copy})
  set(from ${generated_header_LOCATION}${file_name}.h)
  set(to ${generated_header_DESTINATION}${file_name}.h)
  add_custom_command(
    OUTPUT ${to}
    DEPENDS ${from} ${${PROJECT_NAME}_EXPORTED_TARGETS}
    COMMAND cmake -E copy ${from} ${to})
  add_custom_target(copy_${file_name} ALL DEPENDS ${to})
endforeach(file_name)
