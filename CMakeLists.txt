cmake_minimum_required(VERSION 3.16)
project(your_project_name VERSION 0.1.0 LANGUAGES CXX)

# ============================================================
# 基础配置
# ============================================================
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

message(STATUS "Install path: ${CMAKE_INSTALL_PREFIX}")

# ============================================================
# 源文件
# ============================================================
file(GLOB_RECURSE SOURCE_FILES
     CONFIGURE_DEPENDS
     src/*.cpp src/*.cc
     include/*.cpp include/*.cc
)

add_executable(${PROJECT_NAME} ${SOURCE_FILES})

# ============================================================
# 编译选项
# ============================================================
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_17)

if (MSVC)
  target_compile_options(${PROJECT_NAME} PRIVATE /W4 /permissive-)
else()
  target_compile_options(${PROJECT_NAME} PRIVATE -Wall -Wextra -Wpedantic)
endif()

target_compile_definitions(${PROJECT_NAME}
  PRIVATE
    $<$<CONFIG:Debug>:DEBUG>
    $<$<CONFIG:Release>:NDEBUG>
)

# ============================================================
# 第三方库
# ============================================================
function(add_third_party target libname target_name)
    find_package(${libname} CONFIG REQUIRED)
    target_link_libraries(${target} PRIVATE ${target_name})
endfunction()

# 使用示例：
# add_third_party(${PROJECT_NAME} fmt    fmt::fmt)
# add_third_party(${PROJECT_NAME} Eigen3 Eigen3::Eigen)

# ============================================================
# 头文件
# ============================================================
target_include_directories(${PROJECT_NAME}
  PRIVATE
    ${PROJECT_SOURCE_DIR}/include
)
