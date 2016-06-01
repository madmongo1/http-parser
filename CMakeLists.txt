project (http_parser C)

set(HDRS http_parser.h)
set(SRCS http_parser.c)
add_library (http_parser ${SRCS} ${HDRS})
set (binstall_h_path "${CMAKE_CURRENT_BINARY_DIR}/contrib/http_parser")
add_custom_command (TARGET http_parser
POST_BUILD
COMMAND ${CMAKE_COMMAND} -E make_directory
${binstall_h_path})
foreach (_h ${HDRS})
add_custom_command (TARGET http_parser
POST_BUILD
COMMAND ${CMAKE_COMMAND} -E copy
"${CMAKE_CURRENT_SOURCE_DIR}/${_h}"
${binstall_h_path})
endforeach ()
target_include_directories(http_parser
SYSTEM PUBLIC
$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/include>
$<INSTALL_INTERFACE:include>)



add_executable(http_parser_test test.c)
target_link_libraries(http_parser_test http_parser)

add_test(HttpParserTests http_parser_test)

