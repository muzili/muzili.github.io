---
layout: post
category : lessons
tagline: "Supporting tagline"
tags : [intro, beginner, jekyll, tutorial]
---
{% include JB/setup %}

Install valgrind
================

``` {.bash}
tar xvjf ~/Downloads/valgrind-3.9.0.tar.bz2
cd valgrind-3.9.0/
export NDKROOT=/usr/local/android-ndk
export HWKIND=generic
export AR=$NDKROOT/toolchains/arm-linux-androideabi-4.6/prebuilt/linux-x86_64/bin/arm-linux-androideabi-ar
export LD=$NDKROOT/toolchains/arm-linux-androideabi-4.6/prebuilt/linux-x86_64/bin/arm-linux-androideabi-ld
export CC=$NDKROOT/toolchains/arm-linux-androideabi-4.6/prebuilt/linux-x86_64/bin/arm-linux-androideabi-gcc
CPPFLAGS="--sysroot=$NDKROOT/platforms/android-9/arch-arm -DANDROID_HARDWARE_$HWKIND" CFLAGS="--sysroot=$NDKROOT/platforms/android-9/arch-arm" ./configure --prefix=/data/local/Inst --host=armv7-unknown-linux --target=armv7-unknown-linux --with-tmpdir=/sdcard
make -j8
make -j2 install DESTDIR=`pwd`/Inst
adb push Inst /
```

{{{more}}}

Create start~valgrindabv~.sh
============================

``` {.bash}
#!/system/bin/sh
VGPARAMS='--error-limit=no'
export TMDIR=/data/data/tv.ppcam.abviewer
exec /data/local/Inst/bin/valgrind $VGPARAMS $*
```

Run app with valgrind
=====================

Push start~valgrindabv~.sh to *data/local/Inst/bin*
---------------------------------------------------

adb shell setprop wrap.tv.ppcam.abviewer "logwrapper /data/local/Inst/bin/start~valgrindabv~.sh"
------------------------------------------------------------------------------------------------

adb shell am start -a android.intent.action.MAIN -n tv.ppcam.abviewer/.Splash
-----------------------------------------------------------------------------
