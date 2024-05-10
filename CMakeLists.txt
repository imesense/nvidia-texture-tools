cmake_minimum_required(VERSION 3.5.0)
project(NV)
enable_testing()

set(NV_CMAKE_DIR "${NV_SOURCE_DIR}/cmake")
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${NV_CMAKE_DIR}")

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

include(${NV_CMAKE_DIR}/OptimalOptions.cmake)

if(CMAKE_BUILD_TYPE MATCHES "debug")
    set(CMAKE_DEBUG_POSTFIX "_d" CACHE STRING "Postfix for debug build libraries.")
    add_definitions(-D_DEBUG=1)
endif()


add_subdirectory(extern)

add_subdirectory(src)

# These files should only be installed when creating packages.
install(FILES
	LICENSE
	README.md
	DESTINATION share/doc/nvtt)

# Add packaging support
include(InstallRequiredSystemLibraries)

if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
	set(CPACK_GENERATOR "TGZ;DEB")
endif(CMAKE_SYSTEM_NAME STREQUAL "Linux")

set(CPACK_PACKAGE_NAME "nvidia-texture-tools")
set(CPACK_PACKAGE_VERSION_MAJOR "2")
set(CPACK_PACKAGE_VERSION_MINOR "1")
set(CPACK_PACKAGE_VERSION_PATCH "2")
set(CPACK_PACKAGE_VERSION "2.1.2")
set(CPACK_PACKAGE_CONTACT "Ignacio Castaño <castano@gmail.com>")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Texture processing tools with support for Direct3D 10 and 11 formats.")

set(CPACK_PACKAGE_DESCRIPTION_FILE "${NV_SOURCE_DIR}/README.md")
set(CPACK_RESOURCE_FILE_LICENSE "${NV_SOURCE_DIR}/LICENSE")

# NSIS options:
if(WIN32)
	set(CPACK_NSIS_DISPLAY_NAME "${CPACK_PACKAGE_VENDOR}\\\\NVIDIA Texture Tools 2.1")
	set(CPACK_PACKAGE_INSTALL_DIRECTORY "${CPACK_PACKAGE_VENDOR}\\\\NVIDIA Texture Tools 2.1")
	set(CPACK_PACKAGE_ICON "${NV_SOURCE_DIR}\\\\project\\\\vc2017\\\\nvcompress\\\\nvidia.ico")
endif(WIN32)

include(CPack)

