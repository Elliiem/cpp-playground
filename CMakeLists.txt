cmake_minimum_required(VERSION 3.20)

project(Playground)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

function(GetDirs ret path)
    file(GLOB children "${path}" LIST_DIRECTORIES TRUE)

    set(directories)

    foreach(child ${children})
        if(IS_DIRECTORY "${child}")
            list(APPEND directories "${child}")
        endif()
    endforeach()

    set("${ret}" "${directories}" PARENT_SCOPE)
    
endfunction()

function(SplitDirs ret_subdir ret_dir in)
    set(subdirs)
    set(dirs)

    foreach(dir ${in})
        file(GLOB contents "${dir}/*")

        set(is_subdir FALSE)

        foreach(file ${contents})
            if("${file}" STREQUAL "${dir}/CMakeLists.txt")
                list(APPEND subdirs "${dir}")
                set(is_subdir TRUE)
                break()
            endif()
        endforeach()

        if(NOT "${is_subdir}")
            list(APPEND dirs "${dir}")
        endif()
    endforeach()

    set("${ret_subdir}" "${subdirs}" PARENT_SCOPE)
    set("${ret_dir}" "${dirs}" PARENT_SCOPE)
endfunction()

GetDirs(DIRS "${CMAKE_SOURCE_DIR}/Programs/*")
SplitDirs(SUBDIRECTORIES DIRECTORIES "${DIRS}")

foreach(SUBDIR ${SUBDIRECTORIES})
    add_subdirectory("${SUBDIR}")    
endforeach()

foreach(DIR ${DIRECTORIES})
    cmake_path(GET DIR FILENAME NAME)

    file(GLOB_RECURSE SOURCES 
        "${DIR}/*.cpp"
        "${DIR}/*.c"
        "${DIR}/*.asm"
        "${DIR}/*.S"
    )

    file(GLOB_RECURSE HEADERS 
        "${DIR}/*.h"
    )

    add_executable("${NAME}"
        "${SOURCES}"
        "${HEADERS}"
    )

    set_target_properties("${NAME}" PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/Programs/${NAME}" OUTPUT_NAME "out")
endforeach()


