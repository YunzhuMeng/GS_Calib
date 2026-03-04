# GS_Calib
GS-Calib: Automated Targetless LiDAR–Camera Calibration for Dynamic Construction Environments via Improved 3D Gaussian Splatting

# Dataset
Dataset link is：https://pan.baidu.com/s/1lMjIKdS9B7hF4KHUrpmxDQ

# Hardware Requirements
GPU: CUDA-enabled GPU with Compute Capability 7.0 or higher (e.g., NVIDIA RTX 4090 or 2080 Ti).

RAM: At least 24 GB of VRAM for training and evaluation.

OS: Ubuntu 18.04/20.04.

# Software Requirements:
General Setup

Conda: For package and environment management (recommended).

C++ Compiler: A compatible C++ compiler (e.g., Visual Studio 2019 for Windows).

CUDA SDK 11.8 for GPU support (use version 11.6 if you encounter issues with 11.8).

C++ Compiler & CUDA SDK must be compatible.

# Install COLMAP
The installation process is provided in this link：https://colmap.github.io/install.html

Verify COLMAP Installation:

After installation, run COLMAP in the terminal:

colmap

# Install 3DGS
The installation process is provided in this link：https://github.com/graphdeco-inria/gaussian-splatting

Running

To run the optimizer, simply use

python train.py -s <path to COLMAP or NeRF Synthetic dataset>

# Install FAST-LIO2
1、 Prerequisited

2.1 Ubuntu and ROS

Ubuntu 18.04~20.04. ROS Installation.

2.2 PCL && Eigen && OpenCV

PCL>=1.8, Follow PCL Installation.

Eigen>=3.3.4, Follow Eigen Installation.

OpenCV>=4.2, Follow Opencv Installation.

2.3 Sophus

Sophus Installation for the non-templated/double-only version.

git clone https://github.com/strasdat/Sophus.git

cd Sophus

git checkout a621ff

mkdir build && cd build && cmake ..

make

sudo make install

2.4 Vikit

Vikit contains camera models, some math and interpolation functions that we need. Vikit is a catkin project, therefore, download it into your catkin workspace source folder.

Different from the one used in fast-livo1

cd catkin_ws/src

git clone https://github.com/xuankuzcr/rpg_vikit.git 

2. Build

Clone the repository and catkin_make:

cd ~/catkin_ws/src

git clone https://github.com/hku-mars/FAST-LIVO2

cd ../

catkin_make

source ~/catkin_ws/devel/setup.bash

4. Run our examples

Download our collected rosbag files via OneDrive (FAST-LIVO2-Dataset).

roslaunch fast_livo mapping_avia.launch

rosbag play YOUR_DOWNLOADED.bag

This configuration document includes the installation steps, environment setup, and dependency instructions for COLMAP, 3DGS, and FASTLIO2, and clearly outlines how to build the project using Conda and CMake.


# Parameter Tables

### Table 1: LiDAR-IMU Parameter Descriptions

| Category   | Parameter                                | Value          |
|------------|------------------------------------------|----------------|
| IMU Noise  | Gyroscope noise `α_g_sd`                 | 0.1&deg;       |
| IMU Noise  | Accelerometer noise `α_a_sd`             | 0.1&deg;       |
| Bias RW    | Gyroscope bias `α_g_bg`                  | 1e-4&deg;      |
| Bias RW    | Accelerometer bias `α_a_ba`              | 1e-4&deg;      |
| Livox      | Scan lines                               | 6              |
| Livox      | Detection range                          | 0.5–450 m      |
| Livox      | FOV (Horizontal × Vertical)              | 70.4&deg; × 77.2&deg; |
| Livox      | Scan frequency                           | 10 Hz          |

---

### Table 2: Adaptive Dense 3DGS Training Iterations and Learning Rate Strategy

| Parameter               | Value     | Description                                        |
|-------------------------|-----------|----------------------------------------------------|
| `iterations`            | 7000      | Total number of training iterations               |
| `position_lr_init`      | 1.6e-4    | Initial learning rate for position parameters     |
| `position_lr_final`     | 1.6e-6    | Final learning rate for position parameters       |
| `position_lr_delay_mult`| 0.01      | Position learning rate delay multiplier           |
| `position_lr_max_steps` | 30000     | Max steps for position learning rate scheduling   |
| `feature_lr`            | 2.5e-3    | Learning rate for features                        |
| `opacity_lr`            | 5.0e-4    | Learning rate for opacity                         |
| `scale_lr`              | 5.0e-3    | Learning rate for Gaussian scale                  |
| `rotation_lr`           | 1.0e-3    | Learning rate for Gaussian rotation               |

