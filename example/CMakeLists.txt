# Copyright (C) 2017-2023 Jonathan Müller and cppast contributors
# SPDX-License-Identifier: MIT
# found in the top-level directory of this distribution.

function(_cppast_example name)
    add_executable(cppast_example_${name} ${name}.cpp example_parser.hpp)
    target_link_libraries(cppast_example_${name} PUBLIC cppast)

    if(MSVC AND CPPAST_USE_STATIC_LIBS)
	    set_property(TARGET cppast_example_${name} PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
    endif()
endfunction()

_cppast_example(ast_printer)
_cppast_example(comparison)
_cppast_example(documentation_generator)
_cppast_example(enum_category)
_cppast_example(enum_to_string)
_cppast_example(serialization)
