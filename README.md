# DXVK Sarek-Async-Optimized (DXVK-SAO)

A Vulkan 1.1-based translation layer for Direct3D 8/9/10/11 which allows running 3D applications on: 

1. Windows 7/8/8.1/10/11, if GPU has Vulkan driver that is Vulkan 1.1 compliant. Requires SSE2 CPU.
2. Linux using Wine, if GPU has Vulkan driver that is Vulkan 1.1 compliant. Requires SSE2 CPU.
3. MacOS using Wine/CrossOver, if GPU has Vulkan driver that is Vulkan 1.1 compliant. Requires SSE2 CPU.
4. Android using [Winlator](https://github.com/brunodev85/winlator), if GPU has Vulkan driver that is Vulkan 1.1 compliant. CPU Requirements defined by [Winlator](https://github.com/brunodev85/winlator).

## Changes compared to [DXVK-Sarek-Async](https://github.com/pythonlover02/DXVK-Sarek)

1. Provided various GCC (for any OS) builds of DXVK-GPLALL:

   a) optimized for `SSE2` (`-march=x86-64, -mtune=x86-64`) CPUs with Link-Time Optimization (`LTO`, a.k.a. `-flto=auto`) and `-O3` optimization level;

   b) optimized for `SSE4.2` (`-march=x86-64-v2, -mtune=intel`) and newer Intel CPUs with Link-Time Optimization (`LTO`, a.k.a. `-flto=auto`) and `-O3` optimization level.

   c) optimized for `SSE4.2` (`-march=x86-64-v2, -mtune=generic`) and newer CPUs with Link-Time Optimization (`LTO`, a.k.a. `-flto=auto`) and `-O3` optimization level.

Author - [Digger1955](https://github.com/Digger1955/dxvk-sao/releases)

2. Provided various MSVC (requires [MSVCRT](https://www.techpowerup.com/download/visual-c-redistributable-runtime-package-all-in-one/)) builds of DXVK-GPLALL:

   a) optimized for `SSE2` (`/arch:SSE2`) CPUs with Link-Time Optimization (`LTO`, a.k.a. `/LTCG`) and `/O2` optimization level;

   b) optimized for `SSE4.2` (`/arch:SSE4.2`) and newer Intel (`/favor:INTEL64` a.k.a. `/favor:EM64T`) CPUs with Link-Time Optimization (`LTO`, a.k.a. `/LTCG`) and `/O2` optimization level;

   c) optimized for `AVX2` (`/arch:AVX2`) and newer AMD (`/favor:AMD64`) CPUs with Link-Time Optimization (`LTO`, a.k.a. `/LTCG`) and `/O2` optimization level.

