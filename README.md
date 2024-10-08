# NBC/NXC to NXT compiler

From [Bricxcc](https://sourceforge.net/projects/bricxcc/) by John Hansen (see <http://bricxcc.sourceforge.net/nbc/>).
This is based on the latest available release, [1.2.1.r4](https://sourceforge.net/projects/bricxcc/files/NBC_NXC/NBC%20release%201.2.1%20r4/).


## Remove device handling from compiler

NBC defults buildin device handling such as file uploading to the NXT brick. Its relaied on the lego fantom driver on windows and macos.
Unfortunately, the fantom is no longer supported and it cant work on moderm 64bit macos.
So remove it from the compiler and use other tools to handling the device is better.

The changes are in the `src/NXT/Makefile`.

```diff
-PFLAGS=-S2cdghi -dRELEASE -vewnhi -l -Fu../commons -Fu. -Fu../bricktools -dCAN_DOWNLOAD
+PFLAGS=-S2cdghi -dRELEASE -vewnhi -l -Fu../commons -Fu.
```

## Compile that in Ubuntu 24.04

+ install dependencies and build

```
sudo apt install -y gcc make fpc libusb-dev
make
```

+ Test that everything goes well, by using `./nbc tests/bools.nbc`. The output should look like

```
# Status: NBC compilation begins
# Status: Compiling for firmware version 128, NBC/NXC enhanced = FALSE
# Status: Loading NBC system files
# Status: Running NBC Preprocessor
# Status: Include path = /home/pierre/code/nbc-compiler/;/usr/local/include/nbc/;tests/
# Status: Processing include: NBCCommon.h
# Status: Compiling NBC source code
# Status: Finished compiling NBC source code
# Status: Finalizing dependencies
# Status: Optimizing at level 1
# Status: Build codespace references
# Status: Optimize mutexes
# Status: Compact the codespace
# Status: Remove unused labels
# Status: Compact the dataspace
# Status: Sort the dataspace
# Status: Generate raw dataspace data
# Status: Fill clump and codespace arrays
# Status: Update executable file header
# Status: Write file header to executable
# Status: Write dataspace to executable
# Status: Write clump data to executable
# Status: Write code to executable
# Status: Write optimized source to compiler output
# Status: Finished
```
+ If so, you are good :)


## Using it

There is some help:

```
$ ./nbc -help
Next Byte Codes Compiler version 1.2 (1.2.1.r4, built Sat 04 Apr 2020 12:07:06 PM CEST)
     Copyright (c) 2006-2010, John Hansen
Syntax: nbc [options] filename [options]

   -S=<portname>: specify port name (usb), brick resource name, or alias
   -d: download program
   -r: download and run program
   -b: treat input file as a binary file (don't compile it)
   -q: quiet
   -n: prevent the system file from being included
   -D=<sym>[=<value>]: define macro <sym>
   -x: decompile program
   -Z[1|2]: turn on compiler optimizations
   -ER=n: set maximum errors before aborting (0 == no limit)
   -PD=n: set max preprocessor recursion depth (default == 10)
   -O=<outfile> : specify output file
   -E=<filename> : write compiler messages to <filename>
   -I=<path>: search <path> for include files
   -nbc=<filename> : save NXC intermediate NBC code to <filename>
   -L=<filename> : generate code listing to <filename>
   -Y=<filename> : generate symbol table to <filename>
   -w[-|+] : warnings off or on (default is on)
   -sm[-|+] : status messages off or on (default is on)
   -EF : enhanced firmware
   -safecall: NXC will wrap all function calls in Acquire/Release
   -api: dump the API to stdout
   -v=n: set the targeted firmware version (default == 128, NXT 1.1 == 105)
   -help : display command line options
```

... But basically, the only thing you need is

```
/path/to/nbc /path/to/input.nbc -O=/path/to/output.rxe
```