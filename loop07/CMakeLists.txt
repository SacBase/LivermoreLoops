CMAKE_MINIMUM_REQUIRED (VERSION 3.3)
INCLUDE ("${CMAKE_SOURCE_DIR}/cmake-common/sac2c-variables.cmake")
INCLUDE ("${CMAKE_SOURCE_DIR}/cmake-common/resolve-sac2c-dependencies.cmake")
INCLUDE ("${CMAKE_SOURCE_DIR}/cmake-common/build-sac2c-program.cmake")
INCLUDE ("${CMAKE_SOURCE_DIR}/cmake-common/sac2c-prog-with-variants.cmake")
INCLUDE ("${CMAKE_SOURCE_DIR}/cmake-common/generate-variant-namelists.cmake")
INCLUDE ("${CMAKE_SOURCE_DIR}/cmake-common/ctest-sac2c-with-variants.cmake")

# SaC files, relative to this CMakeLists.txt.
SET (SAC_PROG_SRC
    loop07.sac
)

# For every sac file, compile Tree and Mod files, generate ctest entries
FOREACH (name ${SAC_PROG_SRC})
    GENERATE_VARIANT_NAMELISTS( ${name})
    COMPILE_SAC2C_WITH_VARIANTS (${name} "" "${SAC2C_FLAGS}")
    CTEST_SAC2C_WITH_VARIANTS   (${name} "" "${SAC2C_FLAGS}")
ENDFOREACH (name)


