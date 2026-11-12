# Old GCC

A fork of [decompals/old-gcc](https://github.com/decompals/old-gcc) whose releases have **native Apple Silicon builds**
of the PS1 GCCs: 2.6.0, 2.6.3, 2.7.2, 2.7.2-cdk, 2.8.0, 2.8.1, 2.91.66 and 2.95.2-psx. It also has fixes for the
arm64 bugs in upstream's macOS builds of 2.7.2 and 2.8.1.

- **Float and double constants:** upstream's 2.7.2-psx and 2.8.1-psx macOS builds emit wrong constants on a 64-bit host.
  `double f(void) { return 1.5; }` at `-O2 -G8` comes out as `.word 0x00000000` for both halves, and a `float`
  constant used inside a function as `0`. These builds emit the right values (`macos/const-double-fix.py`).
- **Crashes:** the 2.91.66 and 2.95.2 builds needed the variadic-argument fixes in `macos/` to run on Apple Silicon at
  all.
- **Identical code otherwise:** the C files of nine public PS1 decomps (4,133 files) compiled with the Intel and the
  arm64 build of each version give identical assembly. 2.7.2-psx gives the same assembly as upstream's macOS build on
  all 3,898 compiling C files of a PS1 game's exported decomp; only float and double constants differ, where
  upstream's are wrong.

PS1 toolchains also need binutils 2.6; a universal (arm64 + x86_64) build is at
[ianderse/mips-binutils-2.6](https://github.com/ianderse/mips-binutils-2.6).

Run `VERSION=2.7.2.2 make` to build an old version of GCC.

Alternatively use one of the prebuilt binaries available from the [Releases](https://github.com/ianderse/old-gcc/releases) page.

**Currently supported versions:**
- GCC 2.5.7
- GCC 2.6.0
- GCC 2.6.3
- GCC 2.7.0
- GCC 2.7.1
- GCC 2.7.2
- GCC 2.7.2.1
- GCC 2.7.2.2
- GCC 2.7.2.3
- GCC 2.8.0
- GCC 2.8.1
- GCC 2.91.60
- GCC 2.91.66
- GCC 2.95.22

**macOS:** `VERSION=2.8.1-psx PLATFORM=macos make` builds natively (x86_64 and arm64). Supported: 2.7.2-psx, 2.7.2-cdk, 2.8.1-psx, 2.6.0-psx, 2.6.3-psx, 2.8.0-psx, 2.91.66-psx and 2.95.2-psx. The last five share `macos/build.sh`; see the comments there and in `macos/` for the arm64 fixes (variadic arguments, `CONST_DOUBLE` copies). `BUILD_ARCH=x86_64` makes an Intel build on an Apple Silicon Mac (it runs under Rosetta).

**NOTE:** All the builds target the `mipsel` architecture. GCC 2.7.0 and above can be passed `-meb` to target big endian at runtime.
