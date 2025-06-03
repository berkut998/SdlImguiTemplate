cmake_minimum_required(VERSION 3.22.1...3.31)

project(SdlImguiTemplate)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
include(FetchContent)

FetchContent_Declare(
  SDL3
  GIT_REPOSITORY https://github.com/libsdl-org/SDL.git
  GIT_TAG        c6a3b5b6ef159a22c0fb9113b51f6e6df2343dfd
  FIND_PACKAGE_ARGS # this one
)

FetchContent_Declare(
  IMGUI
  GIT_REPOSITORY https://github.com/ocornut/imgui.git
  GIT_TAG dbb5eeaadffb6a3ba6a60de1290312e5802dba5a
  FIND_PACKAGE_ARGS # this one
)

FetchContent_MakeAvailable(IMGUI SDL3)

set (imguiSourceDir "${FETCHCONTENT_BASE_DIR}/imgui-src")
set (imguiSources "${FETCHCONTENT_BASE_DIR}/imgui-src/imgui.cpp"
"${FETCHCONTENT_BASE_DIR}/imgui-src/backends/imgui_impl_sdlrenderer3.cpp"
"${FETCHCONTENT_BASE_DIR}/imgui-src/backends/imgui_impl_sdl3.cpp"
"${FETCHCONTENT_BASE_DIR}/imgui-src/imgui_draw.cpp" 
"${FETCHCONTENT_BASE_DIR}/imgui-src/imgui_widgets.cpp" 
"${FETCHCONTENT_BASE_DIR}/imgui-src/imgui_tables.cpp" 
"${FETCHCONTENT_BASE_DIR}/imgui-src/imgui_demo.cpp")#can remove imgui_demo.cpp need only like example


add_executable(SdlImguiTemplate main.cpp ${imguiSources} )
target_include_directories(SdlImguiTemplate PRIVATE "${CMAKE_SOURCE_DIR}" "${imguiSourceDir}")
target_link_libraries(SdlImguiTemplate PRIVATE SDL3::SDL3)
if (WIN32)
        add_custom_command (
            TARGET "SDL_Camera" POST_BUILD
            COMMAND "${CMAKE_COMMAND}" -E copy -t "$<TARGET_FILE_DIR:SDL_Camera>"
                    "$<TARGET_RUNTIME_DLLS:SDL_Camera>" USES_TERMINAL COMMAND_EXPAND_LISTS
        )
endif ()






