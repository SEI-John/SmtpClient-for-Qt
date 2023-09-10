cmake_minimum_required(VERSION 3.16)
project(demo1 VERSION 1.0 LANGUAGES C CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

find_package(QT NAMES Qt5 Qt6 REQUIRED COMPONENTS Core)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED)

qt_standard_project_setup()

qt_add_executable(demo1
    demo1.cpp
)
target_include_directories(demo1 PRIVATE
    ../../../build/SMTPEmail-Desktop-Debug
)

target_link_libraries(demo1 PRIVATE
    Qt::Core
)

if(WIN32 AND CONFIG(release,debug OR release))
    target_link_libraries(demo1 PRIVATE
        # Remove: L${CMAKE_CURRENT_SOURCE_DIR}/../../../build/SMTPEmail-Desktop-Debug/release/
        SMTPEmail
    )
endif()

if((NOT (WIN32 AND CONFIG(release,debug OR release))) AND (WIN32 AND CONFIG(debug,debug OR release)))
    target_link_libraries(demo1 PRIVATE
        # Remove: L/debug/
        SMTPEmail
    )
endif()

if(((NOT (WIN32 AND CONFIG(release,debug OR release))) AND (NOT (WIN32 AND CONFIG(debug,debug OR release)))) AND (UNIX))
    target_link_libraries(demo1 PRIVATE
        # Remove: L
        SMTPEmail
    )
endif()

install(TARGETS demo1
    BUNDLE DESTINATION .
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

qt_generate_deploy_app_script(
    TARGET demo1
    FILENAME_VARIABLE deploy_script
    NO_UNSUPPORTED_PLATFORM_ERROR
)
install(SCRIPT ${deploy_script})
