cmake_minimum_required(VERSION 3.2)

include(cmake/HunterGate.cmake)
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.23.309.tar.gz"
    SHA1 "15e84e1f23eb079360db727d373dab82890420b7"
)

project(SDL_net VERSION 2.0.1)

add_library(${PROJECT_NAME})

hunter_add_package(SDL2)
find_package(SDL2 CONFIG REQUIRED)
target_link_libraries(${PROJECT_NAME} PRIVATE SDL2::SDL2)

set(SRC_FILES
    SDLnet.c
    SDLnetTCP.c
    SDLnetUDP.c
    SDLnetselect.c
)

target_sources(${PROJECT_NAME} PRIVATE ${SRC_FILES})


include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

set(config_install_dir "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}")
set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")
set(version_config "${generated_dir}/${PROJECT_NAME}ConfigVersion.cmake")
set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")
set(targets_export_name "${PROJECT_NAME}Targets")
set(namespace "${PROJECT_NAME}::")

install(TARGETS ${PROJECT_NAME}
	EXPORT "${targets_export_name}"
	RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	FRAMEWORK DESTINATION ${CMAKE_INSTALL_LIBDIR}
	INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

install(
    FILES SDL_net.h
    DESTINATION ${CMAKE_INSTALL_FULL_INCLUDEDIR}/SDL2
)

write_basic_package_version_file(
	"${version_config}" COMPATIBILITY SameMajorVersion
)

configure_package_config_file(
	"cmake/template/Config.cmake.in"
	"${project_config}"
	INSTALL_DESTINATION "${config_install_dir}"
)

install(
	FILES "${project_config}" "${version_config}"
	DESTINATION "${config_install_dir}"
)

install(
	EXPORT "${targets_export_name}"
	NAMESPACE "${namespace}"
	DESTINATION "${config_install_dir}"
)
