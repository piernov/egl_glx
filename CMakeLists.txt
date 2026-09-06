cmake_minimum_required(VERSION 3.7)

project(egl_glx C)

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

install(TARGETS EGL_glx RUNTIME)
