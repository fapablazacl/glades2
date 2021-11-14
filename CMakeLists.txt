
cmake_minimum_required(VERSION 3.0)

set (CMAKE_DEBUG_POSTFIX "d")

project (glades2)

# Target Definition
set (target glades2)

set (sources 
	"include/glad/glad.h"
	"include/glad/glad_release.h"
	"include/glad/glad_debug.h"
    "src/glad.c"
	"src/glad_release_impl.h"
	"src/glad_debug_impl.h"
)

add_library (${target} ${sources})

target_include_directories(${target} PUBLIC 
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

# Package Config Version Section
include(CMakePackageConfigHelpers)

write_basic_package_version_file(
    "${PROJECT_BINARY_DIR}/glades2ConfigVersion.cmake"
    VERSION 1.0
    COMPATIBILITY AnyNewerVersion
)

# Install 
install(TARGETS glades2
    EXPORT glades2Targets
    LIBRARY DESTINATION lib COMPONENT Runtime
    ARCHIVE DESTINATION lib COMPONENT Development
    RUNTIME DESTINATION bin COMPONENT Runtime
    PUBLIC_HEADER DESTINATION include COMPONENT Development
    BUNDLE DESTINATION bin COMPONENT Runtime
)

# Package Configuration
include(CMakePackageConfigHelpers)
configure_package_config_file(
    "${PROJECT_SOURCE_DIR}/cmake/glades2Config.cmake.in"
    "${PROJECT_BINARY_DIR}/glades2Config.cmake"
    INSTALL_DESTINATION lib/cmake/glades2
)

install(EXPORT glades2Targets DESTINATION lib/cmake/glades2)
install(FILES "${PROJECT_BINARY_DIR}/glades2ConfigVersion.cmake"
              "${PROJECT_BINARY_DIR}/glades2Config.cmake"
        DESTINATION lib/cmake/glades2)
install(DIRECTORY ${PROJECT_SOURCE_DIR}/include/ DESTINATION include)
