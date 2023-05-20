# LuaSocket with CMake Support

LuaSocket is a Lua extension library composed of two parts:

- a set of C modules that provide support for the TCP and UDP transport layers, and

- a set of Lua modules that provide functions commonly needed by applications that deal with the Internet.

------

## Building and Installing LuaSocket

### Prerequisites

LuaSocket with CMake Support relies on an installation of [Lua with CMake Support](https://github.com/KritzelKratzel/lua#readme). The same toolchain which has been used with *Lua with CMake Support* is required. LuaSocket gets all necessary path, header and library information from CMake `liblua` package data and will be installed automatically into the right directory locations - no hassle with `package.path` settings anymore.

**Notice:** Present `makefiles` , `*.bat`-files,  `*.cmd`-files and `*.vcxproj`-files originate from original LuaSocket source code and are not used within the context of the CMake-based build infrastructure. Simply ignore them.

### Build and Install

Open `Developer Command Prompt for VS 2022` and change drive and directory. Download and unpack sources or simply clone this repository:

```cmd
c:
cd c:\Temp
git clone https://github.com/KritzelKratzel/luasocket
cd luasocket
```

CMake strongly encourages out-of-source builds.

```cmd
mkdir build
cd build
cmake .. -G "Visual Studio 17 2022" -A <arch>
cmake --build . --config Release
cmake --install . --config Release
```

Replace `<arch>` with your desired architecture. Available architectures with selected `Visual Studio 17 2022` generator are `Win32`, `x64`, `ARM` and `ARM64`. LuaSocket documentation and examples are available in `<lua_install_dir>/share/doc/luasocket` after install.

## Sync this fork with original LuaSocket repository

Open Git Bash and execute `./SyncFork.sh`.

```bash
John Doe@DESKTOP-1HK25HF MINGW64 /c/misc/luasocket (master)
$ ./SyncFork.sh
Original remote repo found.
Already on 'master'
Your branch is up to date with 'origin/master'.
From github.com:KritzelKratzel/luasocket
 * branch            master     -> FETCH_HEAD
Already up to date.
Already up to date.
Everything up-to-date
John Doe@DESKTOP-1HK25HF MINGW64 /c/misc/luasocket (master)
```

