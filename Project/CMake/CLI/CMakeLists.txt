cmake_minimum_required(VERSION 3.16)

project(bwfmetaedit LANGUAGES CXX C)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
endif()

option(ENABLE_C2PA "Enable C2PA support" OFF)
option(C2PA_DYNAMIC "Load the C2PA library dynamically at runtime" OFF)

get_filename_component(SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../../../Source" ABSOLUTE)

set(HEADERS
    ${SOURCE_DIR}/CLI/CLI_Help.h
    ${SOURCE_DIR}/CLI/CommandLine_Parser.h
    ${SOURCE_DIR}/Common/Common_About.h
    ${SOURCE_DIR}/Common/Codes.h
    ${SOURCE_DIR}/Common/Core.h
    ${SOURCE_DIR}/MD5/md5.h
    ${SOURCE_DIR}/Riff/Riff_Base.h
    ${SOURCE_DIR}/Riff/Riff_Chunks.h
    ${SOURCE_DIR}/Riff/Riff_Handler.h
    ${SOURCE_DIR}/TinyXml2/tinyxml2.h
    ${SOURCE_DIR}/ZenLib/BitStream.h
    ${SOURCE_DIR}/ZenLib/BitStream_Fast.h
    ${SOURCE_DIR}/ZenLib/BitStream_LE.h
    ${SOURCE_DIR}/ZenLib/Conf.h
    ${SOURCE_DIR}/ZenLib/Conf_Internal.h
    ${SOURCE_DIR}/ZenLib/CriticalSection.h
    ${SOURCE_DIR}/ZenLib/Dir.h
    ${SOURCE_DIR}/ZenLib/File.h
    ${SOURCE_DIR}/ZenLib/FileName.h
    ${SOURCE_DIR}/ZenLib/InfoMap.h
    ${SOURCE_DIR}/ZenLib/int128s.h
    ${SOURCE_DIR}/ZenLib/int128u.h
    ${SOURCE_DIR}/ZenLib/MemoryDebug.h
    ${SOURCE_DIR}/ZenLib/MemoryUtils.h
    ${SOURCE_DIR}/ZenLib/OS_Utils.h
    ${SOURCE_DIR}/ZenLib/Thread.h
    ${SOURCE_DIR}/ZenLib/Trace.h
    ${SOURCE_DIR}/ZenLib/Translation.h
    ${SOURCE_DIR}/ZenLib/Utils.h
    ${SOURCE_DIR}/ZenLib/Ztring.h
    ${SOURCE_DIR}/ZenLib/ZtringList.h
    ${SOURCE_DIR}/ZenLib/ZtringListList.h
    ${SOURCE_DIR}/ZenLib/ZtringListListF.h
)

set(SOURCES
    ${SOURCE_DIR}/CLI/CLI_Help.cpp
    ${SOURCE_DIR}/CLI/CLI_Main.cpp
    ${SOURCE_DIR}/CLI/CommandLine_Parser.cpp
    ${SOURCE_DIR}/Common/Common_About.cpp
    ${SOURCE_DIR}/Common/Codes.cpp
    ${SOURCE_DIR}/Common/Core.cpp
    ${SOURCE_DIR}/MD5/md5.c
    ${SOURCE_DIR}/Riff/Riff_Base.cpp
    ${SOURCE_DIR}/Riff/Riff_Base_Streams.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE__PMX.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_aXML.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_bext.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_data.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_ds64.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_FLLR.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_fmt_.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_INFO.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_INFO_xxxx.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_iXML.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_MD5_.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_cue_.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_adtl.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_adtl_labl.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_adtl_note.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_adtl_ltxt.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_CSET.cpp
    ${SOURCE_DIR}/Riff/Riff_Chunks_WAVE_C2PA.cpp
    ${SOURCE_DIR}/Riff/Riff_Handler.cpp
    ${SOURCE_DIR}/TinyXml2/tinyxml2.cpp
    ${SOURCE_DIR}/ZenLib/Conf.cpp
    ${SOURCE_DIR}/ZenLib/CriticalSection.cpp
    ${SOURCE_DIR}/ZenLib/Dir.cpp
    ${SOURCE_DIR}/ZenLib/File.cpp
    ${SOURCE_DIR}/ZenLib/FileName.cpp
    ${SOURCE_DIR}/ZenLib/InfoMap.cpp
    ${SOURCE_DIR}/ZenLib/int128s.cpp
    ${SOURCE_DIR}/ZenLib/int128u.cpp
    ${SOURCE_DIR}/ZenLib/MemoryDebug.cpp
    ${SOURCE_DIR}/ZenLib/OS_Utils.cpp
    ${SOURCE_DIR}/ZenLib/Translation.cpp
    ${SOURCE_DIR}/ZenLib/Thread.cpp
    ${SOURCE_DIR}/ZenLib/Utils.cpp
    ${SOURCE_DIR}/ZenLib/Ztring.cpp
    ${SOURCE_DIR}/ZenLib/ZtringList.cpp
    ${SOURCE_DIR}/ZenLib/ZtringListList.cpp
    ${SOURCE_DIR}/ZenLib/ZtringListListF.cpp
)

if(ENABLE_C2PA)
    list(APPEND SOURCES ${SOURCE_DIR}/Riff/Riff_C2PA_Helpers.cpp)
    list(APPEND HEADERS
        ${SOURCE_DIR}/ThirdParty/json/json.h
        ${SOURCE_DIR}/Riff/Riff_C2PA_Helpers.h
        ${SOURCE_DIR}/Riff/Riff_C2PA_Helpers_CA.h
    )
endif()

if(WIN32)
    if(CMAKE_SIZEOF_VOID_P EQUAL 4)
        set(C2PA_TARGET_DIR ${SOURCE_DIR}/ThirdParty/c2pa-rs/target/i686-pc-windows-msvc/release)
    else()
        set(C2PA_TARGET_DIR ${SOURCE_DIR}/ThirdParty/c2pa-rs/target/x86_64-pc-windows-msvc/release)
    endif()
else()
    set(C2PA_TARGET_DIR ${SOURCE_DIR}/ThirdParty/c2pa-rs/target/release)
endif()

set(TARGET_NAME bwfmetaedit)

if(WIN32)
    add_executable(${TARGET_NAME} ${SOURCES} ${HEADERS}
        ${SOURCE_DIR}/../Project/CMake/CLI/bwfmetaedit.rc)
else()
    add_executable(${TARGET_NAME} ${SOURCES} ${HEADERS})
endif()

target_include_directories(${TARGET_NAME} PRIVATE ${SOURCE_DIR})

target_compile_definitions(${TARGET_NAME} PRIVATE
    TIXML_USE_STL
    UNICODE
    _UNICODE
    _LARGE_FILES
    _FILE_OFFSET_BITS=64
)

if(NOT WIN32)
    set(THREADS_PREFER_PTHREAD_FLAG ON)
    find_package(Threads REQUIRED)
    target_link_libraries(${TARGET_NAME} PRIVATE Threads::Threads)
endif()

if(ENABLE_C2PA)
    target_compile_definitions(${TARGET_NAME} PRIVATE ENABLE_C2PA)
    target_include_directories(${TARGET_NAME} PRIVATE ${C2PA_TARGET_DIR})

    if(C2PA_DYNAMIC)
        target_compile_definitions(${TARGET_NAME} PRIVATE C2PA_DYNAMIC_LOADING)
        if(NOT WIN32)
            target_link_libraries(${TARGET_NAME} PRIVATE ${CMAKE_DL_LIBS})
        endif()
    elseif(WIN32)
        target_link_libraries(${TARGET_NAME} PRIVATE ${C2PA_TARGET_DIR}/c2pa_c.dll.lib)
    else()
        target_link_directories(${TARGET_NAME} PRIVATE ${C2PA_TARGET_DIR})
        target_link_libraries(${TARGET_NAME} PRIVATE c2pa_c)
    endif()
endif()

include(GNUInstallDirs)
install(TARGETS ${TARGET_NAME} RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
