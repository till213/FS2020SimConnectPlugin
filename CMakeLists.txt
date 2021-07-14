set(PLUGIN_NAME "FS2020SimConnectPlugin")

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_library(${PLUGIN_NAME} SHARED
        ${PLUGIN_SOURCES}
    )
else()
    add_library(${PLUGIN_NAME} SHARED
        ${PLUGIN_SOURCES}
    )
endif()

# SimConnect
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/3rdParty/cmake")
find_package(SimConnect REQUIRED)

target_sources(${PLUGIN_NAME}
    PRIVATE
        src/FS2020SimConnectPlugin.cpp
        src/FS2020SimConnectPlugin.h
        src/SimConnectType.h
        src/SimConnectPosition.cpp
        src/SimConnectPosition.h
        src/SimConnectEngineReply.cpp
        src/SimConnectEngineReply.h
        src/SimConnectEngineRequest.cpp
        src/SimConnectEngineRequest.h
        src/SimConnectPrimaryFlightControl.cpp
        src/SimConnectPrimaryFlightControl.h
        src/SimConnectSecondaryFlightControl.cpp
        src/SimConnectSecondaryFlightControl.h
        src/SimConnectAircraftHandle.cpp
        src/SimConnectAircraftHandle.h
        src/SimConnectLight.cpp
        src/SimConnectLight.h
        src/SimConnectAircraftInfo.cpp
        src/SimConnectAircraftInfo.h
        src/SimConnectFlightPlan.cpp
        src/SimConnectFlightPlan.h
        src/SimConnectSimulationTime.cpp
        src/SimConnectSimulationTime.h
        src/SimConnectAI.cpp
        src/SimConnectAI.h
        src/EventWidget.cpp
        src/EventWidget.h
    )

# Copy SimConnect.dll to bin directory (for development)
add_custom_command(TARGET ${PLUGIN_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        $<TARGET_FILE:SimConnect>
        $<TARGET_FILE_DIR:${PLUGIN_NAME}>/../../)

set(PLUGIN_LIBS
    Qt${QT_VERSION_MAJOR}::Widgets Kernel Model SkyConnect tsl::ordered_map SimConnect
)

target_link_libraries(${PLUGIN_NAME} PRIVATE ${PLUGIN_LIBS})

target_include_directories(${PLUGIN_NAME} PUBLIC
    "${CMAKE_CURRENT_BINARY_DIR}"
)
