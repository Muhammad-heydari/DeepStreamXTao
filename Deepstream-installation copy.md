Deepstream7.0
prerequarment :
ubuntu 22

upgrade packages:
```
sudo apt update
sudo apt upgrade
sudo apt-get install ninja-build meson git
```
remove cuda and nvidia driver compeletly
```
sudo apt-get --purge remove "*cublas*" "cuda*" "nsight*" 
sudo apt-get --purge remove "*nvidia*"
sudo rm -rf /usr/local/cuda*
sudo apt-get install nvidia-driver-535
```
install glib
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
install dependencies
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
add key to repository
```
$ sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/3bf863cc.pub
$ sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/ /"
$ sudo apt-get update
```
install cuda
```
 sudo apt-get install cuda-toolkit-12-2
```
install tensorrt
```
sudo apt-get install --no-install-recommends libnvinfer-lean8=8.6.1.6-1+cuda12.0 libnvinfer-vc-plugin8=8.6.1.6-1+cuda12.0 \
libnvinfer-headers-dev=8.6.1.6-1+cuda12.0 libnvinfer-dev=8.6.1.6-1+cuda12.0 libnvinfer-headers-plugin-dev=8.6.1.6-1+cuda12.0 \
libnvinfer-plugin-dev=8.6.1.6-1+cuda12.0 libnvonnxparsers-dev=8.6.1.6-1+cuda12.0 libnvinfer-lean-dev=8.6.1.6-1+cuda12.0 \
libnvparsers-dev=8.6.1.6-1+cuda12.0 python3-libnvinfer-lean=8.6.1.6-1+cuda12.0 python3-libnvinfer-dispatch=8.6.1.6-1+cuda12.0 \
uff-converter-tf=8.6.1.6-1+cuda12.0 onnx-graphsurgeon=8.6.1.6-1+cuda12.0 libnvinfer-bin=8.6.1.6-1+cuda12.0 \
libnvinfer-dispatch-dev=8.6.1.6-1+cuda12.0 libnvinfer-dispatch8=8.6.1.6-1+cuda12.0 libnvonnxparsers-dev=8.6.1.6-1+cuda12.0 \
libnvonnxparsers8=8.6.1.6-1+cuda12.0 libnvinfer-vc-plugin-dev=8.6.1.6-1+cuda12.0 libnvinfer-samples=8.6.1.6-1+cuda12.0
```
install librdkafka
```
git clone https://github.com/confluentinc/librdkafka.git
cd librdkafka
git checkout tags/v2.2.0
./configure --enable-ssl
make
sudo make install
```
Downlaod deepstream-7.0_7.0.0-1_amd64.deb from https://catalog.ngc.nvidia.com/orgs/nvidia/resources/deepstream/files?version=7.0 or use this command
```
sudo apt-get install curl
curl -LO 'https://api.ngc.nvidia.com/v2/resources/org/nvidia/deepstream/7.0/files?redirect=true&path=deepstream-7.0_7.0.0-1_amd64.deb'
```
install deepstream
```
sudo apt-get install ./deepstream-7.0_7.0.0-1_amd64.deb
```
Copy the generated libraries to the deepstream directory
```
sudo mkdir -p /opt/nvidia/deepstream/deepstream/lib
sudo cp /usr/local/lib/librdkafka* /opt/nvidia/deepstream/deepstream/lib
sudo ldconfig
```
