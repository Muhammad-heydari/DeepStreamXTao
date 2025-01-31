# Deepstream7.0
NVIDIA® DeepStream Software Development Kit (SDK) is an accelerated AI framework to build intelligent video analytics (IVA) pipelines. DeepStream runs on NVIDIA® T4, NVIDIA® Hopper, NVIDIA® Ampere, NVIDIA® ADA and platforms such as NVIDIA® Jetson AGX Orin™, NVIDIA® Jetson Orin™ NX, NVIDIA® Jetson Orin™ Nano. For dGPU platforms Enterprise GPUs are highly recommended for deployments that are expected to run 24x7. Gaming GPUs are not designed to perform in such type of environments.

### Pre-Requirement
| Software                    | Version  |
|-----------------------------|----------------|
| Ubuntu LTS                   | 22.04   |

## 1. Upgrade packages:
```
sudo apt update
sudo apt upgrade
sudo apt-get install ninja-build meson git
```
## 2. install Nvidia-driver

### 2.1 Remove Cuda and Nvidia drivers completely
```
sudo apt-get --purge remove "*cublas*" "cuda*" "nsight*" 
sudo apt-get --purge remove "*nvidia*"
sudo rm -rf /usr/local/cuda*
sudo apt-get install nvidia-driver-535
```
### 2.2 install Nvidia-driver-535
```
sudo apt-get install nvidia-driver-535
```
## 3. Install Glib
```
cd ~/Documents
mkdir Dependencies
cd Dependencies/
git clone https://github.com/GNOME/glib.git
cd glib
git checkout 2.76.6
# e.g. 2.76.6
meson build --prefix=/usr
ninja -C build/
cd build/
sudo ninja install
```
## 4. Install dependencies
```
 sudo apt install \
libssl3 \
libssl-dev \
libgles2-mesa-dev \
libgstreamer1.0-0 \
gstreamer1.0-tools \
gstreamer1.0-plugins-good \
gstreamer1.0-plugins-bad \
gstreamer1.0-plugins-ugly \
gstreamer1.0-libav \
libgstreamer-plugins-base1.0-dev \
libgstrtspserver-1.0-0 \
libjansson4 \
libyaml-cpp-dev \
libjsoncpp-dev \
protobuf-compiler \
gcc \
make \
git \
python3
```
## 5. Install Cuda, Cudnn, Tensorrt
### 5.1. Add Nvidia key to apt repository for installing Cuda, Cudnn and Tensorrt
```
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/3bf863cc.pub
sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/ /"
sudo apt-get update
```
## 5.2. Install Cuda
```
 sudo apt-get install cuda-toolkit-12-2
 sudo apt install nvidia-cuda-toolkit
```
## 5.3. Install Cudnn
```
sudo apt install cudnn9-cuda-12
```
## 5.4. Install Tensorrt
```
sudo apt-get install --no-install-recommends libnvinfer-lean8=8.6.1.6-1+cuda12.0 libnvinfer-vc-plugin8=8.6.1.6-1+cuda12.0 \
libnvinfer-headers-dev=8.6.1.6-1+cuda12.0 libnvinfer-dev=8.6.1.6-1+cuda12.0 libnvinfer-headers-plugin-dev=8.6.1.6-1+cuda12.0 \
libnvinfer-plugin-dev=8.6.1.6-1+cuda12.0 libnvonnxparsers-dev=8.6.1.6-1+cuda12.0 libnvinfer-lean-dev=8.6.1.6-1+cuda12.0 \
libnvparsers-dev=8.6.1.6-1+cuda12.0 python3-libnvinfer-lean=8.6.1.6-1+cuda12.0 python3-libnvinfer-dispatch=8.6.1.6-1+cuda12.0 \
uff-converter-tf=8.6.1.6-1+cuda12.0 onnx-graphsurgeon=8.6.1.6-1+cuda12.0 libnvinfer-bin=8.6.1.6-1+cuda12.0 \
libnvinfer-dispatch-dev=8.6.1.6-1+cuda12.0 libnvinfer-dispatch8=8.6.1.6-1+cuda12.0 libnvonnxparsers-dev=8.6.1.6-1+cuda12.0 \
libnvonnxparsers8=8.6.1.6-1+cuda12.0 libnvinfer-vc-plugin-dev=8.6.1.6-1+cuda12.0 libnvinfer-samples=8.6.1.6-1+cuda12.0
```
## 6. Install librdkafka
```
cd ~/Documents/Dependencies/
git clone https://github.com/confluentinc/librdkafka.git
cd librdkafka
git checkout tags/v2.2.0
./configure --enable-ssl
make
sudo make install
```
## 7. Install Deepstream SDK 
### 7.1. Downlaod deepstream-7.0_7.0.0-1_amd64.deb from https://catalog.ngc.nvidia.com/orgs/nvidia/resources/deepstream/files?version=7.0 or use this command
```
cd ~/Documents/Dependencies/
sudo apt-get install curl
curl -Lo deepstream-7.0_7.0.0-1_amd64.deb 'https://api.ngc.nvidia.com/v2/resources/org/nvidia/deepstream/7.0/files?redirect=true&path=deepstream-7.0_7.0.0-1_amd64.deb'
```
## 7.2. Install Deepstream using dpkg
```
cd ~/Documents/Dependencies/
sudo apt-get install ./deepstream-7.0_7.0.0-1_amd64.deb
```
## 8. Copy the generated librdkafka libraries to the Deepstream directory
```
sudo mkdir -p /opt/nvidia/deepstream/deepstream/lib
sudo cp /usr/local/lib/librdkafka* /opt/nvidia/deepstream/deepstream/lib
sudo ldconfig
```
## 9. update rtspmanager
> Sometimes with RTSP streams the application gets stuck on reaching EOS. This is because of an issue in rtpjitterbuffer component. To fix this issue,a script “update_rtpmanager.sh” at /opt/nvidia/deepstream/deepstream/ has been provided with required details to update gstrtpmanager library. The script should be executed once above mentioned packages are installed as prerequisite.

```
cd ~/Documents/Dependencies/glib/build/
sudo ninja install
cd /opt/nvidia/deepstream/deepstream-7.0
sudo bash update_rtpmanager.sh
```
