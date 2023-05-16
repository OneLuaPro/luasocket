# LuaSocket

[![Build](https://img.shields.io/github/workflow/status/lunarmodules/luasocket/Build?label=Build=Lua)](https://github.com/lunarmodules/luasocket/actions?workflow=Build)
[![Luacheck](https://img.shields.io/github/workflow/status/lunarmodules/luasocket/Luacheck?label=Luacheck&logo=Lua)](https://github.com/lunarmodules/luasocket/actions?workflow=Luacheck)
[![GitHub tag (latest SemVer)](https://img.shields.io/github/v/tag/lunarmodules/luasocket?label=Tag&logo=GitHub)](https://github.com/lunarmodules/luasocket/releases)
[![Luarocks](https://img.shields.io/luarocks/v/lunarmodules/luasocket?label=Luarocks&logo=Lua)](https://luarocks.org/modules/lunarmodules/luasocket)

LuaSocket is a Lua extension library composed of two parts:

1. a set of C modules that provide support for the TCP and UDP transport layers, and
2. a set of Lua modules that provide functions commonly needed by applications that deal with the Internet.

Original LuaSocket `README.md` ends here.

------

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
