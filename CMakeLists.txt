cmake_minimum_required(VERSION 2.8.8)

project (VL53L0X_linux)
add_library(vl53l0x SHARED I2Cdev.cpp VL53L0X.cpp)

### Setup compiler
# Set warning flags
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wformat=2 -Wall -Wextra")
# Set optimalization flags
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2")

set_target_properties(vl53l0x PROPERTIES SOVERSION 1)
set_target_properties(vl53l0x PROPERTIES PUBLIC_HEADER VL53L0X.hpp)
set_target_properties(vl53l0x PROPERTIES PUBLIC_HEADER VL53L0X_defines.hpp)
target_include_directories(vl53l0x PRIVATE .)

if(NOT DEFINED CMAKE_INSTALL_LIBDIR)
	set(CMAKE_INSTALL_LIBDIR "lib")
endif()
if(NOT DEFINED CMAKE_INSTALL_INCLUDEDIR)
	set(CMAKE_INSTALL_INCLUDEDIR "include")
endif()
if(NOT DEFINED CMAKE_INSTALL_DATAROOTDIR)
	set(CMAKE_INSTALL_DATAROOTDIR "/usr/share")
endif()


install(TARGETS vl53l0x
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

configure_file(vl53l0x.pc.in vl53l0x.pc @ONLY)
install(FILES ${CMAKE_BINARY_DIR}/vl53l0x.pc
        DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/pkgconfig)

### Examples
add_subdirectory (examples)
