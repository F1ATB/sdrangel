project(demodfreedv)

set(freedv_SOURCES
    freedvdemod.cpp
    freedvdemodbaseband.cpp
    freedvdemodsink.cpp
    freedvdemodsettings.cpp
    freedvdemodwebapiadapter.cpp
	freedvplugin.cpp
)

set(freedv_HEADERS
	freedvdemod.h
    freedvdemodbaseband.h
    freedvdemodsink.h
    freedvdemodsettings.h
    freedvdemodwebapiadapter.h
	freedvplugin.h
)

include_directories(
    ${CMAKE_SOURCE_DIR}/swagger/sdrangel/code/qt5/client
    ${CODEC2_INCLUDE_DIR}
)

if(NOT SERVER_MODE)
    set(freedv_SOURCES
        ${freedv_SOURCES}
        freedvdemodgui.cpp
	    freedvdemodgui.ui
    )
    set(freedv_HEADERS
        ${freedv_HEADERS}
        freedvdemodgui.h
    )
    set(TARGET_NAME demodfreedv)
    set(TARGET_LIB "Qt5::Widgets")
    set(TARGET_LIB_GUI "sdrgui")
    set(INSTALL_FOLDER ${INSTALL_PLUGINS_DIR})
else()
    set(TARGET_NAME demodfreedvsrv)
    set(TARGET_LIB "")
    set(TARGET_LIB_GUI "")
    set(INSTALL_FOLDER ${INSTALL_PLUGINSSRV_DIR})
endif()

add_library(${TARGET_NAME} SHARED
	${freedv_SOURCES}
)

if(ENABLE_EXTERNAL_LIBRARIES)
    add_dependencies(${TARGET_NAME} codec2)
endif()

target_link_libraries(${TARGET_NAME}
    Qt5::Core
    ${TARGET_LIB}
	sdrbase
	${TARGET_LIB_GUI}
    swagger
    ${CODEC2_LIBRARIES}
)

install(TARGETS ${TARGET_NAME} DESTINATION ${INSTALL_FOLDER})
