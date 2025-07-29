#
# This file was taken from RakNet 4.082.
# Please see licenses/RakNet license.txt for the underlying license and related copyright.
#
#
# Modified work: Copyright (c) 2019, SLikeSoft UG (haftungsbeschränkt)
#
# This source code was modified by SLikeSoft. Modifications are licensed under the MIT-style
# license found in the license.txt file in the root directory of this source tree.
#

cmake_minimum_required(VERSION 2.6)

option( RAKNET_ENABLE_EXTENSIONS 				"Enable extension projects that might need specific dependencies." OFF )
option( RAKNET_EXTENSION_USEMYSQL 				"Enable extensions using MySQL" 		OFF )
option( RAKNET_EXTENSION_USEPOSTGRESQL			"Enable extensions using PostgreSQL" 	OFF )
option( RAKNET_EXTENSION_USEBOOST				"Enable extensions using Boost" 		OFF )
option( RAKNET_EXTENSION_USESPEEX				"Enable extensions using Speex" 		OFF )
option( RAKNET_EXTENSION_USEIRRLICHT			"Enable extensions using Irrlicht" 		OFF )
option( RAKNET_EXTENSION_USEIRRKLANG			"Enable extensions using Irkklang" 		OFF )
option( RAKNET_EXTENSION_USEOGRE3D				"Enable extensions using Ogre3D" 		OFF )

option( RAKNET_EXTENSION_Autopatcher			"Enable Autopatcher extension" 			OFF )


add_subdirectory("bzip2-1.0.6")
add_subdirectory("XML")

if( RAKNET_ENABLE_EXTENSIONS )
	IF(RAKNET_EXTENSION_USEMYSQL ) 
		add_subdirectory(MySQLInterface)
	ENDIF() 

	IF(RAKNET_EXTENSION_USEPOSTGRESQL) 
		add_subdirectory(PostgreSQLInterface)
	ENDIF() 

endif()

if( RAKNET_EXTENSION_Autopatcher )
	add_subdirectory(Autopatcher)
endif()

IF (WIN32 AND NOT UNIX)
	IF(NOT ${CMAKE_GENERATOR} STREQUAL "MSYS Makefiles")
		add_subdirectory(portaudio_v18_1)
	ENDIF(NOT ${CMAKE_GENERATOR} STREQUAL "MSYS Makefiles")
	add_subdirectory(speex-1.1.12)
ENDIF(WIN32 AND NOT UNIX)

if( RAKNET_ENABLE_EXTENSIONS ) 

	IF(RAKNET_EXTENSION_USEBOOST) 
		add_subdirectory(RPC3)
	ENDIF() 

endif()

IF(NOT WIN32 AND UNIX)
	IF(RAKNET_EXTENSION_USESPEEX AND RAKNET_ENABLE_EXTENSIONS) 
		FINDSPEEX()
		include_directories(${SLIKENET_HEADER_FILES} ${SPEEX_INCLUDE_DIRS})
		add_library(LibRakVoice STATIC RakVoice.h RakVoice.cpp)
		target_link_libraries(LibRakVoice ${SLIKENET_COMMON_LIBS} ${SPEEX_LIBRARIES})
	ENDIF() 
ENDIF()

if( RAKNET_ENABLE_EXTENSIONS )

	IF( RAKNET_EXTENSION_USEIRRLICHT AND RAKNET_EXTENSION_USEIRRKLANG ) 
		add_subdirectory(IrrlichtDemo)
	ENDIF()

	IF( RAKNET_EXTENSION_USEOGRE3D ) 
		add_subdirectory(Ogre3DInterpDemo)
	ENDIF()

endif()



