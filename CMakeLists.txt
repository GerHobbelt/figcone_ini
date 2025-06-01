cmake_minimum_required(VERSION 3.18)

project(figcone_ini VERSION 1.2.0 DESCRIPTION "inifile-cpp parser adapter for figcone library")
include(external/seal_lake)

SealLake_v030_Bundle(
        NAME figcone_inicpp
        GIT_REPOSITORY https://github.com/kamchatka-volcano/inifile-cpp.git
        GIT_TAG v0.1.0
        SKIP_LOAD
        TEXT_REPLACEMENTS
            "namespace ini" "namespace figcone::ini"
)

SealLake_v030_Bundle(
        NAME figcone_ini_eel
        GIT_REPOSITORY https://github.com/kamchatka-volcano/eel.git
        GIT_TAG v0.2.0
        TEXT_REPLACEMENTS
            "namespace eel" "namespace figcone::ini::eel"
            "EEL_" "FIGCONE_INI_EEL_"
)

SealLake_v030_IsInstallEnabled(INSTALL_FIGCONE_TREE)
SealLake_v030_Import(
        figcone_tree 2.2.0
        GIT_REPOSITORY https://github.com/kamchatka-volcano/figcone_tree.git
        GIT_TAG dev
)

SealLake_v030_ObjectLibrary(
        NAMESPACE figcone
        COMPILE_FEATURES cxx_std_17
        SOURCES
            src/parser.cpp
            src/paramparser.cpp
            src/stream.cpp
            src/utils.cpp
        INCLUDES
            ${SEAL_LAKE_SOURCE_figcone_inicpp}/include
        INTERFACE_LIBRARIES
            figcone::figcone_tree
        LIBRARIES
            figcone_ini_eel::figcone_ini_eel
        DEPENDENCIES
            figcone_tree 2.2.0
)

SealLake_v030_OptionalSubProjects(tests)
