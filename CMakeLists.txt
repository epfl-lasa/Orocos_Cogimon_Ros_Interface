cmake_minimum_required(VERSION 2.6.3)
project(test_orocos)

# Use 'source orocos_toolchain/env.sh' if the command below fails:
find_package(catkin REQUIRED COMPONENTS
  roscpp rtt_ros actionlib actionlib_msgs genmsg rtt_roscomm
  # ADDITIONAL ROS PACKAGES
  )


find_package(OROCOS-RTT QUIET)
if (NOT OROCOS-RTT_FOUND)
  message (FATAL_ERROR "\nCould not find Orocos. Please use the shell command\n 'source orocos_toolchain/env.sh' and then run cmake again.")
endif()

find_package(OROCOS-RTT REQUIRED COMPONENTS rtt-scripting rtt-typekit)
if (NOT OROCOS-RTT_FOUND)
  message (FATAL_ERROR "\nCould not find Orocos. Please use the shell command\n 'source orocos_toolchain/env.sh' and then run cmake again.")
endif()
find_package(RST-RT REQUIRED)
message(STATUS "RST-RT version: ${RST-RT_VERSION}")
include_directories(BEFORE SYSTEM ${RST-RT_INCLUDE_DIRS})
add_definitions(${RST-RT_CFLAGS})
link_directories(${RST-RT_LIBRARY_DIR} ${RST-RT_LIBRARY_DIRS})

include_directories(include)
include_directories(${catkin_INCLUDE_DIRS})

### Orocos Dependencies ###
# Note that orocos_use_package() does not need to be called for any dependency
# listed in the package.xml file

include_directories(${USE_OROCOS_INCLUDE_DIRS})


add_definitions(-DRTT_COMPONENT)
orocos_component(test_orocos include/test_orocos-component.hpp src/test_orocos-component.cpp)
target_link_libraries(test_orocos ${catkin_LIBRARIES} ${USE_OROCOS_LIBRARIES} ${RST-RT_LIBRARIES})


# orocos_library(my_library src/my_library.cpp)
# target_link_libraries(my_library ${catkin_LIBRARIES} ${USE_OROCOS_LIBRARIES})

# orocos_service(my_service src/my_service.cpp)
# target_link_libraries(my_service ${catkin_LIBRARIES} ${USE_OROCOS_LIBRARIES})

# orocos_plugin(my_plugin src/my_plugin.cpp)
# target_link_libraries(my_plugin ${catkin_LIBRARIES} ${USE_OROCOS_LIBRARIES})

# orocos_typekit(my_typekit src/my_typekit.cpp)
# target_link_libraries(my_typekit ${catkin_LIBRARIES} ${USE_OROCOS_LIBRARIES})

### Orocos Package Exports and Install Targets ###
orocos_install_headers(include/test_orocos-component.hpp)

# Generate install targets for header files

orocos_install_headers(DIRECTORY include/${PROJECT_NAME})

# Export package information (replaces catkin_package() macro) 
orocos_generate_package()

install(FILES 
  test_orocos.ops 
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
