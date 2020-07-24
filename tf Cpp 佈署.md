# tensorflow 在 C++ 的佈署

## cmd

` docker run --gpus all -it -v {local path}:{docker path} --name {container name} {image name/id}`
` docker run --gpus all -it -v ~/docker-v:/docker-v --name cpp-gpu-test {image name/id}`

```
# Add the package repositories
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```

```cmd
pip install -U  pip six numpy wheel setuptools mock 'future>=0.17.1'
pip install -U  keras_applications --no-deps
pip install -U  keras_preprocessing --no-deps
```

```
git clone https://github.com/tensorflow/tensorflow.git
cd tensorflow
```

### 安裝 bazel : https://docs.bazel.build/versions/master/install-ubuntu.html

```
apt install curl gnupg
curl https://bazel.build/bazel-release.pub.gpg | apt-key add -
echo "deb [arch=amd64] https://storage.googleapis.com/bazel-apt stable jdk1.8" | tee /etc/apt/sources.list.d/bazel.list
```

```
apt update && apt install bazel
```

```
apt install bazel-1.0.0
```

## 環境安裝

tensorflow GPU 教學

- cuda
- cudnn
- GPU driver

tensorflow docker gpu ： https://www.tensorflow.org/install/docker

需要在容器裡安裝 cuda & cudnn

docker container cmd:

```cmd
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/extras/CUPTI/lib64

apt update && apt upgrade -y
apt install wget -y
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-repo-ubuntu1804_10.1.243-1_amd64.deb 
apt-get update && apt-get install -y gnupg2
apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
dpkg -i cuda-repo-ubuntu1804_10.1.243-1_amd64.deb
apt-get update
wget http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/nvidia-machine-learning-repo-ubuntu1804_1.0.0-1_amd64.deb
apt install ./nvidia-machine-learning-repo-ubuntu1804_1.0.0-1_amd64.deb
apt-get update



# Install development and runtime libraries (~4GB)
apt-get install --no-install-recommends \
    cuda-10-1 \
    libcudnn7=7.6.4.38-1+cuda10.1  \
    libcudnn7-dev=7.6.4.38-1+cuda10.1


# Install TensorRT. Requires that libcudnn7 is installed above.
apt-get install -y --no-install-recommends libnvinfer6=6.0.1-1+cuda10.1 \
    libnvinfer-dev=6.0.1-1+cuda10.1 \
    libnvinfer-plugin6=6.0.1-1+cuda10.1
```


```cmd
E: gnupg, gnupg2 and gnupg1 do not seem to be installed, but one of them is required for this operation
```

```cmd
apt-get update && apt-get install -y gnupg2
```

## Q

- sycl

echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8" | tee /etc/apt/sources.list.d/bazel.list
curl https://bazel.build/bazel-release.pub.gpg | apt-key add -

## 使用 nvidia/cuda image

```cmd
apt update && apt upgrade -y
#安裝 bazel
apt install wget -y
#apt install g++ unzip zip -y
# wget https://github.com/bazelbuild/bazel/releases/download/0.27.1/bazel-0.27.1-installer-linux-x86_64.sh


# chmod +x bazel-0.27.1-installer-linux-x86_64.sh
# ./bazel-0.27.1-installer-linux-x86_64.sh
wget https://github.com/bazelbuild/bazel/releases/download/0.27.1/bazel_0.27.1-linux-x86_64.deb
dpkg -i bazel_0.27.1-linux-x86_64.deb

wget https://github.com/bazelbuild/bazel/releases/download/0.29.1/bazel_0.29.1-linux-x86_64.deb
dpkg -i bazel_0.29.1-linux-x86_64.deb


# clone Tensorflow SC
apt install git -y
git clone https://github.com/tensorflow/tensorflow.git
cd tensorflow
```



```cmd
git checkout r2.1
./c
bazel build --config=opt --config=cuda --local_ram_resources=2048 --noincompatible_do_not_split_linking_cmdline //tensorflow:libtensorflow_cc.so
bazel build --config=opt --config=cuda --local_ram_resources=2048 --noincompatible_do_not_split_linking_cmdline //tensorflow:libtensorflow_framework.so

./tensorflow/lite/tools/make/download_dependencies.sh 

cmake .
make
```

## Go

```cmd

```