# Modern Graphics Library (with SDL) Installation Guide on C in Linux/WSL for windows

Installing required files are suprisingly easy on linux compared to Windows

## Installation

Install gcc/g++ compiler with following command

```
sudo apt install build-essentials
```

Install SDL with:

```
sudo apt-get install libsdl2-2.0-0
```
- or with
```
sudo apt-get install libsdl2-dev
```

- more info on this is [here](https://wiki.libsdl.org/SDL2/Installation) !

Download **SDL_bgi** package depending on linux distro from [here](https://sdl-bgi.sourceforge.io/#download) !

![screenshot showing download links](/screenshots/downloadSDL-bgi.png)

- for me, I am using WSL(Windows Subsystem for Linux) with Ubuntu which is Debian based, so I will use deb package.

**Install(for WSL) if you downloaded in default Downloads folder using**:
```
sudo apt install /mnt/c/Users/<userName>/Downloads/sdl_bgi_3.0.0-1_amd64.deb  
```
- if you downloaded in other place than usual downloads folder then use:
```
sudo apt install /mnt/<pathToPackaged>/sdl_bgi_3.0.0-1_amd64.deb  
```
- or simply copy it to the linux's folder using windows file explorer

- when installing wsl, new directory should be formed in our file explorer called "Linux", simply copy downloaded package to **Linux > linuxName > home > userName > desired folder**

- install with command similar to below, except the path location would be where we put our package at

![screenshot showing download links](/screenshots/downloadSDL-bgi%20for%20wsl.png)

**Installing (for Linux) if you downloaded in default Downloads folder using**:
```
sudo apt install ~/Downloads/sdl_bgi_3.0.0-1_amd64.deb
```
  - if it doesn't work, either the path is incorrect or command input is wrong
  - try to install with your package manager
  - and INTERNET is our best friend as always, if it still doesn't work

With this all required files and libraries should be installed.

### Configure VS Code

Now let's configure vs code to link SDL and SDL_bgi headers and libraries

- first create a graphicsTemplate folder in any location of your choice
- open that folder in vs code 
- create a new file, name is test.c or anything and save in c.
- copy code from "test.c" and paste it in YOUR test.c and save it

- now install this extension: [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) (for intellisense and others)

- then, press "**Ctrl+Shift+P**" to open search box and type "**C/C++**", click on "**C/C++: Edit Configurations(UI)**" not the one with **(JSON)**

![screenshot configuring vs code](/screenshots/configureVSCode.png)

- "**Compiler Path**" should be directed automatically at **gcc** for C codes

![screenshot showing compiler path](/screenshots/configureComipilerPath.png)

- (Optional)
  - "**C standard**" should also be automatically set at "**c17**", or **check your gcc version and set it accordingly** and leave "**C++ standard**" as it is, since we won't be using c++ for this (or change it to "**c++20**" or accordingly) - this is just to use latest compatible C/C++ code standards

![screenshot configuring vs code](/screenshots/configureCStandard.png)

A new folder is automatically created in main folder named "**.vscode**". We will use this folder to use as a template for OTHER graphics C programs without having to set up vs code again and again !!!
- we can simply copy this **.vscode** folder in other folders to run our graphics C Code without the hassle of setting it up next time

![screenshot configuring vs code](/screenshots/templayteFile.png)

- now click/hover over "**Terminal**" then click on "**Configure Tasks...**" (in my case due to screen size being smaller, that menu was hidden behind dots)

![screenshot configuring vs code](/screenshots/configureBuild.png)

- all avaiable c compiler options should come up (in my case only GCC is present)
![screenshot configuring vs code](/screenshots/configureCompiler.png)
  
  - the "Build with GCC ..." is the same gcc compiler, I just configured it early

- click on either one of those (while c code tab is active), it creates and opens "tasks.json" inside of that ".vscode" folder. This is where we can properly link libraries to compile our graphics program in C code
  - gcc-11 and gcc is same, in my case, my current installed gcc version is **11.4.0**, yours might be version 12 or 13(latest)

- if you want to use another compiler for eg, Clang, simply follow previous steps and click on Clang

- next, inside "tasks.json" add these lines inside "**args**":

```
  "-lSDL_bgi",
  "-lSDL2"
```

- Don't forget to add commas

![screenshot showing sdl linkers](/screenshots/configureLinkers.png)

These linkers are provided in [SDL_bgi](https://sdl-bgi.sourceforge.io/) !!!

#### Some more vs code configuration

modify the line:
```
"${fileDirname}/${fileBasenameNoExtension}",
//to
"./bin/${fileBasenameNoExtension}",
```

- this keeps all the compiled files in "bin" folder when C code is compiled without issues. Just make sure said "bin" folder is present in the main folder

we can also modify "**Label**" of these compilers by changing in "label":

![screenshot of label](/screenshots/configureLabel.png)

- here I changed it to "**Build using GCC 11.4.0**" as I am using GCC compiler of version **11.4.0**,  this is really helpful if we have multiple different compilers or same compilers with different versions

### Compiling

- to compile our C code, simply press "**Ctrl+Shift+B**"
  - **if** you have configured multiple compilers, we can choose one of them, **else** it will start compiling without having to choose any
- it should say "build completed" in vscode's terminal, simply press any key to close it, then run the file

- if you want to compile C code in typical terminal fashion, use this command:

```
gcc -o test test.c -lSDL_bgi -lSDL2
```
- just make sure your terminal is pointed at that folder

more on these at their [docs](https://sdl-bgi.sourceforge.io/) as usual

![screenshot of output](/screenshots/output.png)

And we are done YAY !!!

- if the window resolution is small, check out SDL_bgi docs [here](https://sdl-bgi.sourceforge.io/using.html) !!! on how to use different available resolutions

#### Error Solving

- if it results in lSDL... not found, try reinstalling SDL/SDL2 again

- if it says bin not found, add correct bin folder path or create bin folder

-  if it results in other errors, Internet is your friend

- LASTLY, check the documentations of both [SDL](https://www.libsdl.org/) and [SDL_bgi](https://sdl-bgi.sourceforge.io/using.html/) libraries for more info on how these works.

### Important !!!

- Make sure to include "**graphics.h**" header in your C program

- Make sure that your **main()** function has following parameters: **(int argc, char *argv[])**

  ```
  int main(int argc, char *argv[]){
    /* Code Segments */
  }
  ```
  
![screenshot configuring vs code](/screenshots/important.png)

- if not, it will conflict with main() function included in SDL librairies. Check their documentations for more, [AGAIN](https://wiki.libsdl.org/SDL2/FrontPage)!!!


### Why do all these instead of using graphics.h header file ?

- Simply because graphics.h is old and not updated for modern systems.
- Also BGI was for old DOS era systems.

### Why use graphics.h at all then ?

- Well for college lessons, not more not less. It introduces us to computer graphics in C atleast.

AND HAPPY CODING !!!

Also, check my "How to configure modern graphics in C for windows", [here](https://github.com/DadeGamerNoob/graphicsInstallationInC)!!!