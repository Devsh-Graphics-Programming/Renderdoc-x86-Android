# Renderdoc-x86-Android
Apk binary build of Renderdoc for x86 arch

# Compiling renderdoccmd apk

## Linux

### Installing dependencies (skippable once done)

- g++
- openjdk-8-jdk
- git
- android SDK
- android NDK
- librandr-dev 
- x11-xserver-utils
- libcurses
- cmake

Once android SDK is downloaded and installed (instalation can be simplified by downloading android studio via ubuntu software), 
navigate to `/home/username/Android/Sdk`

Then, download android NDK https://developer.android.com/ndk/downloads
extract it inside sdk dir so the path looks more or less like this `/home/username/Android/Sdk/android-ndk-r20b`
version number may change.

### Prepping for cmake file gen

Edit these to be correct and run the commands

```
export JAVA_HOME="/usr/lib/jvm/java-x.x.x-openjdk"
export ANDROID_NDK="/home/username/Android/Sdk/android-ndk-r14b"
export ANDROID_SDK="/home/username/Android/Sdk" 
```

Accept the android-sdk license (also skippable once done)

```
cd $ANDROID_SDK/tools/bin
./sdkmanager --sdk_root=$ANDROID_SDK "build-tools;26.0.1" "platforms;android-23"
```

### Setting up RenderDoc Repo

Clone the renderdoc repository

**Important:**
_**Renderdoc’s android apk stores the git commit’s SHA and renderdoc’s version. When launching renderdoc remote server, there will be a check if the PC's client version and commit on which the binary was built is the same as APK’s. Make sure to either create a apk on the commit of the latest release, or there will be a need to build the desktop app too.**_

Open `renderdoccmd/CMakeLists.txt`, alter line `#163` to not throw an error, and instead assign package name variable
```
set(RENDERDOC_ANDROID_PACKAGE_NAME “org.renderdoc.renderdoccmd.x86_64")
```

Then, inside the root CMakeLists.txt there is a variable for storing the package hash to the same as the latest release tag on github 
navigate to repo root directory and open terminal. 

Create a directory for build files inside the source, run cmake file gen, and compile the apk. This takes a while. 

```
mkdir build-android
cd build-android
cmake -DBUILD_ANDROID=On -DANDROID_ABI=x86_64 ../
make
```

This will create the apk file in build-android/bin
If by any chance it fails, then most likely some dependency lib is not installed.

Once thats done, rename the 64 bit apk to org.renderdoc.renderdoccmd.x86

Reference: https://github.com/baldurk/renderdoc/blob/v1.x/docs/CONTRIBUTING/Compiling.md
