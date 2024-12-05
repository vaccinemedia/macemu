#### BasiliskII
```
macOS     x86_64 JIT / arm64 non-JIT
Linux x86 x86_64 JIT / arm64 non-JIT
MinGW x86        JIT
```
#### SheepShaver
```
macOS     x86_64 JIT / arm64 non-JIT
Linux x86 x86_64 JIT / arm64 non-JIT
MinGW x86        JIT
```
### How To Build
These builds need to be installed SDL2.0.14+ framework/library.

https://www.libsdl.org
#### BasiliskII
##### macOS
preparation:

Download gmp-6.2.1.tar.xz from https://gmplib.org.
```
$ cd ~/Downloads
$ tar xf gmp-6.2.1.tar.xz
$ cd gmp-6.2.1
$ ./configure --disable-shared
$ make
$ make check
$ sudo make install
```
Download mpfr-4.2.0.tar.xz from https://www.mpfr.org.
```
$ cd ~/Downloads
$ tar xf mpfr-4.2.0.tar.xz
$ cd mpfr-4.2.0
$ ./configure --disable-shared
$ make
$ make check
$ sudo make install
```
On an Intel Mac, the libraries should be cross-built.  
Change the `configure` command for both GMP and MPFR as follows, and ignore the `make check` command:
```
$ CFLAGS="-arch arm64" CXXFLAGS="$CFLAGS" ./configure -host=aarch64-apple-darwin --disable-shared 
```
(from https://github.com/kanjitalk755/macemu/pull/96)

about changing Deployment Target:  
If you build with an older version of Xcode, you can change Deployment Target to the minimum it supports or 10.7, whichever is greater.

build:
```
$ cd macemu/BasiliskII/src/MacOSX
$ xcodebuild build -project BasiliskII.xcodeproj -configuration Release
```
or same as Linux

##### Linux
preparation (arm64 only): Install GMP and MPFR.
```
$ cd macemu/BasiliskII/src/Unix
$ ./autogen.sh
$ make
```
##### MinGW32/MSYS2
preparation:
```
$ pacman -S base-devel mingw-w64-i686-toolchain autoconf automake mingw-w64-i686-SDL2 mingw-w64-i686-gtk2
```
build (from a mingw32.exe prompt):
```
$ cd macemu/BasiliskII/src/Windows
$ ../Unix/autogen.sh
$ make
```
#### SheepShaver
##### macOS
about changing Deployment Target: see BasiliskII
```
$ cd macemu/SheepShaver/src/MacOSX
$ xcodebuild build -project SheepShaver_Xcode8.xcodeproj -configuration Release
```
or same as Linux

##### Linux
```
$ cd macemu/SheepShaver/src/Unix
$ ./autogen.sh
$ make
```

##### Raspberry Pi OS
preparation:
```
$ sudo apt install libsdl2-dev libgtk2.0-dev
```
Then:
```
$ cd macemu/SheepShaver
$ make links
$ cd SheepShaver/src/Unix
$ ./autogen.sh --enable-addressing=direct,0x10000000
$ make
$ sudo make install
```
Then make a folder in your home folder (or folder where you want SheepShaver to reside) and copy the compiled SheepShaver binary into this folder
Add the Mac OS ROM file in there and the MacOS 9.0.4 installer iso in there too
Run sheepshaver by double clicking on it in the file explorer (no need to use the command line or use sudo) and create a hard disk file 2000 in size called OS9 or something similar and also add the installer iso. Make sure you edit permissions on the iso by right clicking on it in the file explorer and edit permissions and set edit permissons to "nobody" to make it non-writeable or else SheepShaver will mistake the iso CD file as a hard disk file and not a CDROM. Also make sure the boot order is the HD file first, then the CDROM iso file. Set boot from to "any".

Make a folder in your SheepShaver folder called "Shared" and set this as the "Unix Root" in the settings.

Then set the graphics to 60hz refresh and 1024x768 resolution (you can change these later on after installation has complete). In the network tab select slirp as your network interface and in the ROM settings select the ROM file in the SheepShaver folder you copied in earlier.

Click start and it should boot from the CDROM iso and then insialise the hard drive as Mac OS Extended and install Mac OS using the installer. Once this is complete make sure you then run the OS 9.0.4 updater to complete the full installation.

When rebooting for the first time, during setup it may hang on detecting network devices. If it does this, reboot your Pi and next time you start it should be OK.

##### MinGW32/MSYS2
preparation: same as BasiliskII  
  
build (from a mingw32.exe prompt):
```
$ cd macemu/SheepShaver
$ make links
$ cd src/Windows
$ ../Unix/autogen.sh
$ make
```
### Recommended key bindings for gnome
https://github.com/kanjitalk755/macemu/blob/master/SheepShaver/doc/Linux/gnome_keybindings.txt

(from https://github.com/kanjitalk755/macemu/issues/59)
