cmake_minimum_required(VERSION 3.8)
project(orca)
## This is for testing only, you won't be able to build all code with this cmake file
## There is no plan to support cmake to build all code.



set(CMAKE_CXX_STANDARD 11)
set(CMAKE_C_STANDARD 11)

set(CMAKE_BUILD_TYPE DEBUG)
set(CMAKE_C_FLAGS "-DLIBDISCORD_DEBUG -D_XOPEN_SOURCE=700")
set(CMAKE_CXX_FLAGS "-DLIBDISCORD_DEBUG -D_XOPEN_SOURCE=700 -Wno-c++11-compat-deprecated-writable-strings")

set(CMAKE_EXE_LINKER_FLAGS "")

set(JSON_SCANF
        json-common.h
        jsmn.h
        ntl.c
        ntl.h
        json-scanf.c
        json-printf.c
        json-string.c
        json-scanf.h)

set(SOURCE_FILES
        curl-websocket-utils.c
        curl-websocket.c
        curl-websocket.h
        discord-common.h
        discord-public.cpp
        discord-public-guild.cpp
        discord-public-user.cpp
        discord-websockets.cpp
        libdiscord.h
        settings.c
        settings.h
        ${JSON_SCANF})

set(ORKA_FILES
        orka-utils.c
        orka-utils.h)

set(GITHUB_FILES
        http-common.h
        http-common.c
        github-v3.hpp
        github-v3-user-agent.cpp
        github-v3-git-op.cpp
        settings.h
        settings.c
        ${JSON_SCANF}
        ${ORKA_FILES})

set(CEE_FILES
        http-common.h
        http-common.c
        orka-user-agent.hpp
        orka-user-agent.cpp
        settings.h
        settings.c
        ${JSON_SCANF})


set(ORCA_INCLUDE_PATH  ${CMAKE_SOURCE_DIR})

include_directories(${CMAKE_SOURCE_DIR})


add_executable(test-api.exe ${SOURCE_FILES} test/test-api.cpp)
target_link_libraries(test-api.exe curl ssl crypto)

add_executable(test-ws.exe ${SOURCE_FILES} test/test-ws.cpp)
add_executable(test-json-scanf.exe ${JSON_SCANF} test/test-json-scanf.c)

add_executable(test-json-scanf-array.exe ${JSON_SCANF} ${ORKA_FILES}
        test/test-json-scanf-array.c)
target_link_libraries(test-json-scanf-array.exe m)

add_executable(test-json-string.exe ${JSON_SCANF} ${ORKA_FILES}
        test/test-json-string.c)
target_link_libraries(test-json-string.exe m)

add_executable(test-git.exe ${GITHUB_FILES}  test/test-git.cpp)
target_link_libraries(test-git.exe curl ssl crypto)

add_executable(test-git2.exe ${GITHUB_FILES} settings.c ${ORKA_FILES} test/test-git2.cpp)
target_link_libraries(test-git2.exe curl ssl crypto)

add_executable(test-cee.exe ${CEE_FILES} settings.c ${ORKA_FILES} test/test-cee.cpp)
target_link_libraries(test-cee.exe curl ssl crypto)
