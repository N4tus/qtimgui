cmake_minimum_required (VERSION 3.20.2)
project(qtimgui)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_PREFIX_PATH $ENV{QT_CMAKE_PREFIX_PATH})
find_package(Qt6 COMPONENTS Core Quick Gui Widgets OpenGLWidgets REQUIRED)

set(
    qt_imgui_sources
    ImGuiRenderer.h
    ImGuiRenderer.cpp
    QtImGui.h
    QtImGui.cpp
)

# qt_imgui_quick: library with a qt renderer for Qml / QtQuick applications
add_library(qt_imgui_quick STATIC ${qt_imgui_sources})
target_include_directories(qt_imgui_quick PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(
    qt_imgui_quick
    PUBLIC
    imgui
    Qt::Gui
    Qt6::OpenGLWidgets )

if (ANDROID)
    target_link_libraries(qt_imgui_quick PUBLIC log dl GLESv2 z)
endif()

# qt_imgui_widget: library with a qt renderer for classic Qt Widget applications
add_library(qt_imgui_widgets STATIC ${qt_imgui_sources})
target_include_directories(qt_imgui_widgets PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(
    qt_imgui_widgets
    PUBLIC
    imgui
    Qt::Widgets
    Qt6::OpenGLWidgets)

if (ANDROID)
    target_link_libraries(qt_imgui_widgets PUBLIC log dl GLESv2 z)
endif()
target_compile_definitions(qt_imgui_widgets PUBLIC QT_WIDGETS_LIB)
