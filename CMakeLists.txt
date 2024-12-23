 cmake_minimum_required (VERSION 3.8)

# 如果支持，请为 MSVC 编译器启用热重载。
if (POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

project ("VulkanTutorial")
  
set(CMAKE_CXX_STANDARD 17)   #include <optional>  类模板

# Vulkan SDK 路径
set(VULKAN_SDK "C:/VulkanSDK/1.3.296.0")
include_directories("${VULKAN_SDK}/Include")       # Vulkan SDK Include
link_directories   ("${VULKAN_SDK}/Lib")           # Vulkan SDK Library
include_directories("${CMAKE_CURRENT_SOURCE_DIR}/deps/include")  # 搜索目录
link_directories   ("${CMAKE_CURRENT_SOURCE_DIR}/deps/lib")       # 连接器搜索目录

# 复制Shaders编译目录
file(GLOB Shaders "${CMAKE_CURRENT_SOURCE_DIR}/deps/shaders/*") 
file(COPY ${Shaders} DESTINATION  ${CMAKE_CURRENT_BINARY_DIR}/shaders )
file(GLOB Textures "${CMAKE_CURRENT_SOURCE_DIR}/deps/textures/*") 
file(COPY ${Textures} DESTINATION  ${CMAKE_CURRENT_BINARY_DIR}/textures )

# 设置 src 目录路径
set(SRC_DIR "${CMAKE_CURRENT_SOURCE_DIR}/src")

file(GLOB SUBDIRS  RELATIVE  ${SRC_DIR} ${SRC_DIR}/*) # 遍历 src 文件夹中的子目录 RELATIVE获取文件夹名称

foreach(SUBDIR ${SUBDIRS})   
   message(-->>>>> ${SUBDIR}.exe)
   file(GLOB SrcFile  "src/${SUBDIR}/*.h"  "src/${SUBDIR}/*.hpp"  "src/${SUBDIR}/*.cpp" "src/${SUBDIR}/*.c" )
   add_executable (${SUBDIR}  ${SrcFile} )
endforeach()
 
 

 
 
