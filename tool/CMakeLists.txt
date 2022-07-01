# Copyright (C) 2015-2023 Jonathan Müller and foonathan/memory contributors
# SPDX-License-Identifier: Zlib

# builds tools
add_executable(foonathan_memory_node_size_debugger test_types.hpp node_size_debugger.hpp node_size_debugger.cpp)

if(Sanitizers_FOUND)
    if(SANITIZE_THREAD)
        add_sanitize_thread(foonathan_memory_node_size_debugger)
    endif(SANITIZE_THREAD)

    if(SANITIZE_ADDRESS)
        add_sanitize_address(foonathan_memory_node_size_debugger)
    endif(SANITIZE_ADDRESS)

    if(SANITIZE_MEMORY)
        add_sanitize_memory(foonathan_memory_node_size_debugger)
    endif(SANITIZE_MEMORY)

    if(SANITIZE_UNDEFINED)
        add_sanitize_undefined(foonathan_memory_node_size_debugger)
    endif(SANITIZE_UNDEFINED)
endif()

if (CMAKE_CROSSCOMPILING)
    # statically link when cross compiling so emulator doesn't need library paths
    if (MSVC)
        set_target_properties(foonathan_memory_node_size_debugger PROPERTIES LINK_FLAGS "/WHOLEARCHIVE")
    else()
        set_target_properties(foonathan_memory_node_size_debugger PROPERTIES LINK_FLAGS "-static")
    endif()
endif()

if (MSVC)
    target_compile_options(foonathan_memory_node_size_debugger PRIVATE "/bigobj")
endif()
target_compile_definitions(foonathan_memory_node_size_debugger PUBLIC
                           VERSION="${FOONATHAN_MEMORY_VERSION_MAJOR}.${FOONATHAN_MEMORY_VERSION_MINOR}")
set_target_properties(foonathan_memory_node_size_debugger PROPERTIES OUTPUT_NAME nodesize_dbg)

if(NOT MSVC)
target_compile_features(foonathan_memory_node_size_debugger PUBLIC cxx_constexpr)
endif()

install(TARGETS foonathan_memory_node_size_debugger EXPORT foonathan_memoryTargets
                                                    RUNTIME DESTINATION ${FOONATHAN_MEMORY_RUNTIME_INSTALL_DIR})
