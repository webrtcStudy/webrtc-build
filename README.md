# NOTE: This project is no longer maintained

## webrtc-build

Prebuilt (currently `m73`) static [webrtc native](https://webrtc.org/native-code/) libraries + headers, and a Windows batch file to download and build them.

By default the Microsoft Visual C++ compiler is used, and both `debug` and `release` builds are created, to allow debugging with Visual Studio. 

<sup>*Since `H264` can currently not be build using Microsoft Visual C++, it is disabled. If you prefer to use `clang` and include `H264`, just change the `windows_build.bat` file*</sup>

### Building from scratch
Assuming your PC can execute Powershell scripts, just double click on the `windows_build.bat` file. This should create all files in `c:\wc`, and update the `include` and `lib` folders in this cloned repo

If it doesn't work for you, please file an issue.

<sup>*We use `c:\wc` because otherwise paths are becoming too long, and the tools choke on this.*</sup>

### Rationale
Being able to debug the webrtc native code was required for our [WebRTC .NET core](https://github.com/WonderMediaProductions/webrtc-dotnet-core) project



------------------------------------------------------------------------------------
### this project will pull to webrtc-build webrtc dir (yanght)

该工程会下载webrtc脚本到当前目录的webrtc文件夹下

前提：下载depot_tools然后解压到某个目录，depot_tools目录的路径加到系统环境变量Path里，然后把该路径移到最前面（避免已安装的python与git造成影响）。

步骤：
#解决报错
打开Windows PowerShell
#执行
set-executionpolicy remotesigned

#set proxy
set http_proxy=127.0.0.1:1080
set https_proxy=127.0.0.1:1080

#clone build project
git clone https://github.com/webrtcStudy/webrtc-build.git

#build
cd webrtc-build
./windows_build.bat

------------------------------------------------------------------------------------

### 上面的脚本有问题，可能是分支的问题，需要手动编译的话，步骤如下：
set http_proxy=127.0.0.1:1080

set https_proxy=127.0.0.1:1080

mkdir webrtc-checkout

cd webrtc-checkout

fetch --nohooks webrtc

gclient sync

cd src

git checkout master

git pull

gclient sync


set DEPOT_TOOLS_WIN_TOOLCHAIN=0

set GYP_GENERATORS=msvs-ninja,ninja

set GYP_MSVS_VERSION=2017

cd ..

call gn gen ./windows_msvc_debug_x64

::或者call gn gen windows_msvc_release_x64 is_debug=false


ninja -C windows_msvc_debug_x64

::或者ninja -C windows_msvc_release_x64

:: 需要生成头文件等,可以使用以上脚本，可以直接调用脚本（注意.\webrtc斜杠方向）

D:\workspace\webrtc-build\copy_webrtc_API.bat .\webrtc 0

### 新建vs工程
添加依赖项

#pragma comment(lib, "ws2_32.lib")

#pragma comment(lib, "winmm.lib")

#pragma comment(lib, "secur32.lib")

#pragma comment(lib, "iphlpapi.lib")

#pragma comment(lib, "crypt32.lib")

#pragma comment(lib, "dmoguids.lib")

#pragma comment(lib, "wmcodecdspuuid.lib")

#pragma comment(lib, "amstrmid.lib")

#pragma comment(lib, "msdmo.lib")

#pragma comment(lib, "d3d11.lib")

#pragma comment(lib, "dxgi.lib")

添加预处理器定义

NOMINMAX

WIN32_LEAN_AND_MEAN

WEBRTC_WIN

_CRT_SECURE_NO_WARNINGS

WIN32





