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
sudo dnf -y install cuda-devel cuda-gcc cuda-gcc-c++ nvidia-modprobe
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

NOTE: Several of these are not used and instead Hunter downloads and builds them from source.
See https://github.com/ctsrc/ethminer/commit/2d70c489271dd1dca557b60cdbd67062be6f3d67
and also https://github.com/ethereum-mining/ethminer/issues/219
and also https://github.com/ethereum-mining/ethminer/issues/500.

Furthermore, the list is based on
http://ethdocs.org/en/latest/ethereum-clients/cpp-ethereum/building-from-source/linux-fedora.html
and might include dependencies that are not relevant for ethminer.

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
cmake -DETHASHCL=OFF -DETHASHCUDA=ON -DCUDA_TOOLKIT_ROOT_DIR=/usr/include/cuda ..
```

9. Build the project using [CMake Build Tool Mode].

```sh
cmake --build .
```

[CMake Build Tool Mode]: https://cmake.org/cmake/help/latest/manual/cmake.1.html#build-tool-mode
