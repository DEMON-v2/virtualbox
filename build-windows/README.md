# Build VirtualBox in Windows

아래는 Windows 환경에서 VirtualBox를 빌드하는 과정에 대해 설명하고 있습니다.

Windows 환경에서 빌드하고자 한다면 아래 빌드 과정을 참고해주시길 바랍니다.

## Warning

패키지 설치 시에는 `Custom path`가 아닌 `Default path`를 권장합니다.

빌드 진행 중 환경 변수(Environment Variable) 설정 시 경로 문제가 발생할 수 있습니다.

또한, 윈도우 최초 설치 후 진행되는 기본 업데이트(.NET Framework 설치 포함)를 진행하셔야

원활한 설치가 가능합니다.

## Requirements

- Windows 10 21H2 Build (>= Windows 10 1809)

## Building

### 1. Set Environment

- [.NET Framework](#) >= 3.5 required

- [Visual Studio 2010 Professional](https://www.freesoftwarefiles.com/development/microsoft-visual-studio-2010-professional-free-download)

- [Visual Studio 2010 SP1](https://my.visualstudio.com/Downloads?q=visual%20studio%202010%20service%20pack%201)

- [WinSDK 7.1](https://www.microsoft.com/en-us/download/details.aspx?id=8279)

- [WinSDK 8.1](https://developer.microsoft.com/ko-kr/windows/downloads/sdk-archive/)

- [WinDDK 7.1](https://www.microsoft.com/en-us/download/details.aspx?displaylang=en&id=11800)

- [Visual C++ 2010 SP1 Compiler Update for SDK 7.1](https://www.microsoft.com/en-us/download/details.aspx?id=4422)

- [OpenSSL x86/x64](http://slproweb.com/products/Win32OpenSSL.html)

- [CURL](https://curl.se/download/curl-7.77.0.zip)

- [MinGW x64](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/rubenvb/gcc-4.5-release/x86_64-w64-mingw32-gcc-4.5.4-release-win64_rubenvb.7z/download)

- [SDL](http://www.libsdl.org/release/SDL-devel-1.2.15-VC.zip) >= 1.2.15 required

- [QT](http://download.qt.io/new_archive/qt/5.6/5.6.3/single/qt-everywhere-opensource-src-5.6.3.zip) >= 5.6.3 required

### 2. OpenSSL Installation

- Run `Win32OpenSSL.exe` and `Win64OpenSSL.exe`

- OpenSSL (x86) `C:\Program Files (x86)\OpenSSL-Win32`

- OpenSSL (x64) `C:\Program Files\OpenSSL-Win64

- Copy Folder `openssl-x86` and `openssl-x64` to `C:\Build\SSL\<copy-here>

### 3. SDL Installation

- Create Folder `C:\Build\SDL`

- Extract to `C:\Build\SDL\<extract-here>

- Copy Folder `include` to `C:\Build\SDL\<copy-here>

- Copy Folder `lib/x64` to `C:\Build\SDL\lib

### 4. MinGW Installation

- Create Folder `C:\Build\MinGW`

- Extract to `C:\Build\MinGW\<extract-here>

### 5. CURL(x86) Build

- Create folder `C:\Build\curl`

- Extract to `C:\Build\curl\<extract-here>`

- Create folder `curl-x32`

- Create folder `curl-x32/include/curl`

- Build CURL (Following Commands from a cmd prompt)

```bash
> cd c:\Build\curl\curl-7.77.0\winbuild
> nmake /f Makefile.vc mode=dll WITH_SSL=static DEBUG=no MACHINE=x86 SSL_PATH=C:\Build\SSL\OpenSSL-Win32 ENABLE_SSPI=no ENABLE_WINSSL=no ENABLE_IDN=no
```

- Move CURL(x86) Files

```bash
> copy ..\builds\libcurl-vc-x86-release-dll-ssl-static-ipv6\lib\libcurl.lib ..\..\curl-x32\libcurl.lib
> copy ..\builds\libcurl-vc-x86-release-dll-ssl-static-ipv6\bin\libcurl.dll ..\..\curl-x32\libcurl.dll
> copy ..\builds\libcurl-vc-x86-release-dll-ssl-static-ipv6\include\curl ..\..\curl-x32\include\curl\
```

### 6. CURL(x64) Build

- Create folder `curl-x64`

- Create folder `curl-x64/include/curl`

- Build CURL (Following Commands from a cmd prompt)

```bash
> cd c:\Build\curl\winbuild
> nmake /f Makefile.vc mode=dll WITH_SSL=static DEBUG=no MACHINE=x64 SSL_PATH=C:\Build\SSL\OpenSSL-Win64 ENABLE_SSPI=no ENABLE_WINSSL=no ENABLE_IDN=no
```

- Move CURL(x64) Files

```bash
> copy ..\builds\libcurl-vc-x64-release-dll-ssl-static-ipv6\lib\libcurl.lib ..\..\curl-x64\libcurl.lib
> copy ..\builds\libcurl-vc-x64-release-dll-ssl-static-ipv6\bin\libcurl.dll ..\..\curl-x64\libcurl.dll
> copy ..\builds\libcurl-vc-x64-release-dll-ssl-static-ipv6\include\curl ..\..\curl-x64\include\curl\
```

### 7. Qt Creator Build

- Create folder `C:\Build\Qt`

- Extract to `C:\Build\Qt\<extract-here>`

- Create folder `qt-x64`

- Build Qt Creator (Following Commands from a cmd prompt)

```bash
> configure.bat -opensource -confirm-license -nomake tests -nomake examples -no-compile-examples -release -shared -no-ltcg -accessibility -no-sql-sqlite -opengl desktop -no-openvg -no-nis -no-iconv -no-evdev -no-mtdev -no-inotify -no-eventfd -largefile -no-system-proxies -qt-zlib -qt-pcre -no-icu -qt-libpng -qt-libjpeg -qt-freetype -no-fontconfig -qt-harfbuzz -no-angle -incredibuild-xge -no-plugin-manifests -qmake -qreal double -rtti -strip -no-ssl -no-openssl -no-libproxy -no-dbus -no-audio-backend -no-wmf-backend -no-qml-debug -no-direct2d -directwrite -no-style-fusion -native-gestures -skip qt3d -skip qtactiveqt -skip qtandroidextras -skip qtcanvas3d -skip qtconnectivity -skip qtdeclarative -skip qtdoc -skip qtenginio -skip qtgraphicaleffects -skip qtlocation -skip qtmacextras -skip qtmultimedia -skip qtquickcontrols -skip qtquickcontrols2 -skip qtscript -skip qtsensors -skip qtserialbus -skip qtserialport -skip qtwayland -skip qtwebchannel -skip qtwebengine -skip qtwebsockets -skip qtwebview -skip qtx11extras -skip qtxmlpatterns -prefix C:\Build\Qt\qt-x64

> nmake
> nmake install
```

### 8. Enabled TestMode & Signing

```bash
> bcdedit.exe -set loadoptions DDISABLE_INTEGRITY_CHECKS
> bcdedit.exe -set TESTSIGNING ON
```

### 9. VirtualBox Source Code Download & Certificates

[VirtualBox-Source-latest](https://download.virtualbox.org/virtualbox/6.1.22/VirtualBox-6.1.22.tar.bz2)

- Extract to `C:\Build\<extract-here>`

- Rename folder `VirtualBox`

- Add certificates (Following Commands from a cmd prompt)

```bash
> set PATH=%PATH%;c:\WinDDK\7600.16385.1\bin\amd64;
> makecert.exe -r -pe -ss my -eku 1.3.6.1.5.5.7.3.3 -n "CN=MyTestCertificate" mytestcert.cer
> certmgr.exe -add mytestcert.cer -s -r localMachine root
> certmgr.exe -add mytestcert.cer -s -r localMachine trustedpublisher
```

### 10. Build VirtualBox

- Create File `C:\Build\VirtualBox\LocalConfig.kmk`

```bash
VBOX_WITHOUT_HARDENING=1

VBOX_WITH_ADDITIONS :=
VBOX_WITH_ADDITIONS_PACKING := 1

VBOX_PATH_SIGN_TOOLS=C:\WinDDK\7600.16385.1\bin\amd64
VBOX_INF2CAT=C:\WinDDK\7600.16385.1\bin\selfsign\Inf2Cat.exe
VBOX_SIGNING_MODE=test
```

```bash
> cd c:\Build\VirtualBox
> set BUILD_TARGET_ARCH=amd64
> cscript configure.vbs --with-vc="C:\Program Files (x86)\Microsoft Visual Studio 10.0" --with-qt5="C:\Build\qt\qt-x64" --with-DDK="C:\WinDDK\7600.16385.1\bin\amd64" --with-MinGW-w64="C:\Build\MinGW\mingw64" --with-libSDL="C:\Build\SDL" --with-openssl="C:\Build\ssl\OpenSSL-Win64" --with-openssl32="C:\Build\ssl\OpenSSL-Win32" --with-libcurl="C:\Build\curl\curl-x64" --with-libcurl32="C:\Build\curl\curl-x32"
> env.bat
> kmk
```

### 11. Set VirtualBox

```bash
> cd c:\Build\VirtualBox\out\win.amd64\release\bin
> SET PATH=%PATH%;c:\Build\curl\curl-x64;
> SET PATH=%PATH%;c:\Build\curl\curl-x32;
> comregister.cmd
> loadall.cmd
```

### 12. Run VirtualBox

```bash
> SET PATH=%PATH%;c:\Build\qt\qt-x64\bin
> SET PATH=%PATH%;c:\Build\curl\curl-x64
> C:\Build\VirtualBox\out\win.amd64\release\bin\VirtualBox.exe
```

### References

- [VirtualBoBs-Build-Windows](https://github.com/VirtualBoBs/build-virtualbox-in-windows)