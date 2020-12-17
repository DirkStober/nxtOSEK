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
* Put sg.ext into "/toppers_osek/sg/" 
* Adjust ecrobot/tool_gcc.mak:
	* Change GNUARM_ROOT to point to the gnu arm binary directory
	* Change "-Winline" to "-Wno-inline" in CFLAGS and CXXFLAGS to get rid of inline warnings
* Adjust ecrobot/ecrobot.mak: 
	* Change WINECONSOLE to "wine" instead of "wineconsole" to prevent X window from opening

 











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
