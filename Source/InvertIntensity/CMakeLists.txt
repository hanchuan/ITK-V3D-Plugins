CMAKE_MINIMUM_REQUIRED(VERSION 2.4)
IF(COMMAND CMAKE_POLICY)
  CMAKE_POLICY(SET CMP0003 NEW)
ENDIF(COMMAND CMAKE_POLICY)

SET(PLUGIN_NAME ITKInvertIntensity)

PROJECT(${PLUGIN_NAME})

FIND_PATH(V3D_BINARY_DIR v3d)

IF( V3D_BINARY_DIR )

  FILE(MAKE_DIRECTORY ${V3D_BINARY_DIR}/plugins/${PLUGIN_NAME})

  IF(NOT LIBRARY_OUTPUT_PATH)
    SET (LIBRARY_OUTPUT_PATH 
      ${V3D_BINARY_DIR}/plugins/${PLUGIN_NAME} 
      CACHE INTERNAL "Output directory for the public.")
  ENDIF(NOT LIBRARY_OUTPUT_PATH)
ELSE( V3D_BINARY_DIR )
  MESSAGE("V3D_BINARY_DIR is not set, the plugin will be built but will not be installed")
ENDIF( V3D_BINARY_DIR )

INCLUDE_DIRECTORIES(
  ${PROJECT_SOURCE_DIR}
  )

SET(QtITK_SRCS ITKInvertIntensity.cxx )

QT4_WRAP_CPP(QT_MOC_SRCS ITKInvertIntensity.h)

ADD_LIBRARY(${PLUGIN_NAME} SHARED ${QtITK_SRCS} ${QT_MOC_SRCS})
TARGET_LINK_LIBRARIES(${PLUGIN_NAME} ITKIO ${QT_LIBRARIES})

INSTALL(TARGETS ${PLUGIN_NAME}
  LIBRARY DESTINATION ${INSTALLATION_DIRECTORY}/${PLUGIN_NAME} COMPONENT RuntimeLibraries
  )

