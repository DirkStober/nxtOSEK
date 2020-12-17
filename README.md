#nxtOSEK setup instructions for arch linux
This repository includes the scripts that I used to create a compiler toolchain to flash NXT mindstorms with nxtOSEK. The instructions are based on (http://lejos-osek.sourceforge.net/installation_linux.htm)

## GNU arm compiler

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

## Troubleshooting
### texinfo 4.13
Newer version of texinfo (i.e. those in the official repo) are not compatible and will cause and error. 
If this occurs (from (https://askubuntu.com/questions/491184/texinfo-downgrade)):
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
