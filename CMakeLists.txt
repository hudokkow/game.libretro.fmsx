cmake_minimum_required(VERSION 3.1)
project(game.libretro.fmsx)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

find_package(Kodi REQUIRED)

include(ExternalProject)

if(CMAKE_BUILD_TYPE MATCHES Debug)
  set(LIBRETRO_DEBUG DEBUG=1)
endif()

if("${CORE_SYSTEM_NAME}" STREQUAL "windows")
  set(BUILD_COMMAND mingw32-make platform=win ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "linux")
  set(BUILD_COMMAND make ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "darwin")
  set(BUILD_COMMAND make platform=osx ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "ios")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "android")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "rbpi")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "freebsd")
# TODO
endif()

externalproject_add(fmsx
                    GIT_REPOSITORY https://github.com/libretro/fmsx-libretro
                    GIT_TAG 4de0b34
                    PREFIX fmsx
                    CONFIGURE_COMMAND ""
                    INSTALL_COMMAND ""
                    BUILD_COMMAND ${BUILD_COMMAND}
                    BUILD_IN_SOURCE 1)

set(FMSX_CUSTOM_BINARY ${PROJECT_BINARY_DIR}/fmsx/src/fmsx/fmsx_libretro${CMAKE_SHARED_LIBRARY_SUFFIX}
                       ${PROJECT_NAME}${CMAKE_SHARED_LIBRARY_SUFFIX})

build_addon(${PROJECT_NAME} FMSX DEPLIBS)
add_dependencies(${PROJECT_NAME} fmsx)
