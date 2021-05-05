# C# GRPC extensions for Linux

- [x] Arm32 [armhf]
- [x] Arm64 [aarch64]


**Original Idea**

**https://github.com/erikest/libgrpc_csharp_ext**

# You can use already compiled files (which should be Linux SO agnostic)
# or
# Build your own C# ext files for GRPC

## Install necessary deps for building

### Debian based SO (Ubuntu/Debian/LinuxMint/...)
```bash
###
#Install Tool Chain
###

sudo apt-get install build-essential autoconf libtool pkg-config
sudo apt-get install libgflags-dev libgtest-dev
sudo apt-get install clang libc++-dev
sudo apt-get install cmake
```


### Arch based SO (Arch/Manjaro/...)
```bash
###
#Install Tool Chain
###

sudo pacman -S build-essential autoconf libtool pkg-config base-devel
sudo pacman -S clang cmake go
```

### Lets clone from GitHub and build
**Replace: --branch v1.27.x with branch you actually need**

```bash
#!/bin/bash

###
#Get gRPC Source Code
###

git clone https://github.com/grpc/grpc.git --branch v1.27.x
cd grpc
git submodule update --init

###
#Compile libgrpc_csharp_ext target
###

#create location for build files
mkdir -p cmake/build
cd cmake/build

#setup the build files
cmake -DgRPC_BUILD_TESTS=OFF -DCMAKE_BUILD_TYPE="${MSBUILD_CONFIG}" ../..

#compile libgrpc_csharp_ext.so 
make -j4 grpc_csharp_ext
```

### Finally copy generated ext to some directory
**In this case it will copy generated file to directory where you cloned grpc from github**

```bash
#Copy the file back to repository root and rename it to what gRPC.Core currently looks for
cp libgrpc_csharp_ext.so ../../../libgrpc_csharp_ext.x64.so
```

# Notes
**WARNING: GRPC dont support officially arm32, but compiled .so for armhf should work anyway on specific versions of GRPC**

# LinuxArm64 (aarch64)
## Published NetCore app as self-contained -> linux-arm64
### Rename generated .so into -> libgrpc_csharp_ext.x64.so

## Published NetCore app as framework-dependant -> linux-arm64
### Rename generated .so into -> libgrpc_csharp_ext.arm64.so

# LinuxArm32 (armhf)

## Published NetCore app as self-contained -> linux-arm
### Rename generated .so into -> libgrpc_csharp_ext.x86.so

## Published NetCore app as framework-dependant -> linux-arm
### Rename generated .so into -> libgrpc_csharp_ext.arm.so
