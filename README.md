# GS-Calib
# GS-Calib: Automated Targetless LiDAR–Camera Calibration for Dynamic Construction Environments via Improved 3D Gaussian Splatting

## Dataset
# You can download the dataset from the following link:  
# Dataset Link: https://pan.baidu.com/s/1lMjIKdS9B7hF4KHUrpmxDQ

## Hardware Requirements:
# GPU: CUDA-enabled GPU with Compute Capability 7.0 or higher (e.g., NVIDIA RTX 4090 or 2080 Ti)
# RAM: At least 24 GB of VRAM for training and evaluation.
# OS: Ubuntu 18.04/20.04.

## Software Requirements:

### General Setup
# Conda: For package and environment management (recommended).
# C++ Compiler: A compatible C++ compiler (e.g., Visual Studio 2019 for Windows).
# CUDA SDK 11.8 for GPU support (use version 11.6 if you encounter issues with 11.8).
# C++ Compiler & CUDA SDK must be compatible.

## Install COLMAP
# The installation process is provided in this link: https://colmap.github.io/install.html
# Verify COLMAP Installation:
# After installation, run COLMAP in the terminal:
colmap

## Install 3DGS
# The installation process is provided in this link: https://github.com/graphdeco-inria/gaussian-splatting

# Running
# To run the optimizer, simply use
python train.py -s <path to COLMAP or NeRF Synthetic dataset>

## Install FAST-LIO2

# 1. Prerequisites
## 2.1 Ubuntu and ROS
# Ubuntu 18.04~20.04. ROS Installation.

## 2.2 PCL && Eigen && OpenCV
# PCL>=1.8, Follow PCL Installation.
# Eigen>=3.3.4, Follow Eigen Installation.
# OpenCV>=4.2, Follow Opencv Installation.

## 2.3 Sophus
# Sophus Installation for the non-templated/double-only version.
git clone https://github.com/strasdat/Sophus.git
cd Sophus
git checkout a621ff
mkdir build && cd build && cmake ..
make
sudo make install

## 2.4 Vikit
# Vikit contains camera models, some math and interpolation functions that we need. Vikit is a catkin project, therefore, download it into your catkin workspace source folder.
# Different from the one used in fast-livo1
cd catkin_ws/src
git clone https://github.com/xuankuzcr/rpg_vikit.git 

## 2. Build
# Clone the repository and catkin_make:
cd ~/catkin_ws/src
git clone https://github.com/hku-mars/FAST-LIVO2
cd ../
catkin_make
source ~/catkin_ws/devel/setup.bash

## 3. Run our examples
# Download our collected rosbag files via OneDrive (FAST-LIVO2-Dataset).
# roslaunch fast_livo mapping_avia.launch
# rosbag play YOUR_DOWNLOADED.bag

# This configuration document includes the installation steps, environment setup, and dependency instructions for COLMAP, 3DGS, and FASTLIO2, and clearly outlines how to build the project using Conda and CMake.
