# LuaSocket with CMake Support

[![Build](https://img.shields.io/github/workflow/status/lunarmodules/luasocket/Build?label=Build=Lua)](https://github.com/lunarmodules/luasocket/actions?workflow=Build)
[![Luacheck](https://img.shields.io/github/workflow/status/lunarmodules/luasocket/Luacheck?label=Luacheck&logo=Lua)](https://github.com/lunarmodules/luasocket/actions?workflow=Luacheck)
[![GitHub tag (latest SemVer)](https://img.shields.io/github/v/tag/lunarmodules/luasocket?label=Tag&logo=GitHub)](https://github.com/lunarmodules/luasocket/releases)
[![Luarocks](https://img.shields.io/luarocks/v/lunarmodules/luasocket?label=Luarocks&logo=Lua)](https://luarocks.org/modules/lunarmodules/luasocket)

LuaSocket is a Lua extension library composed of two parts:

1. a set of C modules that provide support for the TCP and UDP transport layers, and
2. a set of Lua modules that provide functions commonly needed by applications that deal with the Internet.

Original LuaSocket `README.md` ends here.

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

## Fetch Changes form original LuaSocket Repository

These are the command to incorporate code base changes form original LuaSocket repository.

1. Add original and remote repository to local clone. This step is only required once.

```cmd
C:\misc\luasocket>git remote add luasocket https://github.com/lunarmodules/luasocket.git
C:\misc\luasocket>git remote -v
luasocket       https://github.com/lunarmodules/luasocket.git (fetch)
luasocket       https://github.com/lunarmodules/luasocket.git (push)
origin  git@github.com:KritzelKratzel/luasocket.git (fetch)
origin  git@github.com:KritzelKratzel/luasocket.git (push)
C:\misc\luasocket>
```

2. Command sequence for fetching changes on original repository on [Lunar Modules](https://github.com/lunarmodules):

```cmd
git checkout master
git pull origin master
git merge luasocket/master
git push origin master
```

