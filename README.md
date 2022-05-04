# 🍏 Isaac ROS Template

Example fork from [Isaac ROS template](https://github.com/rbonghi/isaac_ros_template) to use Isaac ROS vslam with a realsense camera

# Test this example

You need:
 * [NVIDIA Jetson](https://developer.nvidia.com/buy-jetson)
   *  NVIDIA Jetson AGX Xavier
   *  NVIDIA Jetson Xavier NX
   *  NVIDIA Jetson Nano (4Gb or 2Gb)
 * [NVIDIA Jetpack 4.6.1](https://developer.nvidia.com/jetpack-sdk-461)

# How is changed

There are three parts on this template to edit:
1. [Dockerfile](#dockerfile)
2. [01_isaac_ros.rosinstall](#01_isaac_rosrosinstall)
3. [02_your_ros2_pkgs.rosinstall](#02_your_ros2_pkgsrosinstall)

Let's move step by step and learn where you need to change

## [Dockerfile](/Dockerfile)

There are few changes in this Dockerfile

### (Stage 4) Install your ROS2 dependecies

Added realsense driver dependency

```Dockerfile
# Install Realsense
RUN apt-get update && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE && \
    add-apt-repository -y "deb https://librealsense.intel.com/Debian/apt-repo $(lsb_release -cs) main" -u && \
    apt-get install -y rsync librealsense2-utils librealsense2-dev && \
    rm -rf /var/lib/apt/lists/*
```

Clone realsense ros2 repository in (stage 5)

```Dockerfile
RUN cd ${ROS_WS}/src && \
    git clone --depth 1 --branch `git ls-remote --tags https://github.com/IntelRealSense/realsense-ros.git | grep -Po "(?<=tags/)3.\d+\.\d+" | sort -V | tail -1` https://github.com/IntelRealSense/realsense-ros.git
```

## [01_isaac_ros.rosinstall](/01_isaac_ros.rosinstall)

Added Isaac ROS packages in rosinstall file

```yml
- git:
    local-name: isaac_ros_image_pipeline
    uri: https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_image_pipeline.git
- git:
    local-name: isaac_ros_visual_slam
    uri: https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_visual_slam.git
```

# Build Isaac ROS Docker image

```
bash build_image.bash isaac_ros_realsense
```

# Run You Isaac ROS docker container

```
docker run --rm --network host --privileged isaac_ros_realsense:latest
```

# Debug your container

```
docker run --rm --network host -it isaac_ros_realsense:latest bash
```

# Reference

* [NVIDIA Isaac](https://developer.nvidia.com/isaac)
* [NVIDIA Isaac ROS GEMs](https://developer.nvidia.com/isaac-ros-gems)
* [NVIDIA Isaac ROS GEMs repositories](https://github.com/NVIDIA-ISAAC-ROS)
* [NVIDIA Jetson](https://www.nvidia.com/en-gb/autonomous-machines/embedded-systems/)
* [NVIDIA Jetson containers](https://github.com/dusty-nv/jetson-containers)
* [NVIDIA Jetpack](https://developer.nvidia.com/embedded/jetpack)

Developer blog posts and webinars:
 * [Integrating Isaac ROS Visual Odometry GEM on Jetson](https://info.nvidia.com/isaac-ros-and-nvidia-jetson-wbn.html)
 * [Isaac ROS tutorial](https://github.com/rbonghi/isaac_ros_tutorial) Many examples to build your own docker for Isaac ROS
 * [Designing Robots with NVIDIA Isaac GEMs for ROS](https://developer.nvidia.com/blog/designing-robots-with-isaac-gems-for-ros/)
 * [Isaac ROS template](https://github.com/rbonghi/isaac_ros_template)


Robot example with Isaac ROS:
 * [nanosaur.ai](https://nanosaur.ai)
 * [nanosaur architecture](https://nanosaur.ai/architecture/)
 * [nanosaur_perception](https://github.com/rnanosaur/nanosaur_perception) GPU accelerated repository