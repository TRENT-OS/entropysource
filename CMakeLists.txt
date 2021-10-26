#
# EntropySource driver
#
# Copyright (C) 2020, HENSOLDT Cyber GmbH
#

cmake_minimum_required(VERSION 3.17)


#-------------------------------------------------------------------------------
#
# Declare EntropySource CAmkES Component
#
# Parameters:
#
#   <name>
#     required, component instance name
#
#   The following parameters are optional. If none of them is given, the dummy
#   EntropySource implementation is used. Any of the following parameters can be
#   used to override the dummy defaults.
#
#   INCLUDES <inc_1> <inc_2> ...
#     optional, override dummy include files
#   SOURCES <src_1> <src_2> ...
#     optional, override dummy source files
#   C_FLAGS <flag_1> <flag_2> ...
#     optional, override dummy compile flags
#   LIBS <lib_1> <lib_2> ...
#     optional, override dummy includes
#
function(EntropySource_DeclareCAmkESComponent
    name
    # ...
)

    # Let caller override any of the build options with his own variables
    cmake_parse_arguments(
        PARSE_ARGV
        1
        ENTROPYSOURCE
        ""
        ""
        "INCLUDES;SOURCES;C_FLAGS;LIBS"
    )

    # Fill all those variables which are NOT defined by default values
    if(NOT DEFINED ENTROPYSOURCE_INCLUDES)
        set(ENTROPYSOURCE_INCLUDES
            ""
        )
    endif(NOT DEFINED ENTROPYSOURCE_INCLUDES)

    if(NOT DEFINED ENTROPYSOURCE_SOURCES)
        set(ENTROPYSOURCE_SOURCES
            "${CMAKE_CURRENT_FUNCTION_LIST_DIR}/src/DummySource.c"
        )
        message(WARNING "No EntropySource implementation specified, using DUMMY!")
    endif(NOT DEFINED ENTROPYSOURCE_SOURCES)

    if(NOT DEFINED ENTROPYSOURCE_C_FLAGS)
        set(ENTROPYSOURCE_C_FLAGS
            "-Wall"
            "-Werror"
        )
    endif(NOT DEFINED ENTROPYSOURCE_C_FLAGS)

    if(NOT DEFINED ENTROPYSOURCE_LIBS)
        set(ENTROPYSOURCE_LIBS
            "os_core_api"
        )
    endif(NOT DEFINED ENTROPYSOURCE_LIBS)

    # Declare the component in the most generic way
    DeclareCAmkESComponent(
        ${name}
        INCLUDES
            ${ENTROPYSOURCE_INCLUDES}
        SOURCES
            ${ENTROPYSOURCE_SOURCES}
        C_FLAGS
            ${ENTROPYSOURCE_C_FLAGS}
        LIBS
            ${ENTROPYSOURCE_LIBS}
    )

endfunction()
