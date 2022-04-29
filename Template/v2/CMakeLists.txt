#+--------------------------------------------------------------------------------------------------------------------------+
#|big box                                                                                                                   |
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
#+------------------------------------------------------+
#|small box                                             |
#|                                                      |
#|                                                      |
#+------------------------------------------------------+


#+--------------------------------------------------------------------------------------------------------------------------+
#|solution template.                                                                                                        |
#|TEMPLATE_VERSION:2022.03.26                                                                                               |
#|                                                                                                                          |
#| * CMAKE for cross compiling                                                                                              |
#| * Please remove the "[NULL]" part and enter                                                                              |
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
#|DESCRIPTION : Solution name                           |
#|                                                      |
#+------------------------------------------------------+
set(MB_SOLUTION_NAME "[NULL]")

#+------------------------------------------------------+
#|TYPE : string                                         |
#|DESCRIPTION : cmake msg tag                           |
#|                                                      |
#+------------------------------------------------------+
set(MB_MSG_TAG "[NULL]")

#+------------------------------------------------------+
#|TYPE : array                                          |
#|DESCRIPTION : environment list                        |
#|                                                      |
#+------------------------------------------------------+
set(MB_ENVIRONMENTS
	"ANDROID"
	"LINUX"
	"WINDOWS"
)

#+------------------------------------------------------+
#|TYPE : string                                         |
#|DESCRIPTION : Choose from the list of environments    |
#|above                                                 |
#+------------------------------------------------------+
set(MB_CURRENT_ENVIRONMENT "[NULL]")

#+------------------------------------------------------+
#|TYPE : array                                          |
#|DESCRIPTION : add macro                               |
#|                                                      |
#|MBPREPROC__${MB_CURRENT_ENVIRONMENT} : do not erase   |
#|It will tell you the current environment.             |
#|"MBPREPROC__<Environment>" <- Macro                   |
#+------------------------------------------------------+
set(MB_GLOBAL_PREPROC
	MBPREPROC__${MB_CURRENT_ENVIRONMENT}
)

#+------------------------------------------------------+
#|TYPE : array                                          |
#|DESCRIPTION : A project that works in any environment |
#|                                                      |
#+------------------------------------------------------+
set(MB_COMMON_PROJECTS
)

#+------------------------------------------------------+
#|TYPE : array                                          |
#|DESCRIPTION : Projects to be treated as native        |
#|                                                      |
#+------------------------------------------------------+
set(MB_NATIVE_PROJECTS
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

project (
	${MB_SOLUTION_NAME}
	VERSION 0.0.0.0
	DESCRIPTION ""
)

#null check
if(${MB_SOLUTION_NAME} STREQUAL "[NULL]")
massage(fatal_error "is NULL")
endif()

if(${MB_MSG_TAG} STREQUAL "[NULL]")
massage(fatal_error "is NULL")
endif()

if(${MB_CURRENT_ENVIRONMENT} STREQUAL "[NULL]")
massage(fatal_error "is NULL")
endif()

if(EXISTS ${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME})
else()

#create solution dir
file(COPY __TEMPLATE__/[SOLUTION_NAME] DESTINATION ${CMAKE_SOURCE_DIR})
file(RENAME ${CMAKE_SOURCE_DIR}/[SOLUTION_NAME] ${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME})

#create native environment dir
foreach(mb_project ${MB_ENVIRONMENTS})
file(COPY __TEMPLATE__/[NATIVE_DIR_NAME] DESTINATION ${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__NATIVE__/)
file(RENAME ${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__NATIVE__/[NATIVE_DIR_NAME] ${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__NATIVE__/__${mb_project}__)
endforeach()

#create project ndirs
foreach(mb_en ${MB_ENVIRONMENTS})
foreach(mb_project ${MB_NATIVE_PROJECTS})
file(COPY __TEMPLATE__/[PROJECT_NAME] DESTINATION ${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__NATIVE__/__${mb_en}__)
file(RENAME ${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__NATIVE__/__${mb_en}__/[PROJECT_NAME] ${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__NATIVE__/__${mb_en}__/${mb_project})
endforeach()
endforeach() 

#create project cdirs
foreach(mb_project ${MB_COMMON_PROJECTS})
file(COPY __TEMPLATE__/[PROJECT_NAME] DESTINATION ${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__COMMON__)
file(RENAME ${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__COMMON__/[PROJECT_NAME] ${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__COMMON__/${mb_project})
endforeach()

endif()



message(${MB_MSG_TAG} <==SOLUTION_BEGIN==>)

foreach(mb_project ${MB_NATIVE_PROJECTS})
message(${MB_MSG_TAG} Native Projects -> ${mb_project})
add_subdirectory(${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__NATIVE__/__${MB_CURRENT_ENVIRONMENT}__/${mb_project})
endforeach()

foreach(mb_project ${MB_COMMON_PROJECTS})
message(${MB_MSG_TAG} Common Projects -> ${mb_project})
add_subdirectory(${CMAKE_SOURCE_DIR}/${MB_SOLUTION_NAME}/__COMMON__/${mb_project})
endforeach()

message(${MB_MSG_TAG} <==SOLUTION_END==>)