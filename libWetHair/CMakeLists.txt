# libWetHair Executable

append_files (Headers "h" . pcgsolver)
append_files (Sources "cpp" . pcgsolver)
append_files (Sources "c" . pcgsolver)

append_files (Headers "h" . DER)
append_files (Sources "cpp" . DER)
append_files (Sources "c" . DER)

append_files (Headers "h" . DER/Forces)
append_files (Sources "cpp" . DER/Forces)
append_files (Sources "c" . DER/Forces)

append_files (Headers "h" . DER/Dependencies)
append_files (Sources "cpp" . DER/Dependencies)
append_files (Sources "c" . DER/Dependencies)

#find_package (TCLAP REQUIRED)
#include_directories (${TCLAP_INCLUDE_DIR})

add_definitions(-DBOOST_ALL_NO_LIB)
set(Boost_USE_STATIC_LIBS OFF) 
set(Boost_USE_MULTITHREADED ON)  
set(Boost_USE_STATIC_RUNTIME OFF) 
find_package(Boost COMPONENTS serialization) 
if(Boost_FOUND)
  include_directories(${Boost_INCLUDE_DIRS}) 
  set (LIBWETHAIR_LIBRARIES ${Boost_LIBRARIES})
else (Boost_FOUND)
  message (SEND_ERROR "Unable to locate Boost")
endif()

# Locate OpenGL
find_package (OpenGL REQUIRED)
if (OPENGL_FOUND)
  include_directories (${OPENGL_INCLUDE_DIR})
  set (LIBWETHAIR_LIBRARIES ${LIBWETHAIR_LIBRARIES} ${OPENGL_LIBRARIES})
else (OPENGL_FOUND)
  message (SEND_ERROR "Unable to locate OpenGL")
endif (OPENGL_FOUND)

find_package(GLEW REQUIRED)
if (GLEW_FOUND)
  include_directories(${GLEW_INCLUDE_DIRS})
  set (LIBWETHAIR_LIBRARIES ${LIBWETHAIR_LIBRARIES} ${GLEW_LIBRARIES})
else (GLEW_FOUND)
  message (SEND_ERROR "Unable to locate GLEW")
endif()

# Locate NANOFLANN
find_package (NANOFLANN REQUIRED)
include_directories( ${NANOFLANN_INCLUDE_DIR} )

find_package (TBB REQUIRED)
if (TBB_FOUND)
  include_directories (${TBB_INCLUDE_DIRS})
  set (LIBWETHAIR_LIBRARIES ${LIBWETHAIR_LIBRARIES} ${TBB_LIBRARIES})
else (TBB_FOUND)
  message (SEND_ERROR "Unable to locate TBB")
endif (TBB_FOUND)

# Locate GLUT
find_package (GLUT REQUIRED glut)
if (GLUT_FOUND)
  include_directories (${GLUT_INCLUDE_DIR})
  set (LIBWETHAIR_LIBRARIES ${LIBWETHAIR_LIBRARIES} ${GLUT_glut_LIBRARY})
else (GLUT_FOUND)
  message (SEND_ERROR "Unable to locate GLUT")
endif (GLUT_FOUND)

# RapidXML library is required
find_package (RapidXML REQUIRED)
if (RAPIDXML_FOUND)
  include_directories (${RAPIDXML_INCLUDE_DIR})
else (RAPIDXML_FOUND)
  message (SEND_ERROR "Unable to locate RapidXML")
endif (RAPIDXML_FOUND)

# TCLAP library is required
find_package (TCLAP REQUIRED)
if (TCLAP_FOUND)
  include_directories (${TCLAP_INCLUDE_PATH})
else (TCLAP_FOUND)
  message (SEND_ERROR "Unable to locate TCLAP")
endif (TCLAP_FOUND)

# Locate AntTweakBar
find_package (ANTTWEAKBAR REQUIRED)
if (ANTTWEAKBAR_FOUND)
  include_directories( ${ANT_TWEAK_BAR_INCLUDE_DIR} )
  set (LIBWETHAIR_LIBRARIES ${LIBWETHAIR_LIBRARIES} ${ANT_TWEAK_BAR_LIBRARY})  
else (ANTTWEAKBAR_FOUND)
  message (SEND_ERROR "Unable to locate ANTTWEAKBAR")
endif (ANTTWEAKBAR_FOUND)  

find_package (PNG REQUIRED)
if (PNG_FOUND)
  add_definitions (${PNG_DEFINITIONS})
  include_directories (${PNG_INCLUDE_DIR})
  set (LIBWETHAIR_LIBRARIES ${LIBWETHAIR_LIBRARIES} ${PNG_LIBRARIES})
endif (PNG_FOUND)

find_package(Threads REQUIRED)
if(THREADS_HAVE_PTHREAD_ARG)
  set_property(TARGET libWetHair PROPERTY COMPILE_OPTIONS "-pthread")
  set_property(TARGET libWetHair PROPERTY INTERFACE_COMPILE_OPTIONS "-pthread")
endif()
if(CMAKE_THREAD_LIBS_INIT)
  set(LIBWETHAIR_LIBRARIES ${LIBWETHAIR_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT})
endif()

add_executable (libWetHair ${Headers} ${Templates} ${Sources})
target_link_libraries (libWetHair ${LIBWETHAIR_LIBRARIES} "-lpthread" )

INSTALL_TARGETS(/bin libWetHair)
