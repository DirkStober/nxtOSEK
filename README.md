# nxtOSEK setup instructions for arch linux
This repository includes the scripts that I used to create a compiler toolchain to flash NXT mindstorms with nxtOSEK. The instructions are based on (http://lejos-osek.sourceforge.net/installation_linux.htm)

## 1. Build and Install GNU ARM 

### Requirements
* gcc 4.4 (other 4.x versions might also work (not 4.3 though))
* texinfo 4.13 (See texinfo section for downgrading)

### Installation
Use the script build_arm_toolchain.sh, which will:
* create SRC, BUILD and GNUARM directories in the current directory
* download sources of binutils, gcc, newlib (check that the links still work!) 
* compile them in place with the right options

### Test

To test the new gcc use the output should show (hard and soft float support):
```console
~$ ./gnuarm/bin/arm-elf-gcc -print-multi-lib
.;
thumb;@mthumb
fpu;@mhard-float
interwork;@mthumb-interwork
fpu/interwork;@mhard-float@mthumb-interwork
thumb/interwork;@mthumb@mthumb-interwork
```

## 2. Set up nxtOSEK


* install wine (in official repo) as sg.exe will nee it
* Download: http://downloads.sourceforge.net/project/lejos-osek/nxtOSEK/nxtOSEK_v213.zip
* Put sg.exe into "/toppers_osek/sg/" 
* Adjust ecrobot/tool_gcc.mak:
	* Change GNUARM_ROOT to point to the gnu arm binary directory
	* Change "-Winline" to "-Wno-inline" in CFLAGS and CXXFLAGS to get rid of inline warnings
* Adjust ecrobot/ecrobot.mak: 
	* Change WINECONSOLE to "wine" instead of "wineconsole" to prevent X window from opening
* Test:
```console
~$ cd ./nxtOSEK/samples_c/test/
~$ make all
```
Should generate a binary executable (helloworld.rxe) and other scripts without error.
 

##3. Firmware Upload	
1. Download libnxt from (https://github.com/rvs/libnxt)
2. You can use scons to build the library but it is easier to use the "Makefile-libnxt) from (http://www.it.uu.se/katalog/marst984/nxt/Makefile-libnxt%20). 
The python file is written in python 2 so change line 9 to run the python file with python2.x (not working with python 3.x).
3. Rename "Makefile-libnxt" to Makefile and run with
```console
~$ make all
```
4. Use fwflash to flash firmware:
	* The firmware "lms_arm_nbcnxc_128.rfw" works but others can be found online
```console
~$ sudo ./fwflash lms_arm_nbcnxc_128.rfw
```

## Program Upload using NeXTTool:
Use the pre-build binary in the installation files, other versions can be found here (http://sourceforge.net/projects/nxceditor/files/nxcEditor/) the tool can also be build from source).
```console
~$ ./nexttool 
nexttool version 1.2 (1.2.1.r5)
     Copyright (c) 2006-2013, John Hansen
Use "nexttool -help" for more information.
```
The tool does not give any error messages, so check carefully that the entered options are correct. To see whether the brick is connected use:
```console
~$ sudo ./nexttool -versions
Protocol version = 1.124
Firmware version = 1.28
```
Check for http://www.krizka.net/2009/12/27/starting-mindstorm-nxt-2-0-development-on-linux/ for more information and how to connect the brick using bluetooth.




## Troubleshooting
### texinfo 4.13
Newer version of texinfo (i.e. those in the official repo) are not compatible and will cause and error. 
If this occurs you will need to use a earlier version of texinfo (4.13 is known to work).
Installation from (https://askubuntu.com/questions/491184/texinfo-downgrade)):
1. Download the texinfo 4.13 source:
```console
~$ wget http://ftp.gnu.org/gnu/texinfo/texinfo-4.13.tar.gz
```
2. Extract the source files:
```console
~$ gzip -dc < texinfo-4.13.tar.gz | tar -xf -
~$ cd texinfo-4.13
```
3. Configure build and install
```console
~$ ./configure
~$ make
~$ sudo make install
```
### visual bug after flashing firmware
Reboot the brick with a quick press on the reset button
