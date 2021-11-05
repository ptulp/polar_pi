##---------------------------------------------------------------------------
## Author:      nohal aka. Pavel Kalian
## Copyright:   
## License:     wxWidgets License
##---------------------------------------------------------------------------
## Frontend Author: Jon Gough  Frontend2 Version: 1.0.176.1  Date:04/10/2021  DD/MM/YYYY       
##  buildCI:yes useODAPI:no validateJSON:no  
## ---------------------------------------------------------------------------
## ----- When changing this file do NOT change the order in which sections occur        ----- ##
## ----- Changes should only be made between the section blocks that identify where     ----- ##
## ----- these changes should be. The whole configuration process relies on this        ----- ##
## ----- sequence to be successful                                                      ----- ##
##  
##----- Modify section below to include all the details for your plugin ----- ##
##

set(CMLOC "CMakeLists: ")

# define minimum cmake version
cmake_minimum_required(VERSION 3.1.1)
if(COMMAND cmake_policy)
    if(POLICY CMP0043)
        cmake_policy(SET CMP0043 NEW)
    endif(POLICY CMP0043)
    cmake_policy(SET CMP0048 NEW)
    if(POLICY CMP0077)
        cmake_policy(SET CMP0077 NEW)
    endif(POLICY CMP0077)
endif(COMMAND cmake_policy)

# define plugin name, owner and versions
set(VERBOSE_NAME "Polar")
set(COMMON_NAME "Polar")  # Search "getcommonname"in <plugin_pi>.cpp
set(TITLE_NAME "Polar")
set(PACKAGE_CONTACT "Peter Tulp")
set(PACKAGE "Polar")
set(SHORT_DESCRIPTION "Polar Plugin makes polar diagrams for Weather Routing Predictions")
set(LONG_DESCRIPTION "Polar Plugin reads VDR data files directly, reads from Opencpn input Nmea channel (Record Start/Stop at bottom) and direct from LogBoookKonni records, to creates a polar diagram which can then be edited manually, saved or loaded as a Boat Polar file. The LogbookKonni selection has several options.")

set(VERSION_MAJOR "1")
set(VERSION_MINOR "1")
set(VERSION_PATCH "20")
set(VERSION_TWEAK "0")
set(VERSION_DATE "12/10/2021")# DD/MM/YYYY format
set(OCPN_MIN_VERSION "ov50")
set(OCPN_API_VERSION_MAJOR "1")
set(OCPN_API_VERSION_MINOR "16")
set(TP_COMMENT "  * Release for O5 using CI")

set(PARENT "opencpn")

#Used to set up a local webserver with git loaded, for testing purposes.
# The default is shown below, but it is done in cmake/pluginconfigure.cmake now.
# This default is needed to build locally and to build flatpak.
#set(GIT_REPOSITORY_SERVER "github.com")

#No longer needed, queried.
# set(GIT_USER "rgleason")
# set(GIT_REPOSITORY_NAME "polar_pi")

# Specifies Cloudsmith upload repository suffix for each catalog
set(PROD "prod")    #Standard Repos
set(BETA "beta")    #Standard Repos
set(ALPHA "alpha")  #Standard Repos
# Set if your Cloudsmith Base Repository name does not match your Git repository name.
# set(CLOUDSMITH_BASE_REPOSITORY "${GIT_REPOSITORY_NAME}")
set (CLOUDSMITH_BASE_REPOSITORY "polar")  #without the pi 

#set(CLOUDSMITH_USER "rick-gleason")
set(CLOUDSMITH_USER "opencpn")

set(XML_INFO_URL "https://opencpn.org/OpenCPN/plugins/polar.html")
set(XML_SUMMARY ${SHORT_DESCRIPTION})
set(XML_DESCRIPTION ${LONG_DESCRIPTION})

## ----- Modify section above to include all the details for your plugin ----- ##
## ----- Modify section below if there are special requirements for the plugin ----- ##

option(PLUGIN_USE_SVG "Use SVG graphics" ON)

set(CMAKE_CXX_STANDARD 11)

# Use local version of GLU library
#set(USE_LOCAL_GLU TRUE)
# Prefer libGL.so to libOpenGL.so, see CMP0072
set(OpenGL_GL_PREFERENCE "LEGACY")
# Don't use local version of GLU library
set(USE_LOCAL_GLU FALSE)

