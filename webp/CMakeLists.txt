cmake_minimum_required(VERSION 2.8)

PROJECT(CORE_webp) 

add_definitions(-D_CRT_SECURE_NO_WARNINGS)
add_definitions(-D_LIB)

add_definitions(-D_VISUALC_)
add_definitions(-DNeedFunctionPrototypes)





add_definitions(-DWIN32_LEAN_AND_MEAN)
add_definitions(-DHAVE_WINCODEC_H)
add_definitions(-DWEBP_USE_THREAD)


#包含工程所需要的头文件路径
include_directories(${PROJECT_SOURCE_DIR}/src/dec)
include_directories(${PROJECT_SOURCE_DIR}/src/dsp)
include_directories(${PROJECT_SOURCE_DIR}/src/enc)
include_directories(${PROJECT_SOURCE_DIR}/src/utils)
include_directories(${PROJECT_SOURCE_DIR}/src/webp)



#add_subdirectory(src)

#设置源文件的相对路径和文件名
#PROJECT_SOURCE_DIR变量为最顶层Cmakelists.txt的路径
file(GLOB SRC ${PROJECT_SOURCE_DIR}/src/dec/*.c
${PROJECT_SOURCE_DIR}/src/dsp/*.c
${PROJECT_SOURCE_DIR}/src/enc/*.c
${PROJECT_SOURCE_DIR}/src/utils/*.c


)







#file(GLOB FFI_SRC  blocksort.c bzilb.c compress.c crctable.c decompress.c huffman.c rendtable.c )
#file(GLOB FFI_HDR RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} *.h)

#source_group("Header Files" FILES ${ZLIB_HDR})
ADD_LIBRARY(CORE_webp STATIC ${SRC} )

set_target_properties(CORE_webp PROPERTIES COMPILE_FLAGS "/EHsc /GF")

set_target_properties(CORE_webp PROPERTIES
        OUTPUT_NAME CORE_webp
        DEBUG_POSTFIX "d"
    )
#set_target_properties(ZLib PROPERTIES COMPILE_FLAGS "/GF")  #VERY IMPORTANT: can not use twice.

#set_source_files_properties(${ZLIB_SRC} PROPERTIES COMPILE_FLAGS "/EHsc")   #every file compile option.

set(CompilerFlags
        CMAKE_CXX_FLAGS
        CMAKE_CXX_FLAGS_DEBUG
        CMAKE_CXX_FLAGS_RELEASE
        CMAKE_C_FLAGS
        CMAKE_C_FLAGS_DEBUG
        CMAKE_C_FLAGS_RELEASE
        )
foreach(CompilerFlag ${CompilerFlags})
  string(REPLACE "/MD" "/MT" ${CompilerFlag} "${${CompilerFlag}}")
  string(REPLACE "/Ob0" "" ${CompilerFlag} "${${CompilerFlag}}")
  string(REPLACE "/TC" "" ${CompilerFlag} "${${CompilerFlag}}")
endforeach()

INSTALL(TARGETS CORE_webp
RUNTIME DESTINATION bin
LIBRARY DESTINATION lib
ARCHIVE DESTINATION lib
)


