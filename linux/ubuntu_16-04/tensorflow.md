## TensorFlow Setup (Ubuntu 16.04)

Install CUDA Toolkit

```
sudo apt install -y nvidia-cuda-toolkit
```

Install dependencies

```
sudo apt install -y git build-essential python-pip python-dev python-pydot python-pandas python-sklearn unzip unzip wget zip g++ gcc
```

Download cudnn and extract

```
tar -xzf cudnn*
cd cuda
```

Copy files

```
sudo mkdir -p /usr/local/cuda
sudo cp lib64 /usr/local/cuda
sudo cp include /usr/local/cuda
```
