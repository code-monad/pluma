project(Pluma)
cmake_minimum_required (VERSION 3.0)
option(BUILD_DOC "Build Documentation" OFF)
option(BUILD_EXAMPLES "Build Examples" OFF)

set(PLUMA_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src/Pluma)
set(PLUMA_SRC
  ${PLUMA_SOURCE_DIR}/Dir.cpp
  ${PLUMA_SOURCE_DIR}/DLibrary.cpp
  ${PLUMA_SOURCE_DIR}/Host.cpp
  ${PLUMA_SOURCE_DIR}/Provider.cpp
  ${PLUMA_SOURCE_DIR}/PluginManager.cpp
  )

add_library(pluma ${PLUMA_SRC})
target_include_directories(pluma PUBLIC
  ${CMAKE_CURRENT_SOURCE_DIR}/include/
  ${CMAKE_CURRENT_SOURCE_DIR}/src/
  )


install (TARGETS pluma
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  )

INSTALL (
  DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/
  DESTINATION include
  FILES_MATCHING PATTERN "*.h*")

if(BUILD_DOC)
  # check if Doxygen is installed
  find_package(Doxygen)
  if (DOXYGEN_FOUND)
    # set input and output files
    set(DOXYGEN_FILES_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/doc)
    
    file(COPY ${DOXYGEN_FILES_SOURCE_DIR} DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
    set(DOXYGEN_FILES_WORKING_DIR ${CMAKE_CURRENT_BINARY_DIR}/doc/build)
    set(${DOXYGEN_FILE} ${DOXYGEN_FILES_WORKING_DIR}/Doxyfile)
    message("Doxygen build started")

    # note the option ALL which allows to build the docs together with the application
    add_custom_target( doc_doxygen ALL
      COMMAND ${DOXYGEN_EXECUTABLE} ${DOXYGEN_FILE}
      WORKING_DIRECTORY ${DOXYGEN_FILES_WORKING_DIR}
      COMMENT "Generating API documentation with Doxygen"
      VERBATIM )
  else (DOXYGEN_FOUND)
    message("Doxygen need to be installed to generate the doxygen documentation")
  endif (DOXYGEN_FOUND)
endif()

if(BUILD_EXAMPLES)
  include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include/)
  add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/example)
endif()
