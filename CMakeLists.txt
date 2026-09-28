cmake_minimum_required(VERSION 3.16)

project(QtAppTemplate VERSION 5.0 LANGUAGES CXX)

set(PROJECT_VERSION 5)
set(PROJECT_VERSION_MAJOR 0)
set(PROJECT_VERSION_MINOR 0)

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_SHARED_LIBRARY_SUFFIX "")
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

# Detect UNIX-like systems
if(UNIX AND NOT APPLE)
    if(${CMAKE_SYSTEM_NAME} MATCHES "Android")
        message("-- Configuring for Android ...")
        set(ANDROID_LINUX TRUE)
    elseif(${CMAKE_SYSTEM_NAME} MATCHES "FreeBSD")
        message("-- Configuring for FreeBSD ...")
        set(FREEBSD TRUE)
    else()
        message("-- Configuring for GNU/Linux ...")
        set(GNU_LINUX TRUE)
    endif()
endif()

# Detect Apple platforms (macOS and iOS)
if(UNIX AND APPLE)
    if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
        # Darwin covers both macOS and iOS
        if(IOS)
            message("-- Configuring for iOS ...")
            set(IOS TRUE)
        else()
            message("-- Configuring for macOS ...")
            set(MACOS TRUE)
        endif()
    endif()
endif()

# Detect Windows
if(WIN32)
    message("-- Configuring for Windows ...")
    set(WINDOWS TRUE)
endif()

if(DARWIN)
find_package(Qt6 6.10 REQUIRED COMPONENTS Quick Core Sql Qml Multimedia Bluetooth)
qt_standard_project_setup(REQUIRES 6.10)
endif(DARWIN)

if(ANDROID)
find_package(Qt6 6.10 REQUIRED COMPONENTS Quick Core Sql Qml Multimedia Bluetooth)
qt_standard_project_setup(REQUIRES 6.10)
endif(ANDROID)

if(GNU_LINUX)
find_package(Qt6 6.10 REQUIRED COMPONENTS Quick Core Sql Qml Multimedia Bluetooth)
qt_standard_project_setup(REQUIRES 6.10)
endif(GNU_LINUX)

if(WIN32)
find_package(Qt6 6.10 REQUIRED COMPONENTS Quick Core Sql Qml Multimedia Bluetooth)
qt_standard_project_setup(REQUIRES 6.10)
endif(WIN32)

set(SOURCE_DIR ./src)
set(HEADER_DIR ./include)

include_directories(${PROJECT_HEADER_DIR} ./include)

set(SOURCES
${SOURCES}
${SOURCE_DIR}/main.cpp
${SOURCE_DIR}/Logger.cpp
${SOURCE_DIR}/QtAppTemplate.cpp
${SOURCE_DIR}/Help.cpp
${SOURCE_DIR}/Controller.cpp
${SOURCE_DIR}/BluetoothInterface.cpp
${SOURCE_DIR}/CameraInterface.cpp
${SOURCE_DIR}/UserInfo.cpp
)

set(HEADERS
${HEADERS}
${HEADER_DIR}/Logger.hpp
${HEADER_DIR}/QtAppTemplate.hpp
${HEADER_DIR}/Help.hpp
${HEADER_DIR}/GraphData.hpp
${HEADER_DIR}/ApplicationTheme.hpp
${HEADER_DIR}/Controller.hpp
${HEADER_DIR}/BluetoothInterface.hpp
${HEADER_DIR}/CameraInterface.hpp
${HEADER_DIR}/Version.hpp
${HEADER_DIR}/UserInfo.hpp
)

if(IOS)
    set(RESOURCE_FILE ./resourcesmobile/QtAppTemplateMobile.qrc)
endif(IOS)

if(ANDROID_LINUX)
    set(RESOURCE_FILE ./resourcesmobile/QtAppTemplateMobile.qrc)
endif(ANDROID_LINUX)

if(GNU_LINUX)
    set(RESOURCE_FILE ./resourcesdesktop/QtAppTemplateDesktop.qrc)
endif(GNU_LINUX)

if(WIN32)
    set(RESOURCE_FILE ./resourcesdesktop/QtAppTemplateDesktop.qrc)
endif(WIN32)

if(MACOS)
    set(RESOURCE_FILE ./resourcesdesktop/QtAppTemplateDesktop.qrc)
endif(MACOS)

if(FREEBSD)
    set(RESOURCE_FILE ./resourcesdesktop/QtAppTemplateDesktop.qrc)
endif(FREEBSD)

qt_add_executable(QtAppTemplate ${SOURCES} ${HEADERS} ${RESOURCE_FILE})

set_target_properties(QtAppTemplate PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER QtAppTemplate
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_FRAMEWORK_INFO_PLIST ${CMAKE_CURRENT_SOURCE_DIR}/darwin/Info.plist
    MACOSX_BUNDLE_INFO_PLIST ${CMAKE_CURRENT_SOURCE_DIR}/darwin/Info.plist
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
    QT_ANDROID_PACKAGE_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/android
)

if(ANDROID_LINUX)
    target_link_libraries(QtAppTemplate
        PRIVATE Qt6::Quick
        PRIVATE Qt6::Core
        PRIVATE Qt6::CorePrivate
        PRIVATE Qt6::Sql
        PRIVATE Qt6::Qml
        PRIVATE Qt6::Multimedia
        PRIVATE Qt6::Bluetooth
    )
else()
    target_link_libraries(QtAppTemplate
        PRIVATE Qt6::Quick
        PRIVATE Qt6::Core
        PRIVATE Qt6::Sql
        PRIVATE Qt6::Qml
        PRIVATE Qt6::Multimedia
        PRIVATE Qt6::Bluetooth
    )
endif(ANDROID_LINUX)

install(TARGETS QtAppTemplate
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
