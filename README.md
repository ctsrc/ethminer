# ethminer -- Ubuntu 17.10 / CUDA 9.1 / Nvidia GTX 1060 6GB

Personalized README for GPU mining with [ethereum-mining/ethminer](https://github.com/ethereum-mining/ethminer) on my workstation.

This README is current as of commit [f27c5b584](https://github.com/ctsrc/ethminer/commit/f27c5b58431ec63c8c9a2bec97f7c282f5b45e43) to the ethminer master branch.

## Usage

```sh
cd ~/src/github.com/ethereum-mining/ethminer/build/
./ethminer/ethminer -U \
  -P stratum2+tcp://3Qsz9ZbmxYojJcGxwm2KqG631yA38wt7gY.GPU:x@daggerhashimoto.eu.nicehash.com:3353
```

## Building from source

1. Install dependencies.

```sh
sudo apt-get install -y build-essential gcc-6 g++-6 git cmake libcrypto++-dev \
  libleveldb-dev libjsoncpp-dev libjsonrpccpp-dev libboost-all-dev libgmp-dev \
  libreadline-dev libcurl4-openssl-dev libmicrohttpd-dev
```

2. Install CUDA 9.1. https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1704&target_type=runfilelocal

3. Clone [ethereum-mining/ethminer](https://github.com/ethereum-mining/ethminer)

```bash
mkdir -p ~/src/github.com/ethereum-mining
cd !$
git clone git@github.com:ethereum-mining/ethminer.git
cd ethminer
```

4. Update git submodules.

```sh
git submodule update --init --recursive
```

5. Create a build directory.

```sh
mkdir build; cd build
```

6. Configure the project with CMake.

```sh
export CC=/usr/bin/gcc-6
export CXX=/usr/bin/g++-6
cmake -DETHASHCL=OFF -DETHASHCUDA=ON -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda-9.1 ..
```

7. Build the project using [CMake Build Tool Mode].

```sh
cmake --build .
```

[CMake Build Tool Mode]: https://cmake.org/cmake/help/latest/manual/cmake.1.html#build-tool-mode
