cmake_minimum_required(VERSION 3.20)
project(server LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(OUTPUT_BASE ${CMAKE_SOURCE_DIR}/bin)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${OUTPUT_BASE}/out)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${OUTPUT_BASE}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${OUTPUT_BASE}/lib)

foreach(cfg Debug Release RelWithDebInfo MinSizeRel)
    string(TOUPPER ${cfg} CFG_UPPER)
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_${CFG_UPPER} ${OUTPUT_BASE}/${cfg}/out)
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_${CFG_UPPER} ${OUTPUT_BASE}/${cfg}/lib)
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_${CFG_UPPER} ${OUTPUT_BASE}/${cfg}/lib)
endforeach()

file(GLOB_RECURSE PROJECT_SOURCES CONFIGURE_DEPENDS
    ${CMAKE_SOURCE_DIR}/src/*.cpp
)

add_executable(${PROJECT_NAME} ${PROJECT_SOURCES})

target_include_directories(${PROJECT_NAME}
    PRIVATE 
    ${CMAKE_SOURCE_DIR}/inc
    ${CMAKE_SOURCE_DIR}/lib/include
)

set(THIRD_PARTY_LIB_DIR ${CMAKE_SOURCE_DIR}/lib/library)
file(GLOB THIRD_PARTY_LIBS CONFIGURE_DEPENDS
    "${THIRD_PARTY_LIB_DIR}/*.a"
    "${THIRD_PARTY_LIB_DIR}/*.so"
    "${THIRD_PARTY_LIB_DIR}/*.dylib"
    "${THIRD_PARTY_LIB_DIR}/*.lib"
)
if(THIRD_PARTY_LIBS)
    target_link_libraries(${PROJECT_NAME} PRIVATE ${THIRD_PARTY_LIBS})
endif()
find_package(Boost REQUIRED COMPONENTS system)
find_package(Threads REQUIRED)

target_link_libraries(${PROJECT_NAME}
    PRIVATE
        Boost::system
        Threads::Threads
)