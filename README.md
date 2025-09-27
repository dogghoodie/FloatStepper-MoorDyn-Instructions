# FloatStepper-MoorDyn-Instructions
Set up FloatStepper and MoorDyn on a fresh OpenFOAM environment with a reproducible toolchain.
These steps show you how to source the correct OpenFOAM version (either the distro package or a local build),
compile VTK into a user prefix, and then build FloatStepper and its MoorDyn dependency.

Written by dogghoodie & topspook

## Sourcing OpenFOAM
1. Add these lines source this openfoam version from your ~/.bashrc.
  * Pre-compiled
  ```bash
  echo 'source /usr/lib/openfoam/openfoam2412/etc/bashrc' >> ~/.bashrc 

  ```
  * Compiled
  ```bash
  echo 'source ~/openFOAM-v2412/OpenFOAM-v2412/etc/bashrc' >> ~/.bashrc 
  ```
2. Source your ~/.bashrc
  ```bash
  source ~/.bashrc
  ```

## FloatStepper Installation
1. Clone the FloatStepper repository in your ~home directory.
  ```bash
  git clone https://github.com/FloatStepper/FloatStepper.git
  ```
2. Navigate into the repository.
  ```bash
  cd FloatStepper
  ```
3. Run the compile script.
  ```bash
  ./Allwmake
  ```

## VTK Setup
1. Ensure CMake is installed
  ```bash
  sudo apt update
  sudo apt install -y build-essential cmake git \
      libxt-dev libgl1-mesa-dev libglu1-mesa-dev
  ```
2. Clone the VTK repository in your ~home directory.
3. Navigate into the repository
  ```bash
  cd VTK
  ```
4. Create the build directory and compile VTK.
  ```bash
  mkdir build
  cd build
  cmake .. \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=$HOME/local/vtk \
    -DBUILD_SHARED_LIBS=ON \
    -DVTK_BUILD_EXAMPLES=OFF \
    -DVTK_GROUP_ENABLE_Qt=NO
  make -j$(( $(nproc) -2 ))
  make install
  ```
5. Export the path to vtk in your ~/.bashrc.
  ```bash
  echo 'export CMAKE_PREFIX_PATH="$HOME/local/vtk:${CMAKE_PREFIX_PATH:-}"' >> ~/.bashrc
  ```
6. Source your ~/.bashrc
  ```bash
  source ~/.bashrc
  ```
6. Navigate to FloatStepper/ThirdParty/MoorDyn and run the make file.
  ```bash
  ./Allwmake
  ```