---

### Table 3: Pruning and Densification Strategy Parameters

| Parameter                | Value     | Description                                      |
|--------------------------|-----------|--------------------------------------------------|
| `percent_dense`          | 2.5e-4    | Base densification sampling ratio               |
| `densification_interval` | 100       | Densification every N iterations                |
| `densify_from_iter`      | 500       | Start densification from this iteration         |
| `densify_until_iter`     | 15000     | Stop densification at this iteration            |
| `densify_grad_threshold` | 1.5e-4    | Gradient threshold for densification            |

---

### Table 4: Fast-LIO2 Key Parameter Settings

| Module      | Parameter               | Value                         | Description                                     |
|-------------|-------------------------|-------------------------------|-------------------------------------------------|
| Common      | `lid_topic`             | `/livox/lidar`                | LiDAR data topic name                           |
|             | `imu_topic`             | `/livox/imu`                  | IMU data topic name                             |
|             | `time_sync_en`          | false                         | Enable software sync if hardware unavailable    |
| Preprocess  | `lidar_type`            | 1                             | Livox-type LiDAR configuration                  |
|             | `scan_line`             | 6                             | Number of Livox scan lines                      |
|             | `blind`                 | 4                             | Blind zone threshold (m)                        |
| Mapping     | `acc_cov`               | 0.1                           | Accelerometer noise covariance                  |
|             | `gyr_cov`               | 0.1                           | Gyroscope noise covariance                      |
|             | `b_acc_cov`             | 0.0001                        | Accelerometer bias RW covariance                |
|             | `b_gyr_cov`             | 0.0001                        | Gyroscope bias RW covariance                    |
|             | `fov_degree`            | 90                            | Field-of-view range (°)                         |
|             | `det_range`             | 450.0                         | Max detection range (m)                         |
|             | `extrinsic_T`           | [0.04165, 0.02326, -0.0284]   | IMU-to-LiDAR translation                        |
|             | `extrinsic_R`           | Identity                      | IMU-to-LiDAR rotation                           |
| Publish     | `scan_publish_en`       | true                          | Publish current frame point cloud               |
|             | `dense_publish_en`      | true                          | Publish dense point cloud                       |
|             | `scan_bodyframe_pub_en` | true                          | Publish in IMU body coordinate frame            |

---

### Table 5: COLMAP-based SFM Parameters

| Stage            | Parameter                                 | Value     | Description                              |
|------------------|-------------------------------------------|-----------|------------------------------------------|
| Feature          | `ImageReader.single_camera`               | 1         | Use shared intrinsics                    |
|                  | `SiftExtraction.max_num_features`         | 8192      | Max SIFT features per image              |
|                  | `SiftExtraction.num_octaves`              | 4         | Number of SIFT pyramid levels            |
| Feature Matching | `SiftMatching.use_gpu`                    | 1         | Use GPU for matching                     |
|                  | `SiftMatching.max_ratio`                  | 0.8       | Lowe's ratio test threshold              |
| Mapper           | `Mapper.ba_global_function_tolerance`     | 0.0001    | Global BA convergence tolerance          |
|                  | `Mapper.ba_local_max_num_iterations`      | 25        | Local BA max iterations                  |
| Image            | `ImageUndistorter.max_image_size`         | -1        | No image size limit                      |

---

### Table 6: Target Segmentation Parameters

| Stage             | Parameter                 | Value       |
|-------------------|---------------------------|-------------|
| ROI projection    | Trajectory-based polygon  | Enabled     |
| Ground removal    | `z_min`                   | -0.5        |
| Low-height filter | `z_low_max`               | 0.1         |
| Neighborhood      | `r`                       | 0.01 m      |
|                   | `N_min`                   | 3           |

---

### Table 7: PL-ICP Refinement Parameters

| Parameter                    | Value     | Description                        |
|------------------------------|-----------|------------------------------------|
| `voxel_size`                 | 0.05 m    | Voxel size                         |
| `max_correspondence_distance`| 0.12 m    | Max correspondence distance        |
| `max_iterations`             | 50        | Max iterations                     |
| `convergence_threshold`      | 1e-6      | Convergence threshold              |

