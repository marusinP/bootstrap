# Generated Cmake file begin
cmake_minimum_required(VERSION 3.1)

project(lualogging C CXX)
set(lualogging_VERSION 1.3.0-1)

set(ENV{LUA_DIR} ${CMAKE_INSTALL_PREFIX})
find_package(Lua REQUIRED)

## INSTALL DEFAULTS (Relative to CMAKE_INSTALL_PREFIX)
# Primary paths
set(INSTALL_BIN bin CACHE PATH "Where to install binaries to.")
set(INSTALL_LIB lib CACHE PATH "Where to install libraries to.")
set(INSTALL_ETC etc CACHE PATH "Where to store configuration files")
set(INSTALL_SHARE share CACHE PATH "Directory for shared data.")

set(INSTALL_LMOD ${INSTALL_LIB}/lua/${LUA_VERSION_MAJOR}.${LUA_VERSION_MINOR} CACHE PATH "Directory to install Lua modules.")
set(INSTALL_CMOD ${INSTALL_LIB}/lua/${LUA_VERSION_MAJOR}.${LUA_VERSION_MINOR} CACHE PATH "Directory to install Lua binary modules.")

## Platform detection
# By default use system name
set(DETECTED_PLATFORM ${CMAKE_SYSTEM_NAME})
string(TOLOWER ${DETECTED_PLATFORM} DETECTED_PLATFORM)

# Overrides for more specific platforms
if (DETECTED_PLATFORM STREQUAL "darwin")
    set(DETECTED_PLATFORM macosx)
elseif (MINGW)
    set(DETECTED_PLATFORM mingw32)
elseif (MSYS)
    set(DETECTED_PLATFORM msys)
elseif (CYGWIN)
    set(DETECTED_PLATFORM cygwin)
elseif (WIN32)
    set(DETECTED_PLATFORM win32)
endif()

set(BUILD_COPY_DIRECTORIES "doc/html")
set(BUILD_INSTALL_lua ${BUILD_INSTALL_lua};logging;logging.console;logging.email;logging.file;logging.rolling_file;logging.socket;logging.sql)
set(BUILD_INSTALL_lua_logging.console_DST logging)
set(BUILD_INSTALL_lua_logging.console_RENAME console.lua)
set(BUILD_INSTALL_lua_logging.console_SRC src/logging/console.lua)
set(BUILD_INSTALL_lua_logging.email_DST logging)
set(BUILD_INSTALL_lua_logging.email_RENAME email.lua)
set(BUILD_INSTALL_lua_logging.email_SRC src/logging/email.lua)
set(BUILD_INSTALL_lua_logging.file_DST logging)
set(BUILD_INSTALL_lua_logging.file_RENAME file.lua)
set(BUILD_INSTALL_lua_logging.file_SRC src/logging/file.lua)
set(BUILD_INSTALL_lua_logging.rolling_file_DST logging)
set(BUILD_INSTALL_lua_logging.rolling_file_RENAME rolling_file.lua)
set(BUILD_INSTALL_lua_logging.rolling_file_SRC src/logging/rolling_file.lua)
set(BUILD_INSTALL_lua_logging.socket_DST logging)
set(BUILD_INSTALL_lua_logging.socket_RENAME socket.lua)
set(BUILD_INSTALL_lua_logging.socket_SRC src/logging/socket.lua)
set(BUILD_INSTALL_lua_logging.sql_DST logging)
set(BUILD_INSTALL_lua_logging.sql_RENAME sql.lua)
set(BUILD_INSTALL_lua_logging.sql_SRC src/logging/sql.lua)
set(BUILD_INSTALL_lua_logging_RENAME logging.lua)
set(BUILD_INSTALL_lua_logging_SRC src/logging.lua)

install(DIRECTORY ${BUILD_COPY_DIRECTORIES} DESTINATION ${INSTALL_SHARE}/lualogging)

function(build_install KEYS DIR)
    list(REMOVE_DUPLICATES KEYS)

    foreach(KEY ${${KEYS}})
        set(BASE_NAME ${KEYS}_${KEY})
        install(FILES ${${BASE_NAME}_SRC} DESTINATION ${DIR}/${${BASE_NAME}_DST} RENAME ${${BASE_NAME}_RENAME})
    endforeach(KEY)
endfunction(build_install)

build_install(BUILD_INSTALL_lua ${INSTALL_LMOD})
build_install(BUILD_INSTALL_lib ${INSTALL_LIB})
build_install(BUILD_INSTALL_conf ${INSTALL_ETC})
build_install(BUILD_INSTALL_bin ${INSTALL_BIN})



