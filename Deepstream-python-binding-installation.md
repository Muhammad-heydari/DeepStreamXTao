https://github.com/NVIDIA-AI-IOT/deepstream_python_apps/tree/master/bindings
```
sudo apt install python3-gi python3-dev python3-gst-1.0 python-gi-dev git meson \
    python3 python3-pip python3.10-dev cmake g++ build-essential libglib2.0-dev \
    libglib2.0-dev-bin libgstreamer1.0-dev libtool m4 autoconf automake libgirepository1.0-dev libcairo2-dev
```
Initialization of submodules
```
cd /opt/nvidia/deepstream/deepstream-7.0/sources/
sudo chmod 777 -R .
git clone https://github.com/NVIDIA-AI-IOT/deepstream_python_apps
cd /opt/nvidia/deepstream/deepstream/sources/deepstream_python_apps/
git checkout v1.1.11
git submodule update --init
```
Installing Gst-python
```
sudo apt-get install -y apt-transport-https ca-certificates -y
sudo update-ca-certificates
```
Build and install gst-python:
```
cd 3rdparty/gstreamer/subprojects/gst-python/
meson setup build
cd build
ninja
ninja install
```
Compiling the bindings
```
cd /opt/nvidia/deepstream/deepstream/sources/deepstream_python_apps/bindings
mkdir build
cd build
cmake .. -DS_VERSION=7.0 -DPYTHON_MAJOR_VERSION=3 -DPYTHON_MINOR_VERSION=10 
make -j$(nproc)
```
create python envierment and install ds
```
cd ~/Documents
python3.10 -m venv deepstream-venv
source deepstream-venv/bin/activate
pip3 install ./pyds-1.1.11-py3-none*.whl
pip3 install cuda-python
```
pip install --upgrade pip
pip3 install ./pyds-1.1.11-py3-none*.whl
```
