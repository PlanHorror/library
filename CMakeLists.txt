cmake_minimum_required(VERSION 3.16)

project(student-manager VERSION 0.1 LANGUAGES CXX)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets Sql)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets Sql)

set(PROJECT_SOURCES

        GUI/bookwidget.cpp
        GUI/imagewidget.cpp
        GUI/loginwidget.cpp
        GUI/mainwindow.cpp
        GUI/publisherwidget.cpp
        GUI/signupwidget.cpp
        GUI/startwidget.cpp
        GUI/studentwidget.cpp
        book.cpp
        controller.cpp
        database.cpp
        librarian.cpp
        main.cpp
        publisher.cpp
        student.cpp
        user.cpp
        GUI/bookwidget.h
        GUI/header.h
        GUI/imagewidget.h
        GUI/indexs.h
        GUI/loginwidget.h
        GUI/mainwindow.h
        GUI/publisherwidget.h
        GUI/signupwidget.h
        GUI/startwidget.h
        GUI/studentwidget.h
        book.h
        controller.h
        database.h
        GUI/indexs.h
        librarian.h
        publisher.h
        student.h
        user.h
        icons
        ray2.db
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(student-manager
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
        GUI/mainwindow.ui GUI/mainwindow.h GUI/mainwindow.cpp
        GUI/indexs.h controller.h controller.cpp

    )
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET student-manager APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation
else()
    if(ANDROID)
        add_library(student-manager SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
        add_executable(student-manager
            ${PROJECT_SOURCES}
        )
    endif()
endif()

target_link_libraries(student-manager PRIVATE Qt${QT_VERSION_MAJOR}::Widgets Qt${QT_VERSION_MAJOR}::Sql)

# Qt for iOS sets MACOSX_BUNDLE_GUI_IDENTIFIER automatically since Qt 6.1.
# If you are developing for iOS or macOS you should consider setting an
# explicit, fixed bundle identifier manually though.
if(${QT_VERSION} VERSION_LESS 6.1.0)
  set(BUNDLE_ID_OPTION MACOSX_BUNDLE_GUI_IDENTIFIER com.example.student-manager)
endif()
set_target_properties(student-manager PROPERTIES
    ${BUNDLE_ID_OPTION}
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

include(GNUInstallDirs)
install(TARGETS student-manager
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

if(QT_VERSION_MAJOR EQUAL 6)
    qt_finalize_executable(student-manager)
endif()
