#+------------------------------------------------------+
#|project template.                                     |
#|TEMPLATE_VERSION:2022.03.26                           |
#|                                                      |
#|                                                      |
#|                                                      |
#|                                                      |
#|                                                      |
#|                                                      |
#|                                                      |
#|                                                      |
#|                                                      |
#+------------------------------------------------------+
cmake_minimum_required (VERSION 3.8)

#<PROJECT_NAME> <OUPUT_NAME>
set(MB_PROJECT_NAME [PROJECT_NAME])

#===============Language===============
#* "CPP"
#* "C"
set(MB_LANGUAGE [SELECT])
set(MB_LANGUAGE_VERSION [LANGUAGE_VERSION])

#===============Build===============
#* "LIB"
#* "EXE"
set(MB_BUILD_TYPE [SELECT])
#* STATIC
#* SHARED
#* MODULE
set(MB_IF_LIB_BUILD_OPTION [SELECT])

#===============CompileOptions===============

set(MB_OPTIONS2
)

#===============IncludeHeaderDir_PathList===============
set(MB_INCLUDE_DIRS
    ${CMAKE_SOURCE_DIR}/__NATIVE__/__INCLUDE__# <- Do not delete the current line
)

#===============LibDir_PathList===============
set(MB_LINK_LIB_DIRS
)

#===============LibFile_NameList===============
set(MB_LINK_LIBS
)

#===============SourceFileList===============
set(MB_SOURCE_FILES
)

#+------------------------------------------------------+
#|processing part.                                      |
#|                                                      |
#|                                                      |
#|                                                      |
#|                                                      |
#|                                                      |
#|                                                      |
#|                                                      |
#|                                                      |
#|                                                      |
#|                                                      |
#+------------------------------------------------------+
message(${MB_MSG_TAG} Language is ${MB_LANGUAGE}${MB_LANGUAGE_VERSION})
if(${MB_LANGUAGE} STREQUAL "CPP")
set(CMAKE_CXX_STANDARD ${MB_LANGUAGE_VERSION})
elseif(${MB_LANGUAGE} STREQUAL "C")
set(CMAKE_C_STANDARD ${MB_LANGUAGE_VERSION})
else()
massage(fatal_error "[MB]unsupported language")
endif()

foreach(mb_items ${MB_INCLUDE_DIRS})
message(${MB_MSG_TAG} include_dir -> ${mb_items})
endforeach()
include_directories (${MB_INCLUDE_DIRS})

foreach(mb_items ${MB_GLOBAL_PREPROC})
message(${MB_MSG_TAG} preproc -> ${mb_items})
endforeach()
add_compile_definitions(${MB_PROJECT_NAME} PRIVATE ${MB_GLOBAL_PREPROC})

add_compile_options(${MB_OPTIONS2})

foreach(mb_items ${MB_DEF_LINK_OPTIONS})
message(${MB_MSG_TAG} link_options -> ${mb_items})
endforeach()
add_link_options (${MB_DEF_LINK_OPTIONS})

foreach(mb_items ${MB_LINK_LIBS})
message(${MB_MSG_TAG} lib -> ${mb_items})
endforeach()
link_libraries (${MB_LINK_LIBS})

foreach(mb_items ${MB_LINK_LIB_DIRS})
message(${MB_MSG_TAG} lib_dir -> ${mb_items})
endforeach()
link_directories (${MB_LINK_LIB_DIRS})


set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${MB_CURRENT_ENVIRONMENT}/${CMAKE_BUILD_TYPE})
set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${MB_CURRENT_ENVIRONMENT}/${CMAKE_BUILD_TYPE}/lib)
set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${MB_CURRENT_ENVIRONMENT}/${CMAKE_BUILD_TYPE}/lib)


if(${MB_BUILD_TYPE} STREQUAL "LIB")
add_library(${MB_PROJECT_NAME} ${MB_IF_LIB_BUILD_OPTION} ${MB_SOURCE_FILES})
message(${MB_MSG_TAG} <${MB_IF_LIB_BUILD_OPTION}>${MB_BUILD_TYPE} -> ${MB_PROJECT_NAME})

elseif(${MB_BUILD_TYPE} STREQUAL "EXE")
add_executable(${MB_PROJECT_NAME} ${MB_SOURCE_FILES})
message(${MB_MSG_TAG} ${MB_BUILD_TYPE} -> ${MB_PROJECT_NAME})

else()
massage(fatal_error "[MB]unsupported build type")
endif()