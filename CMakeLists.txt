cmake_minimum_required(VERSION 3.21)
project(vulkan_test1)

set(CMAKE_CXX_STANDARD 14)

find_package(Vulkan REQUIRED)

add_executable(vulkan_test1)

target_sources(vulkan_test1 PRIVATE main.cpp)

target_link_libraries(vulkan_test1 PRIVATE Vulkan::Vulkan)

if(WIN32)
    find_path(Vulkan_INCLUDE_DIR
            NAMES vulkan/vulkan.h
            HINTS
            "$ENV{VULKAN_SDK}/Include"
            )
elseif(LINUX)
    find_library(Vulkan_LIBRARY names vulkan hints "$ENV{VULKAN_SDK}/lib")
    if(not Vulkan_FOUND)
        if(Vulkan_LIBRARY)
            set(Vulkan_FOUND on)
            message("Using bundled Vulkan library version")
        endif()
    endif()
endif()

if(Vulkan_FOUND)
    message("vulkan found - ${Vulkan_LIBRARY}")
else()
    message("vulkan not found")
endif()