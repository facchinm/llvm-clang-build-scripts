This repository contains some prebuilt binaries for llvm+clang with a step by step
guide to reproduce them for each OS.

## Ubuntu 14.04.5 LTS 64bit

Install prerequisites:

```
apt-get install build-essentials
```

Build and install cmake (in this case 3.8.0, but any later version is fine)

```
wget https://cmake.org/files/v3.8/cmake-3.8.0.tar.gz
tar xzvf cmake-3.8.0.tar.gz
cd cmake-3.8.0/
./bootstrap
make -j4
sudo make install
cd ..
```

Build llvm+clang and install into `/../llvm-clang-4.0.0-ubuntu-14.04.5-x86_64`

```
wget http://releases.llvm.org/4.0.0/llvm-4.0.0.src.tar.xz
tar xfv llvm-4.0.0.src.tar.xz
wget http://releases.llvm.org/4.0.0/cfe-4.0.0.src.tar.xz
tar xfv cfe-4.0.0.src.tar.xz
mv cfe-4.0.0.src llvm-4.0.0.src/tools/clang
mkdir clang-build
cd clang-build
cmake ../llvm-4.0.0.src -G "Unix Makefiles" \
  -DCMAKE_BUILD_TYPE=Release \
  -DLLVM_TARGETS_TO_BUILD="ARM;X86" \
  -DLLVM_BUILD_TOOLS=ON \
  -DLLVM_INCLUDE_EXAMPLES=OFF \
  -DLLVM_BUILD_TESTS=OFF \
  -DLLVM_INCLUDE_TESTS=OFF \
  -DLLVM_ENABLE_EH=ON \
  -DLLVM_ENABLE_RTTI=ON \
  -DCMAKE_C_COMPILER=gcc -DCMAKE_CXX_COMPILER=g++ \
  -DCMAKE_CXX_FLAGS=-std=c++11 \
  -DCMAKE_INSTALL_PREFIX=`pwd`/../llvm-clang-4.0.0-ubuntu-14.04.5-x86_64 \
  -DBUILD_SHARED_LIBS=OFF
make
make install
cd ..
```

Compress and archive the result of the build:

```
tar c llvm-clang-4.0.0-ubuntu-14.04.5-x86_64 | xz > llvm-clang-4.0.0-ubuntu-14.04.5-x86_64.tar.xz
```

