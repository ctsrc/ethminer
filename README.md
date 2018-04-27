# ethminer – Fedora 27 / CUDA 9.1 / Nvidia GTX 1060 6GB

Personalized README for GPU mining with [ethereum-mining/ethminer](https://github.com/ethereum-mining/ethminer) on my workstation.

This README is current as of commit [30df0e6](https://github.com/ctsrc/ethminer/commit/30df0e6dbba5814e025fc4271764081589c48a19) to the ethminer master branch.

## Usage

```sh
cd ~/src/github.com/ethereum-mining/ethminer/build/
./ethminer/ethminer -U \
  -P stratum2+tcp://3Qsz9ZbmxYojJcGxwm2KqG631yA38wt7gY.GPU:x@daggerhashimoto.eu.nicehash.com:3353
```

## Build from source

1. Update packages

```sh
sudo dnf update
```

2. Install Nvidia drivers and CUDA 9.1

https://negativo17.org/nvidia-driver/

```sh
sudo dnf config-manager --add-repo=https://negativo17.org/repos/fedora-nvidia.repo
sudo dnf -y install nvidia-driver nvidia-settings kernel-devel akmod-nvidia
sudo dnf -y install cuda-devel cuda-gcc cuda-gcc-c++
```

3. Reboot

```sh
sudo reboot
```

4. Install other dependencies.

```sh
sudo dnf -y install git automake autoconf libtool cmake gcc gcc-c++ xkeyboard-config \
  leveldb-devel boost-devel gmp-devel cryptopp-devel miniupnpc-devel qt5-qtbase-devel \
  qt5-qtdeclarative-devel qt5-qtquickcontrols2-devel qt5-qtwebkit-devel snappy-devel \
  ncurses-devel readline-devel curl-devel python-devel jsoncpp-devel libjson-rpc-cpp-devel \
  argtable-devel libmicrohttpd-devel
```

```sh
mkdir ~/build ; cd ~/build
wget http://downloads.cpp-netlib.org/0.12.0/cpp-netlib-0.12.0-final.tar.bz2
tar xf cpp-netlib-0.12.0-final.tar.bz2
mkdir cpp-netlib ; cd cpp-netlib
cmake ../cpp-netlib-0.12.0-final
sudo make install
```

5. Clone [ethereum-mining/ethminer](https://github.com/ethereum-mining/ethminer)

```bash
mkdir -p ~/src/github.com/ethereum-mining
cd !$
git clone git@github.com:ethereum-mining/ethminer.git
cd ethminer
```

6. Update git submodules.

```sh
git submodule update --init --recursive
```

7. Create a build directory.

```sh
mkdir build; cd build
```

8. Configure the project with CMake.

```sh
export CC=/usr/bin/cuda-gcc
export CXX=cuda-g++
export PKG_CONFIG_PATH=/usr/lib64/pkgconfig
cmake -DETHASHCL=OFF -DETHASHCUDA=ON -DHUNTER_ENABLED=OFF \
  -DCUDA_TOOLKIT_INCLUDE_DIR=/usr/include/cuda ..
```

9. Build the project using [CMake Build Tool Mode].

```sh
cmake --build .
```

[CMake Build Tool Mode]: https://cmake.org/cmake/help/latest/manual/cmake.1.html#build-tool-mode
