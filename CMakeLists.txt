cmake_minimum_required(VERSION 3.16)
project(SmtpClient VERSION 1.0 LANGUAGES C CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

find_package(QT NAMES Qt5 Qt6 REQUIRED COMPONENTS Core)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Gui Network)

qt_standard_project_setup()

qt_add_library(SmtpClient
    src/SmtpMime
    src/emailaddress.cpp src/emailaddress.h
    src/mimeattachment.cpp src/mimeattachment.h
    src/mimecontentformatter.cpp src/mimecontentformatter.h
    src/mimefile.cpp src/mimefile.h
    src/mimehtml.cpp src/mimehtml.h
    src/mimeinlinefile.cpp src/mimeinlinefile.h
    src/mimemessage.cpp src/mimemessage.h
    src/mimemultipart.cpp src/mimemultipart.h
    src/mimepart.cpp src/mimepart.h
    src/mimetext.cpp src/mimetext.h
    src/quotedprintable.cpp src/quotedprintable.h
    src/smtpclient.cpp src/smtpclient.h
    src/smtpexports.h
)
target_compile_definitions(SmtpClient PUBLIC
    SMTP_BUILD
)

target_link_libraries(SmtpClient PUBLIC
    Qt::Core
    Qt::Gui
    Qt::Network
)

install(TARGETS SmtpClient
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    FRAMEWORK DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