option(USE_GL "Enable OpenGL support" OFF)
message(STATUS "${CMLOC}USE_GL: ${USE_GL}")

if("${CMAKE_BUILD_TYPE}" STREQUAL "")
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the type of build" FORCE)
endif("${CMAKE_BUILD_TYPE}" STREQUAL "")
set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "" "Debug" "Release" "RelWithDebInfo" "MinSizeRel")
message(STATUS "${CMLOC}Build type: ${CMAKE_BUILD_TYPE}")

## ----- The statements below are used to setup standard variables that are required by the CMAKE process - do not remove ----- ##

project(${PACKAGE})
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake;")

##======================================
include("PluginSetup")
##======================================
## The above is used to setup standard variables that are required by the CMAKE process - do not remove ----- ##
## Modify section below if there are special requirements for the plugin ----- ##

set(CMAKE_VERBOSE_MAKEFILE "Activate verbose mode for make files" ON)

option(Plugin_CXX11 "Use c++11" OFF)

## Modify section above if there are special requirements for the plugin ----- ##
## do not change next section - needed to configure build process ----- ##
##======================================
include("PluginConfigure")
##======================================
## do not change section above - needed to configure build process ----- ##
##Change below to match project requirements for source, headers, etc. ----- ##

add_definitions(-DUSE_S57)

SET(SRCS
            src/polar_pi.h
            src/polar_pi.cpp
            src/PolarGui.h
            src/PolarGui.cpp
            src/PolarDialog.h
            src/PolarDialog.cpp	
            src/Options.h			
            src/Polar.h
            src/Polar.cpp
            src/icons.h 
            src/icons.cpp
	        src/jsonreader.h
	        src/jsonreader.cpp
	        src/jsonwriter.h
	        src/jsonwriter.cpp
	        src/jsonval.h
	        src/jsonval.cpp
	        src/json_defs.h
)

SET(HDRS
            src/polar_pi.h
            src/PolarGui.h
            src/PolarDialog.h
            src/Options.h			
            src/Polar.h
            src/icons.h 
	        src/jsonreader.h
	        src/jsonwriter.h
	        src/jsonval.h
	        src/json_defs.h
)

SET(SRC_LNMEA0183
		src/nmea0183/LatLong.hpp
		src/nmea0183/latlong.cpp
		src/nmea0183/long.cpp
		src/nmea0183/nmea0183.cpp
		src/nmea0183/nmea0183.hpp
		src/nmea0183/Response.hpp
		src/nmea0183/response.cpp
		src/nmea0183/RMB.hpp
		src/nmea0183/rmb.cpp
		src/nmea0183/Sentence.hpp
		src/nmea0183/sentence.cpp
		src/nmea0183/talkerid.cpp
		src/nmea0183/RMC.HPP
		src/nmea0183/rmc.cpp
		src/nmea0183/hexvalue.cpp
		src/nmea0183/lat.cpp
		src/nmea0183/expid.cpp
		src/nmea0183/wpl.hpp
		src/nmea0183/wpl.cpp
		src/nmea0183/rte.hpp
		src/nmea0183/rte.cpp
		src/nmea0183/hdt.hpp
		src/nmea0183/hdt.cpp
		src/nmea0183/hdg.hpp
		src/nmea0183/hdg.cpp
		src/nmea0183/hdm.hpp
		src/nmea0183/hdm.cpp
		src/nmea0183/gll.hpp
		src/nmea0183/gll.cpp
		src/nmea0183/vtg.hpp
		src/nmea0183/vtg.cpp
		src/nmea0183/gga.hpp
		src/nmea0183/gga.cpp
		src/nmea0183/gsv.hpp
		src/nmea0183/gsv.cpp
		src/nmea0183/dbt.cpp
		src/nmea0183/dbt.hpp
		src/nmea0183/dpt.cpp
		src/nmea0183/dpt.hpp
		src/nmea0183/mtw.cpp
		src/nmea0183/mtw.hpp
		src/nmea0183/mwv.cpp
		src/nmea0183/mwv.hpp
		src/nmea0183/vhw.hpp
		src/nmea0183/vhw.cpp
		src/nmea0183/vwr.cpp
		src/nmea0183/vwr.hpp
		src/nmea0183/vwt.cpp
		src/nmea0183/vwt.hpp
		src/nmea0183/zda.cpp
		src/nmea0183/zda.hpp
		src/nmea0183/rsa.hpp
		src/nmea0183/rsa.cpp
		src/nmea0183/SatInfo.h
        )

