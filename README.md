# docker-for-AQ
This repository provides a dockerfile for building a runtime environment for [GLOBIS-AQZ](https://github.com/ymgaq/AQ) (AQ v4.0.0).

>GLOBIS-AQZ is a joint project developed by GLOBIS Corporation, Mr. Yu Yamaguchi, and Tripleize Co., Ltd., provided by the National Institute of Advanced Industrial Science and Technology (AIST), and cooperated by the Nihon Ki-in. This program uses the result of GLOBIS-AQZ.  

_日本語の説明は[こちら](https://github.com/GrahamML/docker-for-AQ/blob/master/README_JP.md)を参照ください。_  

# 1. Prerequisites  
The following are additions or limitations to the requirements of AQ. 
+ Linux x86_64
+ Docker
+ nvidia-docker2 (Docker >= 1.12) or nvidia-container-toolkit (Docker >= 19.03)  

_Learn more about how to install [docker](https://github.com/Microsoft/MMdnn/blob/master/docs/InstallDockerCE.md) and [nvidia-container-toolkit](https://github.com/NVIDIA/nvidia-docker#quickstart)_

# 2. Installation
## 2.1. Downloads
Clone this repository:  
```console
$ git clone https://github.com/GrahamML/docker-for-AQ.git
```
## 2.2. Build the docker image
```console
$ cd ./docker-for-AQ/dockerfile
$ docker build --tag=[image_name:tag] . --build-arg RT=20.03
```  
+ This dockerfile downloads the AQ release package on the [NVIDIA TensorRT official docker image](https://docs.nvidia.com/deeplearning/tensorrt/container-release-notes/running.html#running), and make runtime.  
+ If you are not in the docker group, you will need to change the `docker` command to `sudo docker`.
+ The following is an example of a build command. The `--build-arg` option can be omitted.  
    ```
    $ docker build --tag=aq:4.0.0 . 
    ```
+ The following shows the version of Ubuntu, CUDA, cuDNN and TensorRT in this container. 

    | Ubuntu | CUDA Toolkit  | cuDNN    | TensorRT     |
    |--------|---------------|----------|--------------|
    | 18.04  | 10.2.89       | 7.6.5.32 | 7.0.0        |

# 3. How to run
## 3.1. Start the docker image
Start the docker image as follows:  
```console
$ docker run \
    -it \
    --net host \
    --runtime nvidia \
    --entrypoint /bin/bash \
    --name [container_name] \
    [image_name:tag]
```  
+ The example above is for `nvidia-docker2` case. If it's `nvidia-container-toolkit` case, change it appropriately.  

## 3.2. Launch the AQ
Launch the AQ in this container.  The following is an example of launching in self-play mode:
```console
$ cd AQ
$ ./AQ --self
Configuration is loaded.
building engine ... completed.
game ply=1: left time=0.0[sec]
163[nodes] 44.8[sec] 161[playouts] 3.6[pps/thread]
total games=162, evaluated =161
|move|count  |value|roll |prob |depth| best sequence
|D16 |     46| 44.1| 37.5| 19.9|    8| D16->Q4 ->Q16->D4 ->R6 ->R17->R16
|Q16 |     46| 44.1| 28.6| 19.9|    8| Q16->D4 ->D16->Q4 ->R6 ->R17->R16
|Q4  |     37| 44.2| 25.0| 20.8|    8| Q4 ->D16->Q16->D4 ->F17->R17->Q17
|D4  |     32| 44.3| 25.0| 21.1|    7| D4 ->Q16->D16->Q4 ->O17->C17->D17
   A  B  C  D  E  F  G  H  J  K  L  M  N  O  P  Q  R  S  T 
19 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  . 19
18 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  . 18
17 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  . 17
16 .  .  . [X] .  .  .  .  .  +  .  .  .  .  .  +  .  .  . 16
15 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  . 15
14 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  . 14
13 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  . 13
12 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  . 12
11 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  . 11
10 .  .  .  +  .  .  .  .  .  +  .  .  .  .  .  +  .  .  . 10
 9 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  9
 8 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  8
 7 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  7
 6 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  6
 5 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  5
 4 .  .  .  +  .  .  .  .  .  +  .  .  .  .  .  +  .  .  .  4
 3 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  3
 2 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  2
 1 .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  .  1
   A  B  C  D  E  F  G  H  J  K  L  M  N  O  P  Q  R  S  T 
 :
```  
+ Refer to the [AQ's readme](https://github.com/ymgaq/AQ) for more information on onptions and launch modes.  
+ As mentioned in the AQ's README, the first launch will take a few minutes to build the engine. After the engine is built under `/workspace/AQ/engine`, it is recommended to save the current container as follows. Then it will start up faster from the second time.  

    ```console
    $ docker commit [container_ID] [image_name:tag]  
    ```

# 4. Communitacion with Lizzie  
See this [wiki](https://github.com/GrahamML/docker_for_AQ/wiki/Communitacion-with-Lizzie).  

![Select](https://github.com/GrahamML/docker_for_AQ/wiki/images/Communitacion-with-Lizzie/Fig8.png)

# 5. License  
[MIT](https://github.com/GrahamML/docker_for_AQ/blob/master/LICENSE)
