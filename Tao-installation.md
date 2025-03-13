pre requirment :
| Software                    | Version  |
|-----------------------------|----------------|
| Ubuntu LTS                   | 22.04   |

what we will install 
| Software                    | Version        | Comment                                                           |
|-----------------------------|----------------|-------------------------------------------------------------------|
| Ubuntu LTS                   | 22.04          |                                                                   |
| python                       | ==3.10.x       | Not needed if you are using TAO API (See #3 below)                |
| docker-ce                    | >19.03.5       | Not needed if you are using TAO API (See #3 below)                |
| docker-API                   | 1.40           | Not needed if you are using TAO API (See #3 below)                |
| nvidia-container-toolkit     | >1.3.0-1       | Not needed if you are using TAO API (See #3 below)                |
| nvidia-container-runtime     | 3.4.0-1        | Not needed if you are using TAO API (See #3 below)                |
| nvidia-docker2               | 2.5.0-1        | Not needed if you are using TAO API (See #3 below)                |
| nvidia-driver                | >550.xx        | Not needed if you are using TAO API (See #3 below)                |
| python-pip                   | >21.06         | Not needed if you are using TAO API (See #3 below)                |

install docker(https://docs.docker.com/engine/install/ubuntu/)
uninstall old-version
```
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
install new-version
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
post-install docker that Docker can be run without sudo (https://docs.nvidia.com/tao/tao-toolkit/text/quick_start_guide/beginner.html) 
Create the docker group
```
sudo groupadd docker
```
Add your user to the docker group and activate the changes 
```
sudo usermod -aG docker $USER
newgrp docker
docker run hello-world
```
install nvidia-container-toolkit (https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#next-steps)
```
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
sed -i -e '/experimental/ s/^#//g' /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
```
install other packages
```
sudo apt install python3.10 python3.10-venv nvidia-container-runtime nvidia-docker2 python-pip
```
Log in to the NGC Docker registry 
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
pulling docker
```
docker pull nvcr.io/nvidia/tao/tao-toolkit:5.5.0-tf1.15.5
```
create container and run it
be aware that **tao-folder** is where your workspace, spec files and your training data exist
```
docker run --runtime=nvidia -it --rm -d --name 5.5.0-docker -v <tao-folder>:<tao-folder> nvcr.io/nvidia/tao/tao-toolkit:5.5.0-tf1.15.5 /bin/bash
docker exec -it 5.5.0-docker /bin/bash
```
