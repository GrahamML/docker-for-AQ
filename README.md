# docker_for_AQ
This repository provides a dockerfile for building a runtime environment for [GLOBIS-AQ](https://github.com/ymgaq/AQ) (AQ v4.0.0).

>GLOBIS-AQZ is a joint project developed by GLOBIS Corporation, Mr. Yu Yamaguchi, and Tripleize Co., Ltd., provided by the National Institute of Advanced Industrial Science and Technology (AIST), and cooperated by the Nihon Ki-in. This program uses the result of GLOBIS-AQZ.

# 1. Prerequisites  
The following are additions or limitations to the requirements of AQ. 
+ Linux x86_64
+ Docker or Docker-CE
+ nvidia-docker >= 2.0
+ CUDA Toollit 10.2

# 2. Installation
## 2.1 Downloads
Download the following file:
+ AQ v4.0.0 release pacakge for linux.  
[AQ_linux.tar.gz](https://github.com/ymgaq/AQ/releases/download/v4.0.0/AQ_linux.tar.gz)

Then clone this repository:  
```
$ git clone https://github.com/GrahamML/docker_for_AQ.git
```
## 2.2 Build the docker image
Copy the downloaded package to your local repository and build:  

```
$ cd ./docker_for_AQ/dockerfile
$ cp ~/Downloads/AQ_linux.tar.gz .
$ docker build --tag=[image_name:tag] . --build-arg RT=20.03
```  
+ This dockerfile install the downloaded packages on the [NVIDIA TensorRT official docker image](https://docs.nvidia.com/deeplearning/tensorrt/container-release-notes/running.html#running), and make the AQ runtime.
+ The following is an example of a build command. The `--build-arg` option can be omitted.  
    ```
    $ docker build --tag=aq:4.0.0 . 
    ```
+ The following shows the version of Ubuntu, CUDA, and TensorRT in this container. 

    | Ubuntu | CUDA Toolkit        | TensorRT       |
    |--------|---------------------|----------------|
    | 18.04  | NVIDIA CUDA 10.2.89 | TensorRT 7.0.0 |

# 3. How to run
## 3.1 Start the docker image
Start the docker image as follows:  
```
$ docker run \
    -it \
    --net host \
    --runtime nvidia \
    --entrypoint /bin/bash \
    --name [container_name] \
    [image_name:tag]
```  
## 3.2 Launch the AQ
Launch the AQ in this container.  The following is an example of launching in self-play mode:
```
$ cd AQ
$ ./AQ --self
```  
+ Please refer to the [AQ's readme](https://github.com/ymgaq/AQ) for more information on onptions and launch modes.
# 4. License  
[MIT](https://github.com/GrahamML/docker_for_AQ/blob/master/LICENSE)
