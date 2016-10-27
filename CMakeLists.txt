# Copyright (C) 2013 LuaDist
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

# This is a straightforward bootstrap for LuaDist.
project ( luadist_bootstrap NONE )
cmake_minimum_required ( VERSION 2.8 )
include ( ExternalProject )

## SETTINGS
# Build Release by default
if ( NOT CMAKE_BUILD_TYPE)
  set ( CMAKE_BUILD_TYPE "MinSizeRel" CACHE STRING "Choose the type of build, options are: Debug Release RelWithDebInfo MinSizeRel." FORCE )
endif ()

# Bootstrap destination
set ( BOOT_PREFIX ${CMAKE_INSTALL_PREFIX} )

# Install dists required for bootstrap by default
# Format: package (repository name), owner (github organization / user), git tag (version)
set ( DISTS
  "lua" "LuaDist-core" "5.2.4-1"
  "luafilesystem" "LuaDist2" "1.6.3-2"
  "luasocket" "LuaDist2" "3.0rc1-2"
  "penlight" "LuaDist2" "1.3.3.luadist-1"
  "lualogging" "LuaDist2" "1.3.0-1"
  "rocksolver" "LuaDist2" "0.4.2-1"
  "rockspec2cmake" "LuaDist-core" "0.3-1"
  "zlib" "LuaDist-core" "1.2.6-1"
  "lua-git" "LuaDist-core" "0.5-1"
  "luadist2" "LuaDist-core" "0.6-3"
)

# Generate cache file with configuration from config.in
configure_file ( config.in ${CMAKE_CURRENT_BINARY_DIR}/config.cmake )

macro ( install_dist _DIST _OWNER _TAG )
  set ( _DIST_SOURCE GIT_REPOSITORY http://github.com/${_OWNER}/${_DIST}.git GIT_TAG ${_TAG} )

  if (EXISTS ${CMAKE_SOURCE_DIR}/cmake/${_DIST}/CMakeLists.txt)
    set(PATCH_COMMAND "${CMAKE_COMMAND}" -E copy "${CMAKE_SOURCE_DIR}/cmake/${_DIST}/CMakeLists.txt" "${CMAKE_INSTALL_PREFIX}/tmp/${_DIST}/src/${_DIST}/")
  else()
    set(PATCH_COMMAND "")
  endif()

  # Build project using the generated settings in cache
  ExternalProject_Add( ${_DIST}
      PREFIX ${_DIST}
      ${_DIST_SOURCE}
      BINARY_DIR ${_DIST}
      INSTALL_DIR ${CMAKE_INSTALL_PREFIX}
      CMAKE_ARGS -C ${CMAKE_CURRENT_BINARY_DIR}/config.cmake
      PATCH_COMMAND ${PATCH_COMMAND}
      DEPENDS ${_PREV}
    )
    set ( _PREV ${_DIST})
    list ( APPEND DEPS ${_DIST} )
endmacro ()

list(LENGTH DISTS DISTS_CNT)
math(EXPR DISTS_CNT "${DISTS_CNT} / 3 - 1")

# Build all modules one after another
foreach ( BASE_IDX RANGE ${DISTS_CNT})
  math(EXPR IDX "3 * ${BASE_IDX}")
  list(GET DISTS ${IDX} DIST )
  math(EXPR IDX "${IDX} + 1")
  list(GET DISTS ${IDX} OWNER )
  math(EXPR IDX "${IDX} + 1")
  list(GET DISTS ${IDX} TAG )

  install_dist ( ${DIST} ${OWNER} ${TAG} )
endforeach ()

# Make a fake target
add_custom_target ( install
  COMMAND echo "Bootstrap finished: ${BOOT_PREFIX}."
  DEPENDS ${_PREV}
)
