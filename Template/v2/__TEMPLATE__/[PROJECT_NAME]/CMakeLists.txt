#+--------------------------------------------------------------------------------------------------------------------------+
#|project template.                                                                                                         |
#|TEMPLATE_VERSION:2022.03.26                                                                                               |
#|                                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#+--------------------------------------------------------------------------------------------------------------------------+
cmake_minimum_required (VERSION 3.8)

#+------------------------------------------------------+
#|TYPE : string                                         |
#|DESCRIPTION : project name                            |
#|Must have the same name as the current directory      |
#+------------------------------------------------------+
set(MB_PROJECT_NAME "[NULL]")

#+------------------------------------------------------+
#|TYPE : string                                         |
#|DESCRIPTION : select language                         |
#|Choose CPP or C                                       |
#+------------------------------------------------------+
set(MB_LANGUAGE "[NULL]")
set(MB_LANGUAGE_VERSION "[NULL]")

#+------------------------------------------------------+
#|TYPE : string                                         |
#|DESCRIPTION : build type                              |
#|Choose LIB or EXE                                     |
#+------------------------------------------------------+
set(MB_BUILD_TYPE "[NULL]")

#+------------------------------------------------------+
#|TYPE : string                                         |
#|DESCRIPTION : In case of library build, select the    |
#|type                                                  |
#|Choose STATIC or SHARED or MODULE                     |
#+------------------------------------------------------+
set(MB_IF_LIB_BUILD_OPTION "[NULL]")

#+------------------------------------------------------+
#|TYPE : array                                          |
#|DESCRIPTION : compile options                         |
#|                                                      |
#+------------------------------------------------------+
set(MB_OPTIONS2
)

#+------------------------------------------------------+
#|TYPE :                                                |
#|DESCRIPTION :                                         |
#|                                                      |
#|${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__INCLUDE__ : |
#| * do not erase                                       |
#| * Points to the header directory to be mapped        |
#+------------------------------------------------------+
set(MB_INCLUDE_DIRS
    ${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__INCLUDE__
    #This is the default path that references the native library. Available in native projects
    #${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__NATIVE__/__${MB_CURRENT_ENVIRONMENT}__/${MB_PROJECT_NAME}/__LIBS__/include
)

#+------------------------------------------------------+
#|TYPE : array                                          |
#|DESCRIPTION : library directory path                  |
#|                                                      |
#|if __COMMON__                                         |
#| It's a public project, so it shouldn't point         |
#| to a native library                                  |
#|                                                      |
#|if __NATIVE__                                         |
#|                                                      |
#+------------------------------------------------------+
set(MB_LINK_LIB_DIRS
    #This is the default path that references the native library. Available in native projects
    #${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__NATIVE__/__${MB_CURRENT_ENVIRONMENT}__/${MB_PROJECT_NAME}/__LIBS__
)

#+------------------------------------------------------+
#|TYPE : array                                          |
#|DESCRIPTION : library names                           |
#| * If you would like to add your library here!        |
#+------------------------------------------------------+
set(MB_LINK_LIBS
)

#+------------------------------------------------------+
#|TYPE : array                                          |
#|DESCRIPTION : your source files                       |
#|                                                      |
#+------------------------------------------------------+
set(MB_SOURCE_FILES
)

#+--------------------------------------------------------------------------------------------------------------------------+
#|processing part.                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#|                                                                                                                          |
#+--------------------------------------------------------------------------------------------------------------------------+

#null check
if(${MB_PROJECT_NAME} STREQUAL "[NULL]")
massage(fatal_error "is NULL")
endif()

if(${MB_LANGUAGE} STREQUAL "[NULL]")
massage(fatal_error "is NULL")
endif()

if(${MB_LANGUAGE_VERSION} STREQUAL "[NULL]")
massage(fatal_error "is NULL")
endif()

if(${MB_BUILD_TYPE} STREQUAL "[NULL]")
massage(fatal_error "is NULL")
endif()

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


set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/__BUILD__/${MB_CURRENT_ENVIRONMENT}/${CMAKE_BUILD_TYPE})
set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/__BUILD__/${MB_CURRENT_ENVIRONMENT}/${CMAKE_BUILD_TYPE})
set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/__BUILD__/${MB_CURRENT_ENVIRONMENT}/${CMAKE_BUILD_TYPE})


if(${MB_BUILD_TYPE} STREQUAL "LIB")
add_library(${MB_PROJECT_NAME} ${MB_IF_LIB_BUILD_OPTION} ${MB_SOURCE_FILES})
message(${MB_MSG_TAG} <${MB_IF_LIB_BUILD_OPTION}>${MB_BUILD_TYPE} -> ${MB_PROJECT_NAME})

elseif(${MB_BUILD_TYPE} STREQUAL "EXE")
add_executable(${MB_PROJECT_NAME} ${MB_SOURCE_FILES})
message(${MB_MSG_TAG} ${MB_BUILD_TYPE} -> ${MB_PROJECT_NAME})

else()
massage(fatal_error "[MB]unsupported build type")
endif()