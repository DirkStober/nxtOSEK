#nxtOSEK setup instructions for arch linux
This repository includes the scripts that I used to create a compiler toolchain to flash NXT mindstorms with nxtOSEK. The instructions are based on (http://lejos-osek.sourceforge.net/installation_linux.htm)

## GNU arm compiler

### Requirements
* gcc 4.4 (other 4.x versions might also work (not 4.3 though))
* texinfo 4.13 (See texinfo section for downgrading)

### Installation
Use the script build_arm_toolchain.sh, which will:
* create SRC, BUILD and GNUARM directories in the current directory
* download sources of binutils, gcc, newlib 
* compile them in place with the right options
To test the new gcc use:
```console
$./gnuarm/bin/arm-elf-gcc -print-multi-lib
.;
thumb;@mthumb
fpu;@mhard-float
interwork;@mthumb-interwork
fpu/interwork;@mhard-float@mthumb-interwork
thumb/interwork;@mthumb@mthumb-interwork
```



