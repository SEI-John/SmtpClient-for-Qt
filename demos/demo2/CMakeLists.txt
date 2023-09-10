cmake_minimum_required(VERSION 3.16)
project(demo2 VERSION 1.0 LANGUAGES C CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOUIC ON)

find_package(QT NAMES Qt5 Qt6 REQUIRED COMPONENTS Core)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Gui)
find_package(Qt${QT_VERSION_MAJOR} OPTIONAL_COMPONENTS Widgets)

qt_standard_project_setup()

qt_add_executable(demo2 WIN32 MACOSX_BUNDLE
    demo2.cpp
    sendemail.cpp sendemail.h sendemail.ui
)
target_include_directories(demo2 PRIVATE
    ../../../build/SMTPEmail-Desktop-Debug
)

target_link_libraries(demo2 PRIVATE
    Qt::Core
    Qt::Gui
)

if((QT_VERSION_MAJOR GREATER 4))
    target_link_libraries(demo2 PRIVATE
        Qt::Widgets
    )
endif()

if(WIN32 AND CONFIG(release,debug OR release))
    target_link_libraries(demo2 PRIVATE
        # Remove: L${CMAKE_CURRENT_SOURCE_DIR}/../../../build/SMTPEmail-Desktop-Debug/release/
        SMTPEmail
    )
endif()

if((NOT (WIN32 AND CONFIG(release,debug OR release))) AND (WIN32 AND CONFIG(debug,debug OR release)))
    target_link_libraries(demo2 PRIVATE
        # Remove: L/debug/
        SMTPEmail
    )
endif()

if(((NOT (WIN32 AND CONFIG(release,debug OR release))) AND (NOT (WIN32 AND CONFIG(debug,debug OR release)))) AND (UNIX))
    target_link_libraries(demo2 PRIVATE
        # Remove: L
        SMTPEmail
    )
endif()

install(TARGETS demo2
    BUNDLE DESTINATION .
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

qt_generate_deploy_app_script(
    TARGET demo2
    OUTPUT_SCRIPT deploy_script
    NO_UNSUPPORTED_PLATFORM_ERROR
)
install(SCRIPT ${deploy_script})
