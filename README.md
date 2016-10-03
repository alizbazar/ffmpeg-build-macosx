build-ffmpeg-mac
==========

This is a forked/modified script from the excellent original at https://github.com/markus-perl/ffmpeg-build-script

The original build script provides an easy way to build ffmpeg on OSX and Linux with non-free codecs included. I added:
* the frei0r plugins (for doing fisheye removal on cameras like GoPro, Mobius, etc.)
* Freetype (for doing text overlays)
* fontconfig (plus dependency libxml2) 

This version also adds support for [avfoundation](https://www.ffmpeg.org/ffmpeg-devices.html#avfoundation) and open-ssl.


Requirements OSX
------------

* XCode 5.x or greater with command line tools installed. Use either the Xcode UI, or from the terminal:

```xcode-select --install```

Requirements for Linux
------------

* build-essentials installed


Usage
------

```
./build-ffmpeg --help       Display usage information
./build-ffmpeg --build      Starts the build process
./build-ffmpeg --cleanup    Remove all working dirs
```

Contact
-------

You can file an issue on this repo; I may be able to help with simple issues, but certainly the expert is the original creator:

* Github: [http://www.github.com/markus-perl](http://www.github.com/markus-perl)
* E-Mail: markus (at) www-factory.de


Tested on
---------

* Mac OSX 10.10, El Capitan Beta 2/3
* Debian 7.5

Example fisheye removal for GoPro/Mobius
-------
* For Mobius lens "C": 

```ffmpeg -i <inputfile> -vf frei0r=defish0r:0.8:y:0.6:0 -vcodec libx264 -pix_fmt yuv420p -acodec copy <outputfile.mp4>```

![Example of cleaned up image](https://github.com/rtgoodwin/ffmpeg-build-script-plus-frei0r-freetype/blob/master/compare.png)

and another

![Example of cleaned up image](https://github.com/rtgoodwin/ffmpeg-build-script-plus-frei0r-freetype/blob/master/compare2.png)

(Blur at edges is in the original video; keep those lenses clean, people! :) )

* For GoPro (or other cameras with less barrel distortion):

```ffmpeg -i <inputfile> -vf frei0r=defish0r:0.75:y:0.6:0 -vcodec libx264 -pix_fmt yuv420p -acodec copy <outputfile.mp4>```



Example compilation output
-------

```
user@localhost:~/dev/mac/ffmpeg-build-script$ ./build-ffmpeg --build

building yasm
=======================
Downloading http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz ... Done
$ ./configure --prefix=~/dev/mac/ffmpeg/workspace
$ make -j 4
$ make install

building opencore
=======================
Downloading http://garr.dl.sourceforge.net/project/opencore-amr/opencore-amr/opencore-amr-0.1.3.tar.gz ... Done
$ ./configure --prefix=~/dev/mac/ffmpeg/workspace --disable-shared --enable-static
$ make -j 4
$ make install

building libvpx
=======================
Downloading http://webm.googlecode.com/files/libvpx-v1.3.0.tar.bz2 ... Done
$ ./configure --prefix=~/dev/mac/ffmpeg/workspace --disable-unit-tests --disable-shared
$ make -j 4
$ make install

building lame
=======================
Downloading http://kent.dl.sourceforge.net/project/lame/lame/3.99/lame-3.99.5.tar.gz ... Done
$ ./configure --prefix=~/dev/mac/ffmpeg/workspace --disable-shared --enable-static
$ make -j 4
$ make install

building xvidcore
=======================
Downloading http://downloads.xvid.org/downloads/xvidcore-1.3.3.tar.gz ... Done
$ ./configure --prefix=~/dev/mac/ffmpeg/workspace --disable-shared --enable-static
$ make -j 4
$ make install
$ rm ~/dev/mac/ffmpeg/workspace/lib/libxvidcore.4.dylib

building x264
=======================
Downloading ftp://ftp.videolan.org/pub/x264/snapshots/last_x264.tar.bz2 ... Done
$ ./configure --prefix=~/dev/mac/ffmpeg/workspace --disable-shared --enable-static
$ make -j 4
$ make install
$ make install-lib-static

building libogg
=======================
Downloading http://downloads.xiph.org/releases/ogg/libogg-1.3.2.tar.gz ... Done
$ ./configure --prefix=~/dev/mac/ffmpeg/workspace --disable-shared --enable-static
$ make -j 4
$ make install

building libvorbis
=======================
Downloading http://downloads.xiph.org/releases/vorbis/libvorbis-1.3.4.tar.gz ... Done
$ ./configure --prefix=~/dev/mac/ffmpeg/workspace --with-ogg-libraries=~/dev/mac/ffmpeg/workspace/lib --with-ogg-includes=/VoluPACKAGES/Ramdisk/sw/include/ --enable-static --disable-shared
$ make -j 4
$ make install

building libtheora
=======================
Downloading http://downloads.xiph.org/releases/theora/libtheora-1.1.1.tar.gz ... Done
$ ./configure --prefix=~/dev/mac/ffmpeg/workspace --with-ogg-libraries=~/dev/mac/ffmpeg/workspace/lib --with-ogg-includes=~/dev/mac/ffmpeg/workspace/include/ --with-vorbis-libraries=~/dev/mac/ffmpeg/workspace/lib --with-vorbis-includes=~/dev/mac/ffmpeg/workspace/include/ --enable-static --disable-shared --disable-oggtest --disable-vorbistest --disable-examples --disable-asm
$ make -j 4
$ make install

building pkg-config
=======================
Downloading http://pkgconfig.freedesktop.org/releases/pkg-config-0.28.tar.gz ... Done
$ ./configure --silent --prefix=~/dev/mac/ffmpeg/workspace --with-pc-path=~/dev/mac/ffmpeg/workspace/lib/pkgconfig --with-internal-glib
$ make -j 4
$ make install

building cmake
=======================
Downloading http://www.cmake.org/files/v3.0/cmake-3.0.2.tar.gz ... Done
$ ./configure --prefix=~/dev/mac/ffmpeg/workspace
$ make -j 4
$ make install

building vid_stab
=======================
Downloading https://codeload.github.com/georgmartius/vid.stab/legacy.tar.gz/release-0.98b ... Done
$ cmake -DCMAKE_INSTALL_PREFIX:PATH=~/dev/mac/ffmpeg/workspace .
$ make -s install

building x265
=======================
Downloading https://bitbucket.org/multicoreware/x265/get/1.3.tar.gz ... Done
$ cmake -DCMAKE_INSTALL_PREFIX:PATH=~/dev/mac/ffmpeg/workspace -DENABLE_SHARED=NO .
$ make -j 4
$ make install

building fdk_aac
=======================
Downloading http://netcologne.dl.sourceforge.net/project/opencore-amr/fdk-aac/fdk-aac-0.1.3.tar.gz ... Done
$ ./configure --prefix=~/dev/mac/ffmpeg/workspace --disable-shared --enable-static
$ make -j 4
$ make install

building ffmpeg
=======================
Downloading http://ffmpeg.org/releases/ffmpeg-2.4.2.tar.bz2 ... Done
$ ./configure --arch=64 --prefix=~/dev/mac/ffmpeg/workspace --extra-cflags=-I~/dev/mac/ffmpeg/workspace/include --extra-ldflags=-L~/dev/mac/ffmpeg/workspace/lib --extra-version=static --disable-debug --disable-shared --enable-static --extra-cflags=--static --disable-ffplay --disable-ffserver --disable-doc --enable-version3 --enable-libvpx --enable-libmp3lame --enable-libtheora --enable-libvorbis --enable-libx264 --enable-avfilter --enable-gpl --enable-libopencore_amrwb --enable-libopencore_amrnb --enable-nonfree --enable-filters --enable-libvidstab --enable-libx265 --enable-runtime-cpudetect --enable-libfdk-aac
$ make -j 4
$ make install

Building done. The binary can be found here: ~/dev/mac/ffmpeg/workspace/bin/sw/ffmpeg

Install the binary to your /usr/local/bin/folder? [Y/n]
```
