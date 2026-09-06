cmake_minimum_required(VERSION 3.12)

project(egl_glx
  VERSION 0.1.0
  DESCRIPTION "Translates EGL calls into GLX"
  HOMEPAGE_URL "https://github.com/piernov/egl_glx"
  LANGUAGES C)

include(GNUInstallDirs)

find_package(OpenGL COMPONENTS GLX REQUIRED)
find_package(X11 REQUIRED)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra -pedantic")

add_library(EGL_glx SHARED
  eglapi.c
  eglarray.c
  eglconfig.c
  eglcontext.c
  eglcurrent.c
  egldisplay.c
  egldriver.c
  eglfallbacks.c
  eglglobals.c
  egllog.c
  eglmisc.c
  eglstring.c
  eglsurface.c
  eglsync.c
  egl_glx.c
)

target_compile_definitions(EGL_glx PUBLIC
  -DHAVE_PTHREAD
  -DHAVE_X11_PLATFORM
  -DUSE_X11
  -D_EGL_NATIVE_PLATFORM=_EGL_PLATFORM_X11
  -D_EGL_OS_UNIX
  -D_EGL_DRIVER_SEARCH_DIR=\"\"
  -D_EGL_BUILT_IN_DRIVER_GLX
)

target_include_directories(EGL_glx PUBLIC
  ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_link_libraries(EGL_glx PUBLIC
  OpenGL::GLX
  X11::X11
)

set_target_properties(EGL_glx PROPERTIES VERSION ${CMAKE_PROJECT_VERSION} SOVERSION ${CMAKE_PROJECT_VERSION_MAJOR})

install(TARGETS EGL_glx LIBRARY
  DESTINATION ${CMAKE_INSTALL_LIBDIR})

## CPack ##
set(CPACK_PACKAGE_CONTACT "piernov <piernov@piernov.org>")
set(CPACK_GENERATOR DEB CACHE STRING "List of generators to build packages with")
set(CPACK_DEBIAN_PACKAGE_DEPENDS "libx11-6")
set(CPACK_DEBIAN_PACKAGE_SECTION "libs")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
set(CPACK_DEBIAN_FILE_NAME DEB-DEFAULT)
set(CPACK_DEBIAN_PACKAGE_NAME libegl-glx)

include(CPack)
