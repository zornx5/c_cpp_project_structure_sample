# Cmake 请求，FATAL_ERROR 表示失败即停
cmake_minimum_required(VERSION 3.0 FATAL_ERROR)

# 项目名/语言 (Project name / Code type)
project(c_cpp_project_structure_sample C)

# 设置 C 标准 (Setting C standard)
set(CMAKE_C_STANDARD 11)

# 发布版本 (Release Build)
if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release")
endif ()

# 要显示执行构建过程中详细的信息 (To display detailed information during the execution of the build process)
if (CMAKE_BUILD_TYPE MATCHES "Release")
    set(CMAKE_VERBOSE_MAKEFILE OFF)
elseif (CMAKE_BUILD_TYPE MATCHES "Debug")
    set(CMAKE_VERBOSE_MAKEFILE ON)
endif ()

# 定义项目版本 (project version)
set(project_VERSION_MAJOR 0) # 主版本 (Major version)
set(project_VERSION_MINOR 0) # 次版本 (Minor version)
set(project_VERSION_PATCH 1) # 补丁等级 (Patch version)

# 编译选项 (Compile option)
if (CMAKE_SYSTEM_NAME MATCHES "Linux")
    message(STATUS "Set C flags from Linux")
    set(CMAKE_C_FLAGS_DEBUG "$ENV{CFLAGS} -O0 -Wall -g -ggdb")
    set(CMAKE_C_FLAGS_RELEASE "$ENV{CFLAGS} -O3 -Wall")
    add_definitions(-DLINUX -D_THREAD_SAFE)
elseif (CMAKE_SYSTEM_NAME MATCHES "FreeBSD")
    message(STATUS "Set C flags from FreeBSD")
    add_definitions(-DFreeBSD)
elseif (APPLE OR CMAKE_SYSTEM_NAME MATCHES "Darwin")
    message(STATUS "Set C flags from MacOS")
    add_definitions(-DLINUX -DDARWIN -D_THREAD_SAFE)
elseif (WIN32 OR CMAKE_SYSTEM_NAME MATCHES "Windows")
    message(STATUS "Set C flags from Windows")
    add_definitions(-DWIN32)
else ()
    add_definitions(-DLINUX -D_THREAD_SAFE)
endif ()

# 获取系统语言 (Get system language)
set(LANGUAGE $ENV{LANG})
if (LANGUAGE AND LANGUAGE MATCHES "zh_CN.UTF-8")
    message(STATUS "操作系统是 ${CMAKE_SYSTEM}")
    message(STATUS "项目版本是 ${CMAKE_BUILD_TYPE} ${project_VERSION_MAJOR}.${project_VERSION_MINOR}.${project_VERSION_PATCH}")

    # 获取环境变量
    if (NOT INSTALL_DIR)
        set(INSTALL_DIR $ENV{PROJECT_HOME})
        message(STATUS "获取项目环境变量 [PROJECT_HOME]")
    endif ()
    if (NOT INSTALL_DIR)
        message(SEND_ERROR "没有设置项目的环境变量 [PROJECT_HOME]")
        set(INSTALL_DIR "${CMAKE_CURRENT_SOURCE_DIR}/dist")
        message(STATUS "项目将安装在 ${INSTALL_DIR}")
    endif ()

    # 设置安装路径
    set(project_DIRLIB ${INSTALL_DIR}/lib)
    set(project_DIRBIN ${INSTALL_DIR}/bin)
    message(STATUS "安装-项目动态库目录: ${project_DIRLIB}")
    message(STATUS "安装-项目可执行文件目录: ${project_DIRBIN}")

    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/dist/bin)
    # 设定编译后生成的库文件所在的目录
    set(LIBRARY_OUTPUT_PATH ${CMAKE_CURRENT_SOURCE_DIR}/dist/lib)
    # 设定编译后生成的执行文件所在的目录
    set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_SOURCE_DIR}/dist/bin)
else ()
    message(STATUS "Operating system is ${CMAKE_SYSTEM}")
    message(STATUS "project version ${CMAKE_BUILD_TYPE} ${project_VERSION_MAJOR}.${project_VERSION_MINOR}.${project_VERSION_PATCH}")

    # Get environment variables
    if (NOT INSTALL_DIR)
        set(INSTALL_DIR $ENV{PROJECT_HOME})
        message(STATUS "Get project environment variable [PROJECT_HOME]")
    endif ()
    if (NOT INSTALL_DIR)
        message(SEND_ERROR "No environment variables for the project are set [PROJECT_HOME]")
        set(INSTALL_DIR "${CMAKE_CURRENT_SOURCE_DIR}/dist")
        message(STATUS "The project will be installed in ${INSTALL_DIR}")
    endif ()

    # Set the installation path
    set(project_DIRLIB ${INSTALL_DIR}/lib)
    set(project_DIRBIN ${INSTALL_DIR}/bin)
    message(STATUS "project dynamic library directory: ${project_DIRLIB}")
    message(STATUS "project executable directory: ${project_DIRBIN}")

    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/dist/bin)
    # Set the directory where the library file generated after compilation is located
    set(LIBRARY_OUTPUT_PATH ${CMAKE_CURRENT_SOURCE_DIR}/dist/lib)
    # Set the directory where the executable file generated after compilation is located
    set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_SOURCE_DIR}/dist/bin)
endif ()

# 添加子目录 (Add subdirectory)
# 库 (library)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/src/lib/module1)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/src/lib/module2)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/src/bin)