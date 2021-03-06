cmake_minimum_required(VERSION 3.10)

set(CMAKE_INSTALL_PREFIX /usr)

project(logiops)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -O0 -Wall -Wextra")

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Git)

# Set version number
if(GIT_FOUND AND EXISTS ${CMAKE_SOURCE_DIR}/.git)
    execute_process(COMMAND ${GIT_EXECUTABLE} describe
         OUTPUT_VARIABLE LOGIOPS_VERSION
         RESULT_VARIABLE LOGIOPS_VERSION_RET
         ERROR_QUIET)
    if(NOT LOGIOPS_VERSION_RET EQUAL "0")
        execute_process(COMMAND ${GIT_EXECUTABLE}
            rev-parse --short HEAD
            OUTPUT_VARIABLE LOGIOPS_COMMIT_HASH)
        set(LOGIOPS_VERSION git-${LOGIOPS_COMMIT_HASH})
    endif()
    string(REGEX REPLACE "\n$" "" LOGIOPS_VERSION ${LOGIOPS_VERSION})
elseif(EXISTS ${CMAKE_SOURCE_DIR}/version.txt)
    file(READ version.txt LOGIOPS_VERSION)
    string(REGEX REPLACE "\n$" "" LOGIOPS_VERSION ${LOGIOPS_VERSION})
endif()

if(NOT LOGIOPS_VERSION)
    set(LOGIOPS_VERSION "null")
endif()
message("LogiOps Version Number: ${LOGIOPS_VERSION}")

add_definitions( -DLOGIOPS_VERSION="${LOGIOPS_VERSION}")

add_subdirectory(src/logid)
