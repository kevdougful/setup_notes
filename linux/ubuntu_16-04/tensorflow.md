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

Environment Variables

```
export CUDA_HOME=/usr/local/cuda
export CUDA_ROOT=/usr/local/cuda
export PATH=$PATH:$CUDA_ROOT/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CUDA_ROOT/lib64
```

Install TensorFlow

```
# Ubuntu/Linux 64-bit, CPU only:
$ sudo pip install --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.8.0-cp27-none-linux_x86_64.whl

# Ubuntu/Linux 64-bit, GPU enabled. Requires CUDA toolkit 7.5 and CuDNN v4.  For
# other versions, see "Install from sources" below.
$ sudo pip install --upgrade https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.8.0-cp27-none-linux_x86_64.whl
```
