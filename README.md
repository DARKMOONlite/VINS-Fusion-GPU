# About
This is fork from [VINS-Fusion-gpu](https://github.com/pjrambo/VINS-Fusion-gpu) for OpenCV 4 (and some fix).

VINS-Fusion-gpu is a version of [VINS-Fusion](https://github.com/HKUST-Aerial-Robotics/VINS-Fusion) with GPU acceleration.


# Dependencies
- [Ceres Solver](http://ceres-solver.org/installation.html)
- [ROS](http://wiki.ros.org/ROS/Installation)
- [OpenCV](https://opencv.org)
- [OpenCV bridge](https://github.com/ros-perception/vision_opencv)


# Ceres Solver

[Installation](http://ceres-solver.org/installation.html).

[Download](http://ceres-solver.org/ceres-solver-2.0.0.tar.gz) latest stable release. Test it on ceres-solver-1.14 and ceres-solver-2.0.

#### Dependencies

```
sudo apt-get install -y libgoogle-glog-dev libatlas-base-dev libsuitesparse-dev
```

#### Build
```
mkdir ceres-build
cd ceres-build
cmake -D BUILD_TESTING=OFF \
      -D BUILD_EXAMPLES=OFF \
      ../ceres-solver-2.0.0
sudo make install
```

# OpenCV bridge
This is require ROS and OpenCV bridge for OpenCV 4.

```
cd ~/catkin_ws/src
git clone https://github.com/ros-perception/vision_opencv
git checkout origin/noetic
```
>[!TIP]
> If you get errors related to the python version: `nano vision_opencv/cv_bridge/CMakeLists.txt` and change the following line `find_package(Boost REQUIRED python37) -> find_package(Boost REQUIRED python3)`


>[!TIP]
> if you get compilation errors then: `nano vision_opencv/cv_bridge/src/module.hpp` and add the following lines
```
#include <numpy/ndarrayobject.h> ``
 #define NUMPY_IMPORT_ARRAY_RETVAL NULL
```




Build OpenCV bridge:
>[!NOTE]
> Ensure that you're building with `C++17` or greater.
>also if building on a device with low available memory (*jetson nano*), try reducing the number of jobs to prevent stalling `-j2`

```
cd ~/catkin_ws
catkin_make --cmake-args -DCMAKE_CXX_STANDARD=17
```


# Build
Build is same as [VINS-Fusion-gpu](https://github.com/pjrambo/VINS-Fusion-gpu#2-build-vins-fusion) and [VINS-Fusion](https://github.com/HKUST-Aerial-Robotics/VINS-Fusion#2-build-vins-fusion).

#### Dependencies
```
sudo apt-get install ros-noetic-tf ros-noetic-image-transport
```

#### Build
Clone the repository and catkin_make:
```
cd ~/catkin_ws/src
git clone https://github.com/IOdissey/VINS-Fusion-GPU.git
cd ../
catkin_make
source ~/catkin_ws/devel/setup.bash
```


# Usage
In the config file, there are two parameters for gpu acceleration.

GPU for GoodFeaturesToTrack (0 - off, 1 - on) 
```
use_gpu: 1
```

GPU for OpticalFlowPyrLK (0 - off, 1 - on) 
```
use_gpu_acc_flow: 1
```

If your GPU resources is limitted or you want to use GPU for other computaion. You can set
```
use_gpu: 1
use_gpu_acc_flow: 0
```

If your other application do not require much GPU resources (recommanded)
```
use_gpu: 1
use_gpu_acc_flow: 1
```

There are two parameters for config OpticalFlowPyrLK (pyramid level and window size):
```
lk_n: 3
lk_size: 21
```
