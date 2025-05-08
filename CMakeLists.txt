cmake_minimum_required(VERSION 3.10)
project(lg)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O3 -Wall -pthread -fpic ${CMAKE_CPP_FLAGS}")

set(SOVERSION 1)

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

include_directories(
    ${CMAKE_FIND_ROOT_PATH}/usr/include
    ${CMAKE_FIND_ROOT_PATH}/usr/include/${CMAKE_LIBRARY_ARCHITECTURE}
)

set(OBJ_LGPIO
    lgCtx.c
    lgDbg.c
    lgErr.c
    lgGpio.c
    lgHdl.c
    lgI2C.c
    lgNotify.c
    lgPthAlerts.c
    lgPthTx.c
    lgSerial.c
    lgSPI.c
    lgThread.c
    lgUtil.c
)

set(OBJ_RGPIO
    rgpio.c
    lgCfg.c
    lgErr.c
    lgDbg.c
    lgMD5.c
)

set(OBJ_RGPIOD
    lgCfg.c
    lgCmd.c
    lgExec.c
    lgFile.c
    lgMD5.c
    lgPthSocket.c
    lgScript.c
)

set(OBJ_RGS
    lgCmd.c
    lgCfg.c
    lgDbg.c
    lgErr.c
    lgMD5.c
)

set(DOCS
    DOC/src/defs/rgs.def
    DOC/src/defs/rgpiod.def
    DOC/src/defs/permits.def
    DOC/src/defs/scripts.def
    lgpio.h
    rgpio.h
)

add_library(lgpio SHARED ${OBJ_LGPIO})
set_target_properties(lgpio PROPERTIES
    VERSION ${SOVERSION}
    SOVERSION ${SOVERSION}
)

add_library(rgpio SHARED ${OBJ_RGPIO})
set_target_properties(rgpio PROPERTIES
    VERSION ${SOVERSION}
    SOVERSION ${SOVERSION}
)

add_executable(rgpiod rgpiod.c ${OBJ_RGPIOD})
target_link_libraries(rgpiod lgpio pthread rt)

add_executable(rgs rgs.c ${OBJ_RGS})

add_custom_target(docs DEPENDS ${DOCS})

# add_custom_target(all DEPENDS lgpio rgpio rgpiod rgs docs)

install(TARGETS lgpio DESTINATION lib)
install(TARGETS rgpio DESTINATION lib)
install(TARGETS rgpiod DESTINATION bin)
install(TARGETS rgs DESTINATION bin)

install(FILES lgpio.h rgpio.h DESTINATION include)
install(FILES DOC/src/defs/rgs.def DOC/src/defs/rgpiod.def DOC/src/defs/permits.def DOC/src/defs/scripts.def DESTINATION share/man)
