# **TAO Installation**

[**NVIDIA TAO**](https://github.com/NVIDIA/tao_tutorials), is a python-based AI toolkit that is built on TensorFlow and PyTorch for computer vision applications. It simplifies and accelerates the model training process by abstracting away the complexity of AI models and the underlying deep learning framework. You can use the power of transfer learning to fine-tune NVIDIA pre-trained models with your own data and optimize the model for inference throughput — all without the need for AI expertise or large training datasets.

On this page, we are going to install Tao 4.0.1 and use it in a **docker container**, because there is a regression issue for detectnet_v2 in 5.0.1 docker 
you can find this problem [here](https://forums.developer.nvidia.com/t/getting-0-map-for-detectnet-v2-model-over-150-epochs/316986)

## Pre-Requirment
| Software                    | Version  |
|-----------------------------|----------------|
| Ubuntu LTS                   | 22.04   |
# Installing Tao

### what we are going to install: 
| Software                    | Version        | Comment                                                           |
|-----------------------------|----------------|-------------------------------------------------------------------|
| python                       | ==3.10.x       | Not needed if you are using TAO API (See #3 below)                |
| docker-ce                    | >19.03.5       | Not needed if you are using TAO API (See #3 below)                |
| docker-API                   | 1.40           | Not needed if you are using TAO API (See #3 below)                |
| nvidia-container-toolkit     | >1.3.0-1       | Not needed if you are using TAO API (See #3 below)                |
| nvidia-container-runtime     | 3.4.0-1        | Not needed if you are using TAO API (See #3 below)                |
| nvidia-docker2               | 2.5.0-1        | Not needed if you are using TAO API (See #3 below)                |
| nvidia-driver                | >550.xx        | Not needed if you are using TAO API (See #3 below)                |
| python-pip                   | >21.06         | Not needed if you are using TAO API (See #3 below)                |

## 1. install Nvidia-driver 
### 1.1 Uninstall previous Nvidia-driver and Cuda
```
sudo apt-get --purge remove "*cublas*" "cuda*" "nsight*" 
sudo apt-get --purge remove "*nvidia*"
sudo rm -rf /usr/local/cuda*
```
### 1.2 install Nvidia-driver-550
```
sudo apt-get install nvidia-driver-550
```
## 2. install docker(https://docs.docker.com/engine/install/ubuntu/)
  
### 2.1. uninstall old-version

```
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
### 2.2. install new-version
```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo docker run hello-world
```
## 3. Post-install docker
we have to modify Docker that can be run without sudo (https://docs.nvidia.com/tao/tao-toolkit/text/quick_start_guide/beginner.html) 
### 3.1. Create the docker group
```
sudo groupadd docker
```
### 3.2. Add your user to the docker group and activate the changes 
```
sudo usermod -aG docker $USER
newgrp docker
docker run hello-world
```
## 4. install nvidia-container-toolkit (https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#next-steps)
```
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
sed -i -e '/experimental/ s/^#//g' /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
sudo apt install python3.10 python3.10-venv nvidia-container-runtime nvidia-docker2 python-pip
```
## 5. Log in to the NGC Docker registry 
``` 
docker login nvcr.io
```
Get an NGC account and API key from https://org.ngc.nvidia.com/setup/api-key or create an account in https://ngc.nvidia.com/apps and
then click on your profile and click on setup then click on generate api key 
now you have both username and password 
use it in docker login
```
Username: $oauthtoken
Password: <your api key>
```
## 6. pulling tao docker image
```
docker pull nvcr.io/nvidia/tao/tao-toolkit:4.0.1-tf1.15.5
```
## 7. Run docker container
be aware that **tao-folder** is where your workspace, spec files and your training data exist
```
docker run --runtime=nvidia -it --rm -d --name 4.0.1-docker -v <tao-folder>:<tao-folder> nvcr.io/nvidia/tao/tao-toolkit:4.0.1-tf1.15.5 /bin/bash
docker exec -it 4.0.1-docker /bin/bash
```
