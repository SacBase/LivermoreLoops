CMAKE_MINIMUM_REQUIRED (VERSION 3.3)
INCLUDE ("${CMAKE_SOURCE_DIR}/cmake-common/sac2c-variables.cmake")
INCLUDE ("${CMAKE_SOURCE_DIR}/cmake-common/resolve-sac2c-dependencies.cmake")
INCLUDE ("${CMAKE_SOURCE_DIR}/cmake-common/build-sac2c-program.cmake")
INCLUDE ("${CMAKE_SOURCE_DIR}/cmake-common/sac2c-prog-with-variants.cmake")
INCLUDE ("${CMAKE_SOURCE_DIR}/cmake-common/generate-variant-vars.cmake")

# SaC files, relative to this CMakeLists.txt.
SET (SAC_PROG_SRC
    loop03.sac
)

# For every sac file, compile Tree and Mod files.
FOREACH (name ${SAC_PROG_SRC})
    GENERATE_VARIANT_NAMES( ${name})
    COMPILE_SAC2C_WITH_VER (${name} "" "${SAC2C_FLAGS}")
ENDFOREACH (name)


