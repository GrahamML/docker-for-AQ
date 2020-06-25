# docker_for_AQ
This repository provides a dockerfile for building a runtime environment for [GLOBIS-AQZ](https://github.com/ymgaq/AQ) (AQ v4.0.0).

>GLOBIS-AQZ is a joint project developed by GLOBIS Corporation, Mr. Yu Yamaguchi, and Tripleize Co., Ltd., provided by the National Institute of Advanced Industrial Science and Technology (AIST), and cooperated by the Nihon Ki-in. This program uses the result of GLOBIS-AQZ.

# 1. Prerequisites  
The following are additions or limitations to the requirements of AQ. 
+ Linux x86_64
+ Docker or Docker-CE
+ nvidia-docker >= 2.0
+ CUDA Toollit 10.2

# 2. Installation
## 2.1. Downloads
Download the following file:
+ AQ v4.0.0 release pacakge for linux.  
[AQ_linux.tar.gz](https://github.com/ymgaq/AQ/releases/download/v4.0.0/AQ_linux.tar.gz)

Then clone this repository:  
```console
$ git clone https://github.com/GrahamML/docker_for_AQ.git
```
## 2.2. Build the docker image
Copy the downloaded package to your local repository and build:  

```console
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
# 4. License  
[MIT](https://github.com/GrahamML/docker_for_AQ/blob/master/LICENSE)
