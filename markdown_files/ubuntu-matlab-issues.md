# Issue 1:
## "Could not initialize shared resources for X11GraphicsDevice"
Error Messages:
```
com.jogamp.opengl.GLException: X11GLXDrawableFactory - Could not initialize shared resources for X11GraphicsDevice[type .x11, connection :0, unitID 0, handle 0x0, owner false, ResourceToolkitLock[obj 0x413b9df1, isOwner false, <30fe5aac, af2eb55>[count 0, qsz 0, owner <NULL>]]]
  at jogamp.opengl.x11.glx.X11GLXDrawableFactory$SharedResourceImplementation.createSharedResource(X11GLXDrawableFactory.java:326)
  at jogamp.opengl.SharedResourceRunner.run(SharedResourceRunner.java:297)
  at java.lang.Thread.run(Unknown Source)
Caused by: com.jogamp.opengl.GLException: glXGetConfig(0x1) failed: error code Unknown error code 6
  at jogamp.opengl.x11.glx.X11GLXGraphicsConfiguration.glXGetConfig(X11GLXGraphicsConfiguration.java:570)
  at jogamp.opengl.x11.glx.X11GLXGraphicsConfiguration.XVisualInfo2GLCapabilities(X11GLXGraphicsConfiguration.java:500)
  at jogamp.opengl.x11.glx.X11GLXGraphicsConfigurationFactory.chooseGraphicsConfigurationXVisual(X11GLXGraphicsConfigurationFactory.java:434)
  at jogamp.opengl.x11.glx.X11GLXGraphicsConfigurationFactory.chooseGraphicsConfigurationStatic(X11GLXGraphicsConfigurationFactory.java:240)
  at jogamp.opengl.x11.glx.X11GLXDrawableFactory.createMutableSurfaceImpl(X11GLXDrawableFactory.java:524)
  at jogamp.opengl.x11.glx.X11GLXDrawableFactory.createDummySurfaceImpl(X11GLXDrawableFactory.java:535)
  at jogamp.opengl.x11.glx.X11GLXDrawableFactory$SharedResourceImplementation.createSharedResource(X11GLXDrawableFactory.java:283)
  ... 2 more
```

Solutions: [mathworks](https://www.mathworks.com/matlabcentral/answers/342906-could-not-initialize-shared-resources-for-x11graphicsdevice)

1. Create a file with the name 'java.opts' in the directory where MATLAB is executed (for me this is in '~/local/MATLAB/R2020a/bin/glnxa64') with the following line: `-Djogl.disable.openglarbcontext=1`.
2. if it doesn't work, add `export MESA_LOADER_DRIVER_OVERRIDE=i965` in bashrc.

# Issue 2:
## "Gtk-Message: 10:32:31.466: Failed to load module "canberra-gtk-module""

For Ubuntu 20.04:
```
sudo apt install libcanberra-gtk-module
```

For Previous version of Ubuntu:
[Follow this answer](https://www.mathworks.com/matlabcentral/answers/472134-gtk-message-10-32-31-466-failed-to-load-module-canberra-gtk-module)

# Issue 3:
## Create desktop icon

1. Easy way, but not pretty:
```
sudo apt install matlab-support
```

2. Create the desktop entry yourself:

In `/usr/share/applications`, create file name as `matlab.desktop` (need root permission)
```
[Desktop Entry]
Version=1.0
Type=Application
Terminal=false
Exec=/path/to/matlab -desktop
Name=MATLAB RXXXXx
Icon=/path/to/icon
Comment=MATLAB
```
