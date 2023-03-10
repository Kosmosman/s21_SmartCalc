cmake_minimum_required(VERSION 3.5)

project(TheBestCalculatorInTheWorld VERSION 0.1 LANGUAGES C CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets)

set(PROJECT_SOURCES
        frontend/main.cpp
        frontend/mainwindow.cpp
        frontend/credit_calculator.cpp
        frontend/credit_c_a.cpp
        frontend/graphics.cpp
        frontend/mainwindow.h
        frontend/credit_calculator.h
        frontend/credit_c_a.h
        frontend/graphics.h
        frontend/mainwindow.ui
        frontend/credit_c_a.ui
        frontend/credit_calculator.ui
        frontend/graphics.ui
        backend/s21_calculate.h
        backend/s21_credit_calculator.c
        backend/s21_parcer.c
        backend/s21_stack.c
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(TheBestCalculatorInTheWorld
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
    )
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET TheBestCalculatorInTheWorld APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation
else()
    if(ANDROID)
        add_library(TheBestCalculatorInTheWorld SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
        add_executable(TheBestCalculatorInTheWorld
            ${PROJECT_SOURCES}
        )
    endif()
endif()

target_link_libraries(TheBestCalculatorInTheWorld PRIVATE Qt${QT_VERSION_MAJOR}::Widgets)

set_target_properties(TheBestCalculatorInTheWorld PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER my.example.com
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

add_subdirectory(QCustomPlot-library)

target_link_libraries(${PROJECT_NAME} PRIVATE qcustomplot)
target_compile_definitions(${PROJECT_NAME} PRIVATE QCUSTOMPLOT_USE_LIBRARY)

if(QT_VERSION_MAJOR EQUAL 6)
    qt_finalize_executable(TheBestCalculatorInTheWorld)
endif()