Author - [Digger1955](https://github.com/Digger1955/dxvk-sao/releases)

3. Implemented and enabled by defualt `Async` optimization - `m_rtBindingFrameCount >= 2`, which leads to lower peak CPU load during shader compilation process, which leads to less stutters in both amount and longevity. Author - [Digger1955](https://github.com/Digger1955/dxvk-sao/releases), scientific info - [Britt Yazel](https://gitlab.com/Ph42oN/dxvk-gplasync/-/merge_requests/12#note_2632543876)

4. Changed default number of used CPU logical threads from all to `(CPULogicalThreads - 2)`, which leads to less stuttering when compiling shaders while playing, in comparison to using all CPU logical threads. Author - [Digger1955](https://github.com/Digger1955/dxvk-sao/releases)

5. Implemented default location for global DXVK configuration file (`dxvk.conf`) on Windows, Linux and MacOS. Author - [Ph42oN](https://gitlab.com/Ph42oN/dxvk-gplasync/)

Builds Reference Guide provided in [Wiki](https://github.com/Digger1955/dxvk-gplasync-lowlatency/wiki/Builds-Reference-Guide).

## How to use (Windows 7/8/8.1/10/11)

1. Download DXVK-SAO package from [release](https://github.com/Digger1955/dxvk-sao/releases) page.
2. Copy appropriate [DLL dependencies](https://github.com/Digger1955/dxvk-sao/blob/SAO-master-1.11.0/README.md#dll-dependencies) to the location of application's main executable folder.
3. Run application.

**Important**: It is **STRONGLY RECOMMENDED** to create `dxvk.conf` at application's main executable folder (per-application configuration file - first priority) or at `%APPDATA%/dxvk.conf` (one global configuration file - second priority) with your desired DXVK settings.

## How to use (Linux/MacOS)
In order to install a DXVK package obtained from the [release](https://github.com/Digger1955/dxvk-sao/releases) page into a given wine prefix, copy or symlink the DLLs into the following directories as follows, then open `winecfg` and manually add `native` DLL overrides for `d3d8`, `d3d9`, `d3d10core`, `d3d11` and `dxgi` under the Libraries tab.

In a default Wine prefix that would be as follows:
```
export WINEPREFIX=/path/to/wineprefix
cp x64/*.dll $WINEPREFIX/drive_c/windows/system32
cp x32/*.dll $WINEPREFIX/drive_c/windows/syswow64
winecfg
```

For a pure 32-bit Wine prefix (non default) the 32-bit DLLs instead go to the `system32` directory:
```
export WINEPREFIX=/path/to/wineprefix
cp x32/*.dll $WINEPREFIX/drive_c/windows/system32
winecfg
```

Verify that your application uses DXVK instead of wined3d by enabling the HUD (see notes below).

In order to remove DXVK from a prefix, remove the DLLs and DLL overrides, and run `wineboot -u` to restore the original DLL files.

Tools such as Steam Play, Lutris, Bottles, Heroic Launcher, etc will automatically handle setup of dxvk on their own when enabled.

**Important**: It is **STRONGLY RECOMMENDED** to create `dxvk.conf` at application's main executable folder (per-application configuration file - first priority) or at `/home/$USER/.config/dxvk.conf` (one global configuration file - second priority) with your desired DXVK settings.

## DLL dependencies 
Listed below are the DLL requirements for using DXVK with any single API.

- d3d8: `d3d8.dll` and `d3d9.dll`
- d3d9: `d3d9.dll`
- d3d10: `d3d10core.dll`, `d3d11.dll` and `dxgi.dll`
- d3d11: `d3d11.dll` and `dxgi.dll`

## Notes on Vulkan drivers
Before reporting an issue, please check the [Wiki](https://github.com/doitsujin/dxvk/wiki/Driver-support) page on the current driver status and make sure you run a recent enough driver version for your hardware.

## Online multiplayer games
Manipulation of Direct3D libraries in multiplayer games may be considered cheating and can get your account **banned**. This may also apply to singleplayer games with an embedded or dedicated multiplayer portion. 

Async could theoretically trigger client-side anti-cheats, and as such, may be risky to use inside of multiplayer games. There is no information about someone getting banned for using DXVK or DXVK with Async, but - **Use at your own risk.**

## `dxvk.conf` Config File Location

By default, DXVK has [built-in configs](https://github.com/doitsujin/dxvk/blob/master/src/util/config/config.cpp) for specific games and GPU drivers. If user needs to change default DXVK settings, then it can be done by changing default settings in `dxvk.conf` configuration file or by using `DXVK_CONFIG` environment variable.

User can create `dxvk.conf` configuration file at application's main executable folder (per-application configuration file - first priority) or at `%APPDATA%/dxvk.conf` (one global configuration file - second priority) for Windows OS or at `/home/$USER/.config/dxvk.conf` (one global configuration file - second priority) for Linux/MacOS.

User can change default `dxvk.conf` global coniguration file location by specifying path in `DXVK_CONFIG_FILE` environment variable:

- Example (Windows): `DXVK_CONFIG_FILE=%USERPROFILE%/Documents/dxvk.conf`
- Example (Linux): `DXVK_CONFIG_FILE=$XDG_DATA_HOME/dxvk.conf`
- Example (MacOS): `DXVK_CONFIG_FILE=$HOME/Library/dxvk.conf`

User can create `DXVK_CONFIG` to set config variables through the environment instead of a configuration file using the same syntax as in `dxvk.conf`. `;` is used as a seperator.

- Example: `DXVK_CONFIG="dxgi.hideAmdGpu = True; dxgi.syncInterval = 0"`

## HUD
The `DXVK_HUD` environment variable controls a HUD which can display the framerate and some stat counters. It accepts a comma-separated list of the following options:
- `devinfo`: Displays the name of the GPU and the driver version.
- `fps`: Shows the current frame rate.
- `frametimes`: Shows a frame time graph.
- `submissions`: Shows the number of command buffers submitted per frame.
- `drawcalls`: Shows the number of draw calls and render passes per frame.
- `pipelines`: Shows the total number of graphics and compute pipelines.
- `memory`: Shows the amount of device memory allocated and used.
- `gpuload`: Shows estimated GPU load. May be inaccurate.
- `version`: Shows DXVK version.
- `api`: Shows the D3D feature level used by the application.
- `cs`: Shows worker thread statistics.
- `compiler`: Shows shader compiler activity
- `samplers`: Shows the current number of sampler pairs used *[D3D9 Only]*
- `scale=x`: Scales the HUD by a factor of `x` (e.g. `1.5`)
- `opacity=y`: Adjusts the HUD opacity by a factor of `y` (e.g. `0.5`, `1.0` being fully opaque).

Additionally, `DXVK_HUD=1` has the same effect as `DXVK_HUD=devinfo,fps`, and `DXVK_HUD=full` enables all available HUD elements.

### Logs
When used with Wine, DXVK will print log messages to `stderr`. Additionally, standalone log files can optionally be generated by setting the `DXVK_LOG_PATH` variable, where log files in the given directory will be called `app_d3d11.log`, `app_dxgi.log` etc., where `app` is the name of the game executable.

On Windows, log files will be created in the game's working directory by default, which is usually next to the game executable.

## Frame rate limit
The `DXVK_FRAME_RATE` environment variable can be used to limit the frame rate. 

A value of `0` always disables the limiter, i.e. uncaps the frame rate, while any positive value will limit rendering to the given number of frames per second. 

Alternatively, the configuration file can be used.

`DXVK_FRAME_RATE` environment variable represented in `dxvk.conf`:
- For D3D8 and D3D9 - `d3d9.maxFrameRate`. Default value is `d3d9.maxFrameRate = 0`
- For D3D10 and D3D11 - `dxgi.maxFrameRate` . Default value is `dxgi.maxFrameRate = 0`

## Device filter
Some applications do not provide a method to select a different GPU. In that case, DXVK can be forced to use a given device:
- `DXVK_FILTER_DEVICE_NAME="Device Name"` Selects devices with a matching Vulkan device name, which can be retrieved with tools such as `vulkaninfo`. Matches on substrings, so "VEGA" or "AMD RADV VEGA10" is supported if the full device name is "AMD RADV VEGA10 (LLVM 9.0.0)", for example. If the substring matches more than one device, the first device matched will be used.

**Note:** If the device filter is configured incorrectly, it may filter out all devices and applications will be unable to create a D3D device.

## Debugging
The following environment variables can be used for **debugging** purposes.
- `VK_INSTANCE_LAYERS=VK_LAYER_KHRONOS_validation` Enables Vulkan debug layers. Highly recommended for troubleshooting rendering issues and driver crashes. Requires the Vulkan SDK to be installed on the host system.
- `DXVK_LOG_LEVEL=none|error|warn|info|debug` Controls message logging.
- `DXVK_LOG_PATH=/some/directory` Changes path where log files are stored. Set to `none` to disable log file creation entirely, without disabling logging.
- `DXVK_CONFIG_FILE=/xxx/dxvk.conf` Sets path to the configuration file.
- `DXVK_CONFIG="dxgi.hideAmdGpu = True; dxgi.syncInterval = 0"` Can be used to set config variables through the environment instead of a configuration file using the same syntax. ; is used as a seperator.
- `DXVK_PERF_EVENTS=1` Enables use of the VK_EXT_debug_utils extension for translating performance event markers.

## State cache
DXVK caches pipeline state by default, so that shaders can be recompiled ahead of time on subsequent runs of an application, even if the driver's own shader cache got invalidated in the meantime. This cache is enabled by default, and generally reduces stuttering.

The following environment variables can be used to control the cache:

- `DXVK_STATE_CACHE=0` Disables the state cache.
- `DXVK_STATE_CACHE_PATH=/some/directory` Specifies a directory where to put the cache files. Defaults to the current working directory of the application.

## Asynchronous pipeline compilation (Async)

Originally started as hacky solution for shader compilation stutter in dxvk. Similar solution was later added to dxvk itself and promptly removed.

Enabling this solution results in a lot less shader compilation stuttering by not blocking the main thread when compiling async pipelines and (not necessarily) miscellaneous graphical issues while shaders are compiling for the first time.

Asynchronous pipeline compilation is enabled `dxvk.enableAsync = True` in `dxvk.conf`. It is enabled by default.

Asynchronous pipeline compilation is disabled `dxvk.enableAsync = False` in `dxvk.conf`.

## Shader compilation

- `DXVK_ALL_CORES` - Overwrites the default way cores are assigned to compile shaders. By default, DXVK-SAO compiles D3D shaders asynchronously, using `(CPULogicalThreads - 2)` logical CPU threads and leaving the rest free for the game. When `DXVK_ALL_CORES=1` is set, DXVK-SAO uses all available cores for both the game and shader compilation. This might cause the game to become unresponsive at times while compiling shaders. This non-default behavior may improve, worsen, or have no effect on performance, depending on the system.

`DXVK_ALL_CORES` environment variable represented in `dxvk.conf`:
- For Synchronous pipeline compilation - `dxvk.numCompilerThreads`. Default value is `dxvk.numCompilerThreads = 0` - `(CPULogicalThreads - 2)` logical CPU threads.
- For Asynchronous pipeline compilation - `dxvk.numAsyncThreads` . Default value is `dxvk.numAsyncThreads = 0` - `(CPULogicalThreads - 2)` logical CPU threads.

## Build instructions

In order to pull in all submodules that are needed for building, clone the repository using the following command:
```
git clone --recursive https://github.com/Digger1955/dxvk-sao
```

### Requirements:
- [wine 7.1](https://www.winehq.org/) or newer
- [Meson](https://mesonbuild.com/) build system (at least version 0.49)
- [Mingw-w64](https://www.mingw-w64.org) compiler and headers (at least version 10.0)
- [glslang](https://github.com/KhronosGroup/glslang) compiler

### Building DLLs

#### The simple way
Inside the DXVK directory, run:
```
./package-release.sh master /your/target/directory --no-package
```

This will create a folder `dxvk-master` in `/your/target/directory`, which contains both 32-bit and 64-bit versions of DXVK, which can be set up in the same way as the release versions as noted above.

In order to preserve the build directories for development, pass `--dev-build` to the script. This option implies `--no-package`. After making changes to the source code, you can then do the following to rebuild DXVK:
```
# change to build.32 for 32-bit
cd /your/target/directory/build.64
ninja install
```

#### Compiling manually
```
# 64-bit build. For 32-bit builds, replace
# build-win64.txt with build-win32.txt
meson setup --cross-file build-win64.txt --buildtype release --prefix /your/dxvk/directory build.w64
cd build.w64
ninja install
```

The D3D8, D3D9, D3D10, D3D11 and DXGI DLLs will be located in `/your/dxvk/directory/bin`. Setup has to be done manually in this case.

### Build troubleshooting
DXVK requires threading support from your mingw-w64 build environment. If you
are missing this, you may see "error: ‘std::cv_status’ has not been declared"
or similar threading related errors.

On Debian and Ubuntu, this can be resolved by using the posix alternate, which
supports threading. For example, choose the posix alternate from these
commands:
```
update-alternatives --config x86_64-w64-mingw32-gcc
update-alternatives --config x86_64-w64-mingw32-g++
update-alternatives --config i686-w64-mingw32-gcc
update-alternatives --config i686-w64-mingw32-g++
```
For non debian based distros, make sure that your mingw-w64-gcc cross compiler 
does have `--enable-threads=posix` enabled during configure. If your distro does
ship its mingw-w64-gcc binary with `--enable-threads=win32` you might have to
recompile locally or open a bug at your distro's bugtracker to ask for it. 
