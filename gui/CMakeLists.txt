cmake_minimum_required(VERSION 3.10)
project(lvgl)

set(CMAKE_C_STANDARD 99)#C99 # lvgl officially support C99 and above
set(CMAKE_CXX_STANDARD 17)#C17
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

include_directories(/usr/include/freetype2)  # 添加头文件搜索路径
link_directories(/usr/lib)                   # 链接库路径（可选）

add_subdirectory(lv_100ask_xz_ai)
add_subdirectory(lvgl)
target_include_directories(lvgl PUBLIC ${PROJECT_SOURCE_DIR})

#add_subdirectory(lv_100ask_xz_ai)

add_executable(main main.c mouse_cursor_icon.c)

include(${CMAKE_CURRENT_LIST_DIR}/lvgl/tests/FindLibDRM.cmake)
include_directories(${Libdrm_INCLUDE_DIRS})

find_package(SDL2)
find_package(SDL2_image)
include_directories(${CMAKE_SOURCE_DIR}/lv_100ask_xz_ai ${SDL2_INCLUDE_DIRS} ${SDL2_IMAGE_INCLUDE_DIRS})

target_link_libraries(main lv_100ask_xz_ai  lvgl lvgl::examples lvgl::demos lvgl::thorvg ${SDL2_LIBRARIES} ${SDL2_IMAGE_LIBRARIES} ${Libdrm_LIBRARIES} m pthread freetype png)
add_custom_target (run COMMAND ${EXECUTABLE_OUTPUT_PATH}/lv_xiaozhiui DEPENDS main)

