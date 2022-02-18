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

SET (SAC2C_FLAGS -v0 -O3 -maxlur 3 -linksetsize ${LINKSETSIZE} -L ${DLL_BUILD_DIR} -T ${DLL_BUILD_DIR})

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

# Testing section, arguments read from stdin
# Testing section, support for multiple versions
MESSAGE( STATUS  "START TESTING WITH " ${CMAKE_PROJECT_NAME} " and " ${PROJECT_NAME})
MESSAGE (STATUS "Build versions are now: " ${buildversions})

IF(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
    INCLUDE(CTest)
    FOREACH (TARGET IN ITEMS ${TARGETS})
        MESSAGE (STATUS "Testing TARGET is " ${TARGET})
	FOREACH ( BM_DIR IN ITEMS ${BM_DIRST})
          MESSAGE (STATUS "Testing BM_DIR is " ${BM_DIR})
          GET_FILENAME_COMPONENT( BM_NAME ${BM_DIR} NAME)
          # Handle argument filenames
          SET (ARGFNM ${BM_DIR}/${BM_NAME}.inp)
          MESSAGE (STATUS "argfnm is " ${ARGFNM})
          IF (EXISTS ${ARGFNM})
              FILE (STRINGS ${ARGFNM} ARG)
              MESSAGE (STATUS "Value of argument file: " "${ARGFNM} is: "  ${ARG})
          ELSE()
              MESSAGE (STATUS " file: " ${ARGFNM} " does not exist" )
              UNSET (ARG)
          ENDIF()
        ENDFOREACH ()
    ENDFOREACH ()

#Testing from here on
       ##  MESSAGE (STATUS " Project test is:                                  \
       ##Test-${PROJECT_BINARY_DIR}/${BM_NAME}-${TARGET}/${BM_NAME}-${TARGET}  \
       ##-mt ${THREDS} < ${BM_DIR}/${BM_NAME}.inp)

# Add tests for the Cartesian product of BENCHMARKS, TARGETS, THREADS, and BUILDVARIANTS
# E.g., BENCHMARKS are: loop01, loop02, loop03...loop08
#       TARGETS are: seq, mt_pth
#       THREADS are: 1 2 4 8
#       BUILDVARIANTS are (for some benchmarks): apl1, apl2, tc1, tc2
#

  MESSAGE (STATUS "BM_DIRST is " ${BM_DIRST})
  FOREACH ( BM_DIR IN ITEMS ${BM_DIRST})
          GET_FILENAME_COMPONENT( BM_NAME ${BM_DIR} NAME)
          # MESSAGE (STATUS "Adding Test for BM_NAME is " ${BM_NAME})
    FOREACH (TARGET IN ITEMS ${TARGETS})
          FOREACH (THREDS IN ITEMS 1 ) # number of threads
#FIXME add more  THREDS LIST
              MESSAGE (STATUS Test-${BM_DIR}-${TARGET} " with ARG= " ${ARG} " and THREDS= " ${THREDS})
              GENERATE_VARIANT_NAMES( ${BM_NAME})
              message( STATUS " BUILDVARIANTS are" ${BUILDVARIANTS})
              FOREACH (VARS in ${BUILDVARIANTS})
                message( STATUS adding test for ${BM_NAME})
                ADD_TEST (Test-${PROJECT_BINARY_DIR}/${BM_NAME}-${TARGET}
echo ${CMAKE_SOURCE_DIR}${BM_DIR}/${BM_NAME}/${BM_NAME}.inp |
${PROJECT_BINARY_DIR}/${BM_NAME}-${TARGET}/${BM_NAME}-${TARGET} -mt ${THREDS} )
              ENDFOREACH ()
          ENDFOREACH ()
    ENDFOREACH ()
  ENDFOREACH ()
ENDIF()

