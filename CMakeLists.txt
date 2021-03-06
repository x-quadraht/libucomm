
cmake_minimum_required(VERSION 2.8)

project(libucomm)

set(LIBUCOMM_VERSION 0.1)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/cmake/libucommConfigVersion.cmake"
    VERSION ${LIBUCOMM_VERSION}
    COMPATIBILITY AnyNewerVersion
)

configure_file(
    cmake/libucommConfig.cmake.in
    "${CMAKE_CURRENT_BINARY_DIR}/cmake/libucommConfig.cmake"
    COPYONLY
)

set(CMAKE_CXX_STANDARD 17)

set(CONF_INCLUDE_DIR "${CMAKE_INSTALL_PREFIX}/include")
set(CONF_PARSE_PY "${CMAKE_INSTALL_PREFIX}/bin/libucomm_parse.py")

configure_file(cmake/libucommConfig.cmake.in "${PROJECT_BINARY_DIR}/cmake/libucommConfig.cmake" @ONLY)
configure_file(cmake/libucommConfigVersion.cmake.in "${PROJECT_BINARY_DIR}/cmake/libucommConfigVersion.cmake" @ONLY)
configure_file(cmake/libucommBuildTreeSettings.cmake.in "${PROJECT_BINARY_DIR}/cmake/libucommBuildTreeSettings.cmake" @ONLY)

set(LIBUCOMM_PARSE_PY ${CMAKE_HOME_DIRECTORY}/libucomm_parse.py)
set(LIBUCOMM_INCLUDE_DIR ${CMAKE_HOME_DIRECTORY}/include)
include(cmake/libucomm.cmake)

include_directories(include)

set(BUILD_EXAMPLES ON CACHE BOOL "Enable examples")
if(BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()

set(ENABLE_TESTS ON CACHE BOOL "Enable tests")
if(ENABLE_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif()

install(DIRECTORY include/ DESTINATION include)
install(PROGRAMS libucomm_parse.py DESTINATION bin)
install(FILES
    ${PROJECT_BINARY_DIR}/cmake/libucommConfig.cmake
    ${PROJECT_BINARY_DIR}/cmake/libucommConfigVersion.cmake
    cmake/libucomm.cmake
    DESTINATION share/cmake/libucomm)
