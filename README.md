# install tensorflow gpu docker
just to keep record the step used to install tensorflow gpu docker
* Ubuntu 20.04LTS
* RTX 2070 Max-Q
0. references:
* https://www.tensorflow.org/install/docker
* https://github.com/NVIDIA/nvidia-docker
1. install nvidia driver
```bash
sudo atp-get install nvidia-driver-440
```
2. install docker
```bash
sudo apt-get update
sudo apt-get install     apt-transport-https     ca-certificates     curl     gnupg-agent     software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-get install docker-ce docker-ce-cli containerd.io

sudo apt-get install linux-headers-$(uname -r)
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```
3. pull nvidia cuda docker
```bash
sudo docker run --gpu all nvidia/cuda:10.0-base nvidia-smi
```
4. pull tensorflow docker
```bash
sudo docker pull tensorflow/tensorflow:latest-gpu-jupyter
```
5. example usage
```bash
sudo docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu \
   python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
   
sudo docker run --gpus all -it tensorflow/tensorflow:latest-gpu /bin/bash
```

6. test GPU availability
```python
import tensorflow as tf
print(tf.config.list_physical_devices('GPU'))
```
7. Start docker with jupyter
```bash
sudo docker run --gpus all -it --rm -v $(realpath ~/Desktop/test):/tf/notebooks -p 8888:8888 tensorflow/tensorflow:latest-gpu-jupyter

sudo docker run --gpus all -it --rm -v $(pwd):/tf/notebooks -p 8888:8888 tensorflow/tensorflow:latest-gpu-jupyter
```

8. Build docker to include extra python package(see attached dockerfile)
```bash
sudo docker buit --rm -t tf .
sudo docker rmi tensorflow/tensorflow:latest-gpu-jupyter
sudo docker run --gpus all -it tf /bin/bash
sudo docker run --gpus all -it --rm -v $(pwd):/tf/notebooks -p 8888:8888 tf
```















