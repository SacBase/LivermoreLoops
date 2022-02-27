CMAKE_MINIMUM_REQUIRED (VERSION 3.3)

PROJECT (sac-LivermoreLoops)

# Where the compiled sac modules result
SET (DLL_BUILD_DIR  "${PROJECT_BINARY_DIR}/lib")

# For what targets we build modules
SET (TARGETS            seq mt_pth  CACHE STRING "Build stdlib-jpeg for these targets")
SET (SAC2C_EXEC                     CACHE STRING "A path to sac2c compiler")
SET (LINKSETSIZE        "0"         CACHE STRING "Set a value for -linksetsize parameter of sac2c")

# Check whether sac2c is operational
INCLUDE ("cmake-common/check-sac2c.cmake")
INCLUDE ("cmake-common/misc-macros.cmake")

SET (SAC2C_FLAGS -check a -v0 -O3 -maxlur 3 -linksetsize ${LINKSETSIZE} -L ${DLL_BUILD_DIR} -T ${DLL_BUILD_DIR})

ENABLE_TESTING() # This must precede any test creation

# Get Filename, sans path, for each test
FILE(GLOB BM_DIRST ${CMAKE_SOURCE_DIR}/loop*)

# For every target run CMakeLists.txt in src
FOREACH (TARGET IN ITEMS ${TARGETS})
  FOREACH( BM_DIR IN ITEMS ${BM_DIRST})
    MESSAGE (STATUS "Adding SUBDIRECTORY " ${BM_DIR} " for " ${TARGET})
    GET_FILENAME_COMPONENT( BM_NAME ${BM_DIR} NAME)
    ADD_SUBDIRECTORY (${BM_NAME} ${BM_NAME}-${TARGET})
  ENDFOREACH ()
ENDFOREACH ()

