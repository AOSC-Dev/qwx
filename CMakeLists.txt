project(qwx)

cmake_minimum_required(VERSION 2.8.12 FATAL_ERROR)
set(CMAKE_AUTOMOC ON)
set(QT_MIN_VERSION "5.6.0")

# Set version
set(QWX_VERSION_MAJOR 0)
set(QWX_VERSION_MINOR 8)
set(QWX_VERSION_PATCH 0)
set(QWX_VERSION_STRING "${QWX_VERSION_MAJOR}.${QWX_VERSION_MINOR}.${QWX_VERSION_PATCH}")
set(PROJECT_VERSION QWX_VERSION_STRING)
add_definitions("-DQWX_VERSION_STRING=\"${QWX_VERSION_STRING}\"")
add_definitions("-DPROJECT_VERSION=\"${PROJECT_VERSION}\"")

# Definitions
set(DATADIR ${CMAKE_INSTALL_PREFIX}/share)
add_definitions(-std=c++14 -fpermissive)
add_definitions("-DDATADIR=\"${DATADIR}\"")

# Default build type
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()

# Handle build type
if(CMAKE_BUILD_TYPE MATCHES [Dd]ebug)
    message(STATUS "Debug build")
    add_definitions(-DQWX_DEBUG)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -O0")
else()
    message(STATUS "Release build")
    add_definitions(-DNDEBUG)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2")
endif()

# Default absolute paths
if(NOT DEFINED CMAKE_INSTALL_SYSCONFDIR)
    set(CMAKE_INSTALL_SYSCONFDIR "/etc")
endif()
if(NOT DEFINED CMAKE_INSTALL_LOCALSTATEDIR)
    set(CMAKE_INSTALL_LOCALSTATEDIR "/var")
endif()

find_package(Qt5 REQUIRED COMPONENTS LinguistTools Widgets Qml Quick Network Xml)

include(FeatureSummary)

# Add subdirectories
add_subdirectory(translations)
add_subdirectory(src)

# Display feature summary
feature_summary(WHAT ALL FATAL_ON_MISSING_REQUIRED_PACKAGES)