add_definitions(-DPLUGIN_USE_SVG)

# =================================================
set(EXTINCLUDE_DIR ${EXTINCLUDE_DIR} extinclude libs/ocpn-api/)
# =================================================

# Modify/configure 'Include' Directories below for the Plugin

include_directories(BEFORE ${PROJECT_SOURCE_DIR}/ocpninclude)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/ocpnsrc/glu/include)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/${EXTINCLUDE_DIR})
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/ocpn-api)
#include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/wx/include)
#include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/wxJSON/include)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/src/nmea0183)

## Adjust statement below to collect all the set( headers and SRCS
##  ADD_LIBRARY(${PACKAGE_NAME} SHARED ${SRC_POLAR} ${SRC_LNMEA0183})
#target_sources(${PACKAGE_NAME} PUBLIC ${SRC_POLAR} ${SRC_LNMEA0183} ${EXTSRC} ${LIBSSRC})

##=================================================
target_sources(${PACKAGE_NAME} PUBLIC ${SRCS}  ${HDRS} ${SRC_LNMEA0183} ${EXTSRC})
##=================================================

if(NOT OCPN_FLATPAK_CONFIG)
    # Build environment not available when flatpak is being configured so following statements will not work
    message(STATUS "${CMLOC}Adding target link libraries to ${PACKAGE_NAME}")
#    add_subdirectory(libs/tinyxml)
#    target_link_libraries(${PACKAGE_NAME} ocpn::tinyxml)
endif(NOT OCPN_FLATPAK_CONFIG)
	
add_definitions(-DTIXML_USE_STL)

if(UNIX AND NOT APPLE AND NOT QT_ANDROID)
    if(PI_JSON_SCHEMA_VALIDATOR)
        # Change OS/ABI to be UNIX - System V to make it run with OCPN
        set(lib_name lib${PACKAGE_NAME}.so)
        message(STATUS "${CMLOC}lib-name: ${lib_name}")
        find_file(elfedit elfedit)
        if(NOT (${elfedit} STREQUAL "elfedit-NOTFOUND"))
            message(STATUS "${CMLOC}Will ensure OS/ABI is set to 0 to match OCPN")
            add_custom_command(
                TARGET ${PACKAGE_NAME}
                POST_BUILD
                WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
                DEPENDS ${PACKAGE_NAME}
                COMMENT " Running post build action on ${lib_name}."
                COMMAND sh -c 'elfedit --input-osabi=Linux --output-osabi=none ${lib_name} || (echo 0) ')
        else(NOT (${elfedit} STREQUAL "elfedit-NOTFOUND"))
            message(STATUS "${CMLOC}Cannot correct OS/ABI to match OCPN if it is generated incorrectly. Package binutils needed")
        endif(NOT (${elfedit} STREQUAL "elfedit-NOTFOUND"))
    endif(PI_JSON_SCHEMA_VALIDATOR)
endif(UNIX AND NOT APPLE AND NOT QT_ANDROID)

# Not using Json Schema Validator

#These are NOT used in Testplugin, sometimes used for other plugins
#include("cmake/PluginJSON.cmake")
#include("cmake/PluginXML.cmake")
#include("cmake/PluginGL.cmake")

## ----- Change above to match project requirements ----- ##
## ----- do not change next section - needed to configure build process ----- ##

# Needed for android builds
if(QT_ANDROID)
    include_directories(BEFORE ${qt_android_include})
endif(QT_ANDROID)

# Needed for all builds
##======================================			  
include("PluginInstall")
include("PluginLocalization")
include("PluginPackage")
##======================================
## ----- do not change section above - needed to configure build process ----- ##

message(STATUS "${CMLOC}include directories: ")
get_property(dirs DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY INCLUDE_DIRECTORIES)
foreach(dir ${dirs})
    message(STATUS "${CMLOC}dir='${dir}'")
endforeach()
