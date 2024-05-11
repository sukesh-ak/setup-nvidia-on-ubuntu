# How to setup nvidia drivers, cuda, cudnn and Nvidia docker container toolkit on ubuntu
Install &amp; Setup Nvidia drivers, cuda, cudnn and Nvidia docker container toolkit on Ubuntu.  
_Please keep in mind this was setup for nVidia RTX 3080/4060.   
If your GPU model is different you need to get the correct commands from the link provided on each step_

## Pre-requisite
1. Install Ubuntu Desktop (or Server)
2. Install Docker
```bash
curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh get-docker.sh && sudo usermod -aG docker $USER
```

## 3. Install Cuda Toolkit
Link to installer is [here](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04&target_type=runfile_local)

```bash
$ wget https://developer.download.nvidia.com/compute/cuda/12.4.1/local_installers/cuda_12.4.1_550.54.15_linux.run
$ sudo sh ./cuda_12.4.1_550.54.15_linux.run
```
#### Install just the drivers (instructions from above link)
```bash
$ sudo sh ./cuda_12.4.1_550.54.15_linux.run --silent --driver
```

## 4. Install CUDNN
Link to installer is [here](https://developer.nvidia.com/cudnn-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04&target_type=deb_network)

```bash
$ wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb
$ sudo dpkg -i cuda-keyring_1.1-1_all.deb
$ sudo apt-get update
$ sudo apt-get -y install cudnn
```

## 5. Install nVidia Container Toolkit
Link to installer is [here](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#configuring-docker)

```bash
$ curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
$ sudo apt-get update
$ sudo apt-get install -y nvidia-container-toolkit	
```

### Configure docker with nvidia container toolkit
```bash
$ sudo nvidia-ctk runtime configure --runtime=docker
$ sudo systemctl restart docker
```
