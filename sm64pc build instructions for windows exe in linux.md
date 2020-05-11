# sm64pc

Build instructions for a Windows executable in Ubuntu, using MXE. Tested working with WSL.

## 1. Installing MXE
(a) `git clone https://github.com/mxe/mxe.git`

(b) Install required packages specified in https://mxe.cc/#requirements-debian

(c) `sudo mv mxe /opt/mxe`


## 2. Setting up MXE
(a) `cd /opt/mxe`

(b) `make sdl2 glew glfw3`. This may take a while.

(c) `nano ~/.profile`, add `export PATH=/opt/mxe/usr/bin:$PATH` to the bottom, save and restart.


## 3. Building sm64pc
(a) `git clone https://github.com/sm64pc/sm64pc.git --branch master --single-branch`

(b) `cd sm64pc`, copy `baserom.us.z64` to current directory

(c) Replace all occurences of `-no-pie` in Makefile to `-fno-pie`

(d) Add 
```
#define bzero(b,len) (memset((b), '\0', (len)), (void) 0)  
#define bcopy(b1,b2,len) (memmove((b2), (b1), (len)), (void) 0)
```
at the start of the following files:
   
sm64pc/src/game/envfx_snow.c

sm64pc/src/game/envfx_bubbles.c
   
sm64pc/src/game/save_file.c
   
(e) `make CROSS=i686-w64-mingw32.static- WINDOWS_BUILD=1`
