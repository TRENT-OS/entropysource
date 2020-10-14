#
# EntropySource driver
#
# Copyright (C) 2020, Hensoldt Cyber GmbH
#

cmake_minimum_required(VERSION 3.7.2)


# we cannot use CMAKE_CURRENT_LIST_DIR in a function or macro, because that
# would give us the dir of the file that is invoking the macro. The work around
# is that we create a global (ie cached and flagged as internal) variable here
# and then use it in the macro below. As a convenient side effect we don't
# require anybody else to provide any helper variables, this CMake file becomes
# fully self-contained and "copy&paste safe".
set(
    OS_COMPONENT_EntropySource_DIR
    "${CMAKE_CURRENT_LIST_DIR}"
    CACHE INTERNAL ""
)


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
function(DeclareCAmkESComponent_EntropySource
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
            "${OS_COMPONENT_EntropySource_DIR}/src/DummySource.c"
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
            "os_libs"
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
