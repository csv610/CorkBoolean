cmake_minimum_required(VERSION 3.10)
project(cork)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(APPLE)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++ -Wno-c++11-extensions")
endif()

find_package(PkgConfig REQUIRED)
pkg_check_modules(GMP REQUIRED gmp gmpxx)

include_directories(
    ${GMP_INCLUDE_DIRS}
    /Users/csv610/Projects/Mesh/Cork/src
    /Users/csv610/Projects/Mesh/Cork/src/util
    /Users/csv610/Projects/Mesh/Cork/src/math
    /Users/csv610/Projects/Mesh/Cork/src/isct
    /Users/csv610/Projects/Mesh/Cork/src/mesh
    /Users/csv610/Projects/Mesh/Cork/src/rawmesh
    /Users/csv610/Projects/Mesh/Cork/src/accel
    /Users/csv610/Projects/Mesh/Cork/src/file_formats
)
link_directories(${GMP_LIBRARY_DIRS})

set(CORK_SOURCES
    src/cork.cpp
    src/util/timer.cpp
    src/util/log.cpp
    src/isct/empty3d.cpp
    src/isct/quantization.cpp
    src/file_formats/files.cpp
    src/file_formats/ifs.cpp
    src/file_formats/off.cpp
)

set(CORK_HEADERS
    src/cork.h
    src/util/prelude.h
    src/util/memPool.h
    src/util/iterPool.h
    src/util/shortVec.h
    src/util/unionFind.h
    src/math/vec.h
    src/math/bbox.h
    src/math/ray.h
    src/isct/unsafeRayTriIsct.h
    src/isct/ext4.h
    src/isct/fixext4.h
    src/isct/gmpext4.h
    src/isct/absext4.h
    src/isct/quantization.h
    src/isct/fixint.h
    src/isct/empty3d.h
    src/isct/triangle.h
    src/rawmesh/rawMesh.h
    src/mesh/mesh.h
    src/mesh/mesh.decl.h
    src/accel/aabvh.h
    src/file_formats/files.h
)

add_library(cork STATIC ${CORK_SOURCES} ${CORK_HEADERS})
target_link_libraries(cork ${GMP_LIBRARIES})

add_library(cork_triangle OBJECT src/isct/triangle.c)
target_compile_options(cork_triangle PRIVATE
    -O2 -DNO_TIMER -DREDUCED -DCDT_ONLY -DTRILIBRARY -Wall -DANSI_DECLARATORS
)
target_include_directories(cork_triangle PRIVATE ${GMP_INCLUDE_DIRS})

add_library(cork_triangle_lib STATIC $<TARGET_OBJECTS:cork_triangle>)
target_link_libraries(cork cork_triangle_lib ${GMP_LIBRARIES})

add_executable(cork_cli /Users/csv610/Projects/Mesh/Cork/src/main.cpp)
target_link_libraries(cork_cli cork ${GMP_LIBRARIES})

add_executable(off2obj /Users/csv610/Projects/Mesh/Cork/src/off2obj.cpp)
target_link_libraries(off2obj cork ${GMP_LIBRARIES})

enable_testing()

add_executable(cork_test /Users/csv610/Projects/Mesh/Cork/tests/cork_test.cpp)
target_link_libraries(cork_test cork ${GMP_LIBRARIES})

add_test(NAME cork_test COMMAND cork_test)

install(TARGETS cork cork_triangle_lib off2obj
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)