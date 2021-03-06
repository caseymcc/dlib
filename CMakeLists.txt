# Copyright (C) 2015 Ruslan Baratov
# License: Boost Software License
# See LICENSE.txt for the full license.

cmake_minimum_required(VERSION 3.0)

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.16.15.tar.gz"
    SHA1 "6974c2150fc0d3b09de3ad1efcbf15d360647ffa"
)

project(dlib VERSION 19.0)

if(DLIB_HEADER_ONLY)
  add_library(dlib INTERFACE)

  ### Install ###
  set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")

  set(config_install_dir "lib/cmake/${PROJECT_NAME}")
  set(include_install_dir "include")

  set(version_config "${generated_dir}/${PROJECT_NAME}ConfigVersion.cmake")
  set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")
  set(targets_export_name "${PROJECT_NAME}Targets")
  set(namespace "${PROJECT_NAME}::")

  include(CMakePackageConfigHelpers)
  write_basic_package_version_file(
      "${version_config}" COMPATIBILITY SameMajorVersion
  )

  # Note: use 'targets_export_name'
  configure_file("cmake/Config.cmake.in" "${project_config}" @ONLY)

  install(
      TARGETS dlib
      EXPORT "${targets_export_name}"
      INCLUDES DESTINATION "${include_install_dir}"
  )

  install(
      DIRECTORY "dlib"
      DESTINATION "${include_install_dir}"
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
  
else()
  add_subdirectory(dlib)
endif()

