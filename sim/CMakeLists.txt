project(noslink C)
cmake_minimum_required(VERSION 2.6.4)

# NOS Engine headers as system headers, as in /usr/include on the Linux image
include_directories(SYSTEM ${NOSENGINE_INCLUDE_DIRS})
include_directories(inc
                    ${MISSION_SOURCE_DIR}/components/hwlib/fsw/public_inc
                    ${MISSION_SOURCE_DIR}/osal/src/os/inc
                    ${MISSION_SOURCE_DIR}/psp/fsw/inc
                    )

aux_source_directory(src NOSLINK_SRC)

add_library(noslink STATIC ${NOSLINK_SRC})
if(APPLE)
    # no libgcov on macOS (and ComponentSettings.cmake drops coverage there)
    target_link_libraries(noslink ${NOSENGINE_LIBRARIES})
else()
    target_link_libraries(noslink ${NOSENGINE_LIBRARIES} gcov)
endif()

