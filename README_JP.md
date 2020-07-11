# docker-for-AQ
[GLOBIS-AQZ](https://github.com/ymgaq/AQ) (AQ v4.0.0)の実行環境を構築するためのdockerfileを提供します。

>GLOBIS-AQZは、開発：株式会社グロービス、山口祐氏、株式会社トリプルアイズ、開発環境の提供：国立研究開発法人 産業技術総合研究所、協力：公益財団法人日本棋院のメンバーによって取り組んでいる共同プロジェクトです。このプログラムは、GLOBIS-AQZでの試算を活用しています。

# 1. 前提条件  
AQの動作要件に対する追加・制約要件は以下の通りです。 
+ Linux x86_64
+ Docker
+ nvidia-docker2 (Docker >= 1.12) あるいは nvidia-container-toolkit (Docker >= 19.03)  

_[Docker](https://github.com/Microsoft/MMdnn/blob/master/docs/InstallDockerCE.md) と [nvidia-container-toolkit](https://github.com/NVIDIA/nvidia-docker#quickstart)のインストール方法に関する情報_

# 2. インストール方法
## 2.1. ダウンロード  
以下のAQ v4.0.0 パッケージファイルを[Release](https://github.com/ymgaq/AQ/releases)からダウンロードしてください。
+ AQ_linux.tar.gz  

次に本リポジトリをクローンしてください。 
```console
$ git clone https://github.com/GrahamML/docker-for-AQ.git
```
## 2.2. Dockerイメージのビルド  
ダウンロードしたファイルをローカルリポジトリへコピーし、ビルドしてください。

```console
$ cd ./docker-for-AQ/dockerfile
$ cp ~/Downloads/AQ_linux.tar.gz .
$ docker build --tag=[image_name:tag] . --build-arg RT=20.03
```  
+ このdockerfileは[NVIDIA TensorRT 公式dockerイメージ](https://docs.nvidia.com/deeplearning/tensorrt/container-release-notes/running.html#running)の上にAQのランタイムを生成します
+ Linuxアカウントがdockerグループに属していない場合は`docker`コマンドを`sudo docker`コマンドへ置き換えてください
+ 以下はビルドコマンドの一例です。 `--build-arg`オプションは省略可能です  
    ```
    $ docker build --tag=aq:4.0.0 . 
    ```
+ 生成されるコンテナ内のUbuntu, CUDA, and TensorRTのバージョンは以下です

    | Ubuntu | CUDA Toolkit  | cuDNN    | TensorRT     |
    |--------|---------------|----------|--------------|
    | 18.04  | 10.2.89       | 7.6.5.32 | 7.0.0        |

# 3. 実行方法
## 3.1. Dockerイメージの起動
生成したDockerイメージを以下のように起動してください。
```console
$ docker run \
    -it \
    --net host \
    --runtime nvidia \
    --entrypoint /bin/bash \
    --name [container_name] \
    [image_name:tag]
```  
+ 上の例は`nvidia-docker2`の場合です。`nvidia-container-toolkit`の場合は適宜変更してください  

## 3.2. AQの起動
コンテナ内でAQを起動してください。以下は自己対局モードで起動する例です。
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
+ オプションや起動モードの詳細は[AQ'のreadme](https://github.com/ymgaq/AQ)を参照ください

# 4. Lizzieとの連携  
この[wiki](https://github.com/GrahamML/docker_for_AQ/wiki/Communitacion-with-Lizzie)を参照ください。  

![Select](https://github.com/GrahamML/docker_for_AQ/wiki/images/Communitacion-with-Lizzie/Fig8.png)

# 5. License  
[MIT](https://github.com/GrahamML/docker_for_AQ/blob/master/LICENSE)
