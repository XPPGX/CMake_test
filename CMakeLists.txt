cmake_minimum_required(VERSION 3.0)
project(test_include)

set(SRC_DIR src)
set(INC_DIR include)
set(MAIN_DIR main)


set(CMAKE_BUILD_TYPE "DEBUG")
set(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggbb")
set(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")

set(NULL "")

include_directories(${INC_DIR})#讓CMake知道在編譯的時候有一個存放檔案的路徑
include_directories(${SRC_DIR})

aux_source_directory(${INC_DIR} head)
aux_source_directory(${SRC_DIR} source)


#"main"、"src"、"include"三個資料夾應該一直存在，不論裡面是否有檔案。
#當你的main.c沒有要使用自己編寫的lib時，src與include的資料夾應該是空的。
#當你的main.c要使用自己編寫的lib時，src與include的資料夾中應該要有檔案。

if(head STREQUAL NULL AND source STREQUAL NULL)
    add_executable(main ${MAIN_DIR}/main.c)
else()
    add_library(libs ${head} ${source})
    add_executable(main ${MAIN_DIR}/main.c)
    target_link_libraries(main libs)
endif()