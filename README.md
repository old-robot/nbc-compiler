# NBC/NXC to NXT compiler

From [Bricxcc](https://sourceforge.net/projects/bricxcc/) by John Hansen (see <http://bricxcc.sourceforge.net/nbc/>).
This is based on the latest available release, [1.2.1.r4](https://sourceforge.net/projects/bricxcc/files/NBC_NXC/NBC%20release%201.2.1%20r4/).


## Remove device handling from compiler

NBC defults buildin device handling such as file uploading to the NXT brick. Its relaied on the lego fantom driver on windows and macos.

Unfortunately, the fantom is no longer supported and it cant work on moderm 64bit macos.
So remove it from the compiler and use other tools to handling the device is better.

The change is in the `src/NXT/Makefile`.

```diff
-PFLAGS=-S2cdghi -dRELEASE -vewnhi -l -Fu../commons -Fu. -Fu../bricktools -dCAN_DOWNLOAD
+PFLAGS=-S2cdghi -dRELEASE -vewnhi -l -Fu../commons -Fu.
```

## Compile that in Ubuntu 24.04

+ install dependencies and build

```
sudo apt install -y gcc make fpc
make
```

+ Test by using `./nbc tests/bools.nbc`. The output should look like

```
# Status: NBC compilation begins
# Status: Compiling for firmware version 128, NBC/NXC enhanced = FALSE
# Status: Loading NBC system files
# Status: Running NBC Preprocessor
# Status: Include path = ./;./tests/;/usr/local/include/nbc/
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


## Using it

```
/path/to/nbc /path/to/input.nbc -O=/path/to/output.rxe
```