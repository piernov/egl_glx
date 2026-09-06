## egl_glx

Translates EGL calls into GLX. To be used with proprietary drivers that do not provide a proper EGL library.

Original source code extracted from old Mesa tree under `src/egl/drivers/glx`, this source code has long been removed from the upstream tree (in early 2014 with commit 1340e24).
Improvements over original sources:
- Basic EGL 1.5 support: `eglGetPlatformDisplay()`, `eglCreatePlatformWindowSurface()`, `KHR_create_context`
- OpenGL ES/ES2 profile support

This library has been developed as a substitute for NVIDIA's provided `libEGL.so.340.108` from the 340.xx proprietary driver package (legacy branch for Tesla microarchitecture support: GeForce 8000/9000/100/200/300 series).
The NVIDIA library does not support EGL 1.5 and does not expose OpenGL profile (only OpenGL ES), making it useless for most applications.

## Build

```
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr
cmake --build build --parallel
```

## Install

```
sudo cmake --install build
```

## Usage

```
LD_PRELOAD=/usr/lib/libEGL_glx.so <application>
```

Alternatively, for system-wide usage, replace the vendor-provided library by this library.
