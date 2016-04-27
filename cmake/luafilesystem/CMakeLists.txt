# Generated Cmake file begin
cmake_minimum_required(VERSION 3.1)

project(LuaFileSystem C CXX)
set(LuaFileSystem_VERSION 1.6.3-2)

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

set(BUILD_COPY_DIRECTORIES "doc" "tests")
set(lfs_SOURCES src/lfs.c)

install(DIRECTORY ${BUILD_COPY_DIRECTORIES} DESTINATION ${INSTALL_SHARE}/LuaFileSystem)

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


add_library(lfs SHARED ${lfs_SOURCES})

foreach(LIBRARY ${lfs_LIB_NAMES})
    find_library(lfs_${LIBRARY} ${LIBRARY} ${lfs_LIBDIRS})
    list(APPEND lfs_LIBRARIES ${LIBRARY})
endforeach(LIBRARY)

target_include_directories(lfs PRIVATE ${lfs_INCDIRS} ${LUA_INCLUDE_DIRS} ${LUA_INCLUDE_DIR})
target_compile_definitions(lfs PRIVATE ${lfs_DEFINES})
target_link_libraries(lfs PRIVATE ${lfs_LIBRARIES} ${LUA_LIBRARIES})
# Do not prefix "lib" before target name
set_target_properties(lfs PROPERTIES PREFIX "")
set_target_properties(lfs PROPERTIES OUTPUT_NAME lfs)
install(TARGETS lfs DESTINATION ${INSTALL_CMOD}/)

