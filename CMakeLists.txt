cmake_minimum_required(VERSION 3.0)

set(BZIP2_VERSION_MAJOR 1)
set(BZIP2_VERSION_MINOR 0)
set(BZIP2_VERSION_MICRO 8)
set(BZIP2_VERSION ${BZIP2_VERSION_MAJOR}.${BZIP2_VERSION_MINOR}.${BZIP2_VERSION_MICRO})

project(bzip2 VERSION ${BZIP2_VERSION})

if (MSVC)
    configure_file(version.dll.rc.in ${CMAKE_SOURCE_DIR}/version.dll.rc)
    configure_file(version.bzip2.rc.in ${CMAKE_SOURCE_DIR}/version.bzip2.rc)
    set(DLLRC version.dll.rc)
    set(PRORC version.bzip2.rc)
endif()

set(LIB_OBJS blocksort.c huffman.c crctable.c randtable.c compress.c decompress.c bzlib.c)

add_library(libbz2-static STATIC ${LIB_OBJS})
add_library(libbz2 SHARED ${LIB_OBJS} ${DLLRC})
add_executable(bzip2 bzip2.c ${PRORC})
target_link_libraries(bzip2 libbz2)
add_executable(bzip2recover bzip2recover.c ${PRORC})

install(TARGETS libbz2-static libbz2 bzip2 bzip2recover)
