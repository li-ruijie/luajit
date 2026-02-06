# LuaJIT Builds

Automated builds of the latest [LuaJIT](https://luajit.org/) rolling release for Windows and Linux.

A GitHub Actions workflow clones the upstream repository daily, checks for new commits, and produces static and dynamic builds for each platform.

## Downloads

Pre-built archives are available on the [Releases](../../releases) page.

| Archive | Platform | Linkage |
|---------|----------|---------|
| `luajit-{version}-linux-x64-static.tar.gz` | Linux x64 | Static |
| `luajit-{version}-linux-x64-dynamic.tar.gz` | Linux x64 | Dynamic |
| `luajit-{version}-windows-msvc-x64-static.zip` | Windows x64 (MSVC) | Static |
| `luajit-{version}-windows-msvc-x64-dynamic.zip` | Windows x64 (MSVC) | Dynamic |
| `luajit-{version}-windows-mingw-x64-static.zip` | Windows x64 (MinGW) | Static |
| `luajit-{version}-windows-mingw-x64-dynamic.zip` | Windows x64 (MinGW) | Dynamic |

Each archive contains the LuaJIT binary, libraries, headers, and the JIT library (`jit/*.lua`).

## Linkage

"Static" and "dynamic" refer to the LuaJIT library itself (`libluajit.a`/`lua51.lib` vs `lua51.dll`). The C runtime is always linked dynamically on Windows so that `ffi.C` can resolve CRT symbols (e.g. `_wstat64`) at runtime.

| Build | LuaJIT library | C runtime |
|-------|----------------|-----------|
| linux-x64-static | `libluajit.a` (static) | glibc (static) |
| linux-x64-dynamic | `libluajit.so` (dynamic) | glibc (dynamic) |
| windows-msvc-x64-static | `lua51.lib` (static) | UCRT dynamic (`/MD`) |
| windows-msvc-x64-dynamic | `lua51.dll` (dynamic) | UCRT dynamic (`/MD`) |
| windows-mingw-x64-static | `libluajit.a` (static) | UCRT dynamic (`-lucrt`) |
| windows-mingw-x64-dynamic | `lua51.dll` (dynamic) | UCRT dynamic (`-lucrt`) |

All Windows builds depend on `api-ms-win-crt-*.dll` (UCRT API sets, included in Windows 10+). MSVC builds also depend on `VCRUNTIME140.dll` (included in the [Visual C++ Redistributable](https://aka.ms/vs/17/release/vc_redist.x64.exe)).
