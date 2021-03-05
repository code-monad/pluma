project(PlumaExamples)

set(PLUMA_EXAMPLE_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)
set(PLUMA_EXAMPLE_INTERFACE_SOURCE ${PLUMA_EXAMPLE_SOURCE_DIR}/interface/Warrior.cpp)
set(PLUMA_EXAMPLE_SRC
  ${PLUMA_EXAMPLE_SOURCE_DIR}/host/Main.cpp
  ${PLUMA_EXAMPLE_INTERFACE_SOURCE}
  
  )
add_executable(ex_warrior ${PLUMA_EXAMPLE_SRC})

add_library(named_warriors SHARED ${PLUMA_EXAMPLE_SOURCE_DIR}/plugin/Connector.cpp ${PLUMA_EXAMPLE_INTERFACE_SOURCE})
target_include_directories(named_warriors PUBLIC ${PLUMA_EXAMPLE_SOURCE_DIR}/interface)
target_link_libraries(named_warriors PUBLIC pluma)
set_target_properties( named_warriors PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/plugins )


target_include_directories(ex_warrior PUBLIC
  ${PLUMA_EXAMPLE_SOURCE_DIR}/interface)
target_link_libraries(ex_warrior PUBLIC pluma)
