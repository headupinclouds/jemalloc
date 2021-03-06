set(TESTS_INTEGRATION
    MALLOCX_ARENA.c
    aligned_alloc.c
    allocated.c
    # extent.c # not a valid test
    mallocx.c
    overflow.c
    posix_memalign.c
    rallocx.c
    sdallocx.c
    thread_arena.c
    thread_tcache_enabled.c
    xallocx.c
    )

##########################################################################
# Build integration tests
# This links to SO library and UTIL_INTEGRATION
foreach(sourcefile ${TESTS_INTEGRATION})
    get_filename_component(exename ${sourcefile} NAME_WE)
    set(exename "${exename}${install_suffix}")
    add_executable(${exename} ${sourcefile})

    target_include_directories(${exename} PRIVATE ${PROJECT_SOURCE_DIR}/test/include)
    target_include_directories(${exename} PRIVATE "$<BUILD_INTERFACE:${JEMALLOC_INC_DIR}>")    
    target_compile_definitions(${exename} PRIVATE JEMALLOC_INTEGRATION_TEST)

    target_link_libraries(${exename} ${C_TESTLIB_INTEGRATION} ${PROJECT_NAME})
    target_link_libraries(${exename} pthread)

    add_test(NAME ${exename} COMMAND ${exename})
    add_dependencies(check ${exename})
endforeach(sourcefile ${TESTS_UNIT})
