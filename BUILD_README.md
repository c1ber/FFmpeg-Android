## Build instructions ##

* Download the [Android NDK](https://developer.android.com/tools/sdk/ndk/index.html) and set its location in an environment variable:

```
NDK_PATH="<path to Android NDK>"
```

* Set up host platform ("darwin-x86_64" for Mac OS X):

```
HOST_PLATFORM="linux-x86_64"
```

* Fetch and build FFmpeg. The configuration flags determine which formats will
  be supported.
  
To fetch and build for armeabi, armeabi-v7a,
  arm64-v8a, x86 and x86_64 on Linux x86_64:

```
COMMON_OPTIONS="\
    --target-os=android \
    --disable-static \
    --enable-shared \
    --disable-doc \
    --disable-programs \
    --disable-everything \
    --disable-gpl \
    --disable-nonfree \
    --disable-avdevice \
    --disable-avformat \
    --disable-swscale \
    --disable-postproc \
    --disable-avfilter \
    --disable-symver \
    --disable-swresample \
    --enable-avresample \
    --enable-decoder=vorbis \
    --enable-decoder=opus \
    --enable-decoder=flac \
    --enable-decoder=alac \
    --enable-decoder=mp3 \
    --enable-decoder=amrnb \
    --enable-decoder=amrwb \
    --enable-decoder=aac \
    --enable-decoder=ac3 \
    --enable-decoder=eac3 \
    --enable-decoder=dca \
    --enable-decoder=mlp \
    --enable-decoder=truehd \
    " && \
git clone https://github.com/NitroXenon/FFmpeg-Android ffmpeg && cd ffmpeg && \
./configure \
    --libdir=android-libs/armeabi-v7a \
    --arch=arm \
    --cpu=armv7-a \
    --enable-neon \
    --enable-thumb \
    --cross-prefix="${NDK_PATH}/toolchains/arm-linux-androideabi-4.9/prebuilt/${HOST_PLATFORM}/bin/arm-linux-androideabi-" \
    --sysroot="${NDK_PATH}/platforms/android-9/arch-arm/" \
    --extra-cflags="-march=armv7-a -mfloat-abi=softfp -mfpu=neon -mtune=cortex-a7 -mthumb -D__thumb__" \
    --extra-ldflags="-Wl,--fix-cortex-a8" \
    --extra-ldexeflags=-pie \
    ${COMMON_OPTIONS} \
    && \
make -j4 && make install-libs && \
make clean && ./configure \
    --libdir=android-libs/arm64-v8a \
    --arch=aarch64 \
    --cpu=armv8-a \
    --enable-neon \
    --cross-prefix="${NDK_PATH}/toolchains/aarch64-linux-android-4.9/prebuilt/${HOST_PLATFORM}/bin/aarch64-linux-android-" \
    --sysroot="${NDK_PATH}/platforms/android-21/arch-arm64/" \
    --extra-cflags="-mtune=cortex-a57" \
    --extra-ldexeflags=-pie \
    ${COMMON_OPTIONS} \
    && \
make -j4 && make install-libs && \
make clean && ./configure \
    --libdir=android-libs/armeabi \
    --arch=arm \
    --cpu=armv5 \
    --cross-prefix="${NDK_PATH}/toolchains/arm-linux-androideabi-4.9/prebuilt/${HOST_PLATFORM}/bin/arm-linux-androideabi-" \
    --sysroot="${NDK_PATH}/platforms/android-9/arch-arm/" \
    --extra-cflags="-marm -march=armv5te -mtune=arm9tdmi -msoft-float" \
    --extra-ldexeflags=-pie \
    ${COMMON_OPTIONS} \
    && \
make -j4 && make install-libs && \
make clean && ./configure \
    --libdir=android-libs/x86 \
    --arch=x86 \
    --cpu=i686 \
    --cross-prefix="${NDK_PATH}/toolchains/x86-4.9/prebuilt/${HOST_PLATFORM}/bin/i686-linux-android-" \
    --sysroot="${NDK_PATH}/platforms/android-9/arch-x86/" \
    --extra-cflags="-march=i686 -mtune=intel -msse3 -ffast-math -mfpmath=sse -m32" \
    --extra-ldexeflags=-pie \
    --disable-asm \
    ${COMMON_OPTIONS} \
    && \
make -j4 && make install-libs && \
make clean && ./configure \
    --libdir=android-libs/x86_64 \
    --arch=x86_64 \
    --cpu=x86_64 \
    --cross-prefix="${NDK_PATH}/toolchains/x86_64-4.9/prebuilt/${HOST_PLATFORM}/bin/x86_64-linux-android-" \
    --sysroot="${NDK_PATH}/platforms/android-21/arch-x86_64/" \
    --extra-cflags="-march=x86-64 -msse4.2 -mpopcnt -m64 -mtune=intel" \
    --extra-ldexeflags=-pie \
    --disable-asm \
    ${COMMON_OPTIONS} \
    && \
make -j4 && make install-libs && \
make clean
```
