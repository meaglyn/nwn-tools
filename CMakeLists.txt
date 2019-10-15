cmake_minimum_required(VERSION 2.8)
project(nwn-tools)

#find_program(bison bison)
#if (NOT bison)
#	message(FATAL_ERROR "Cannot find bison, need bison installed to generate Parser/Lexer")
#endif (NOT bison)
message("-- CMAKE_SYSTEM_INFO_FILE: ${CMAKE_SYSTEM_INFO_FILE}")
message("-- CMAKE_SYSTEM_NAME:      ${CMAKE_SYSTEM_NAME}")
message("-- CMAKE_SYSTEM_PROCESSOR: ${CMAKE_SYSTEM_PROCESSOR}")
message("-- CMAKE_SYSTEM:           ${CMAKE_SYSTEM}")

find_program(LSB_RELEASE_EXEC lsb_release)
execute_process(COMMAND ${LSB_RELEASE_EXEC} -is
    OUTPUT_VARIABLE LSB_RELEASE_ID_SHORT
    OUTPUT_STRIP_TRAILING_WHITESPACE
)

message("-- LSB_RELEASE_ID_SHORT : ${LSB_RELEASE_ID_SHORT}")
string (REGEX MATCH "RedHat" is_redhat ${LSB_RELEASE_ID_SHORT})
message("-- is_redhat : ${is_redhat}")

find_package(BISON)
if (NOT BISON_FOUND)
	message(FATAL_ERROR "Cannot find bison, need bison installed to generate Parser/Lexer")
endif (NOT BISON_FOUND)

#set(CMAKE_CXX_FLAGS "-Wall -Wno-unused -Wno-write-strings -Wno-deprecated")
set(CMAKE_CXX_FLAGS " -w -O2 -m32")
#set(CMAKE_CXX_FLAGS "-g -w -m32")

# this does not seem needed for my opensuse bison 3.0.4. 
# but is needed for 3.0.5 and for 3.0.4 on RHEL
if (APPLE OR (BISON_VERSION VERSION_GREATER "3.0.3"))
   add_definitions(-DNWN_BISON_3=1)
endif (APPLE OR (BISON_VERSION VERSION_GREATER "3.0.3"))

add_subdirectory(_MathLib)
add_subdirectory(_NmcLib)
add_subdirectory(_NwnLib)
add_subdirectory(_NscLib)
add_subdirectory(nwnnsscomp)
add_subdirectory(nwnmdlcomp)
