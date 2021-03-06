cmake_minimum_required(VERSION 3.15)
project(OptimizedCXX)

set(CMAKE_CXX_STANDARD 17)

if (MSVC)
    set(ENV{_NO_DEBUG_HEAP} 1)
    add_definitions(-D_ITERATOR_DEBUG_LEVEL=0)
endif()

#
# common
#
add_library(common
    STATIC src/stopwatch.h
           src/stopwatch03.h
           src/stopwatch03.cpp
           src/stopwatch11.h
           src/stopwatch11.cpp
           src/priority_thread.h
           src/test_driver.h
           src/test_driver.cpp)

if(MSVC)
    target_sources(common
        PRIVATE src/Win32.cpp)
else()
    target_sources(common
        PRIVATE src/unix.cpp)
endif()

#
# strings
#
add_executable(strings src/strings.h src/strings.cpp)
target_link_libraries(strings common)

#
# stl allocator
#
add_executable(stl_allocator
    src/simple_allocator.h
    src/old_block_allocator.h
    src/old_stl_block_allocator.cpp
    src/old_stl_allocator_test.cpp)
target_link_libraries(stl_allocator common)

#
# computation
#
add_executable(computation src/Statements.cpp src/computation.cpp)
target_link_libraries(computation common)

#
# stl
#
add_executable(stl
    src/charbuf.h
    src/kvstruct.h
    src/search_maps.cpp
    src/search_algorithms.cpp
    src/search_hash.cpp
    src/vector.cpp
    src/deque.cpp
    src/list.cpp
    src/forward_list.cpp
    src/map.cpp
    src/unordered_map.cpp
    src/stl.cpp)
target_link_libraries(stl common)

#
# file reader
#
add_executable(file_reader src/file_reader.cpp)
target_link_libraries(file_reader common)

#
# concurrency
#
add_executable(concurrency src/concurrency.cpp)
target_link_libraries(concurrency common)

#
# allocation
#
add_executable(allocation
    src/instrumented_class_new.cpp
    src/list_allocator_test.cpp
    src/string_allocator_test.cpp
    src/block_mgr.h
    src/block_mgr.inl
    src/block_mgr_test.cpp
    src/copy_elision_test.cpp
    src/insert_ctrl.cpp
    src/allocation.cpp)
target_link_libraries(allocation common)