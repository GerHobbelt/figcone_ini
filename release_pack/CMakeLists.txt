cmake_minimum_required(VERSION 3.18)

project(figcone_ini VERSION 1.1.0 DESCRIPTION "inifile-cpp parser adapter for figcone library")
include(seal_lake.cmake)

SealLake_v030_Bundle(
        NAME figcone_ini_eel
        URL https://github.com/kamchatka-volcano/eel/releases/download/v0.2.0/eel-v0.2.0.zip
        TEXT_REPLACEMENTS
        "namespace eel" "namespace figcone::ini::eel"
        "EEL_" "FIGCONE_INI_EEL_"
)
SealLake_v030_Copy(
        SOURCE_PATH ${SEAL_LAKE_SOURCE_figcone_ini_eel}
        DIRECTORIES .
        DESTINATION deps/figcone_ini_eel
)

find_package(figcone_tree 2.2.0)
if (NOT figcone_tree_FOUND)
    SealLake_v030_IsInstallEnabled(INSTALL_FIGCONE_TREE)
    SealLake_v030_Bundle(
            NAME figcone_tree
            URL https://github.com/kamchatka-volcano/figcone_tree/releases/download/v2.2.0-beta/figcone_tree-v2.2.0-beta.zip
    )
    SealLake_v030_Copy(
            SOURCE_PATH ${SEAL_LAKE_SOURCE_figcone_tree}
            DIRECTORIES .
            DESTINATION deps/figcone_tree
    )
endif()

SealLake_v030_ObjectLibrary(
        NAMESPACE figcone
        COMPILE_FEATURES cxx_std_17
        SOURCES
            src/parser.cpp
            src/paramparser.cpp
            src/stream.cpp
            src/utils.cpp
        INCLUDES
            ${CMAKE_CURRENT_LIST_DIR}/deps/figcone_inicpp/include
        INTERFACE_LIBRARIES
            figcone::figcone_tree
        LIBRARIES
            figcone_ini_eel::figcone_ini_eel
        DEPENDENCIES
            figcone_tree 2.2.0
)

SealLake_v030_OptionalSubProjects(tests)
