#+------------------------------------------------------+
#|solution template.                                    |
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

#===============SolutionInformation===============
project (
	"[SOLUTION_NAME]"
	VERSION 0.0.0.0
	DESCRIPTION ""
)

#===============CMAKE message tag===============
set(MB_MSG_TAG "[TAG]")

#===============Build target environment===============
#* ANDROID
#* LINUX
#* WINDOWS
set(MB_CURRENT_ENVIRONMENT [SELECT])

#===============Add Preproccessor===============
set(MB_GLOBAL_PREPROC
	MBPREPROC__${MB_CURRENT_ENVIRONMENT}# <- Do not delete the current line
)

#===============Common code projects===============
set(MB_COMMON_PROJECTS
)

#===============Native code project to be mapped===============
set(MB_NATIVE_PROJECTS
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
message(${MB_MSG_TAG} <==SOLUTION_BEGIN==>)

foreach(mb_project ${MB_NATIVE_PROJECTS})
message(${MB_MSG_TAG} Native Projects -> ${mb_project})
add_subdirectory(__NATIVE__/__${MB_CURRENT_ENVIRONMENT}__/${mb_project})
endforeach()

foreach(mb_project ${MB_COMMON_PROJECTS})
message(${MB_MSG_TAG} Common Projects -> ${mb_project})
add_subdirectory(__COMMON__/${mb_project})
endforeach()

message(${MB_MSG_TAG} <==SOLUTION_END==>)