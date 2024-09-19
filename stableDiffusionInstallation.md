# Install Stable Diffusion

*This procedure is verified under Ubuntu 20.04.6 LTS (GNU/Linux 5.4. 0-196-generic x86_64)*

## Install CUDA library

The required components are shown on Nvidia support matrix as below:

*CUDA Native Linux Support: Ubuntu 20.04.z (z <= 6) LTS; Kernel 5.15.0-67; Default GCC 9.4.0; GLIBC 2.31*

Noticed that Ubuntu 20.04.6 LTS Kernel is 5.4.0-196-generic, which is not aligned with supported requirements. It turns out this is not issue, whenever Kernel headers are matched.

### Verify CUDA GPU hardware

`sudo lspci | grep -i nvidia`

### Verify Linux version

`uname -m && cat /etc/*release`

### Verify GCC version
`gcc --version`

### Install CUDA Toolkit 12.6

1. Install kernel header and development package: `sudo apt-get install linux-headers-$(uname -r)`
2. `sudo apt -y install build-essential`
3. The NVIDIA CUDA Toolkit is available at https://developer.nvidia.com/cuda-downloads
4. Choose Linux, x86_64, Ubuntu, 20.04, deb (network)
5. Install the new cuda-keyring package. The new GPG public key for the CUDA repository must be enrolled on the system. `wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.1-1_all.deb` and run `sudo dpkg -i cuda-keyring_1.1-1_all.deb`
6. If CUDA has been installed before, use this command to delete old key `sudo apt-key del 7fa2af80`
7. `sudo apt-get update`
8. Install CUDA SDK `sudo apt-get -y install cuda-toolkit-12-6`
9. Install CUDA Driver 
   1. First remove any nvidia packages `sudo apt-get purge nvidia*`
   2. Add the repository to your repo list `sudo add-apt-repository ppa:graphics-drivers`
   3. Update, `sudo apt-get update`
   4. Detect driver `ubuntu-drivers devices`, and take the recommended driver. (*in my installation nvidia-driver-560 is recommended*)
   5. sudo apt-get install nvidia-driver-560
10. Reboot system `sudo reboot`

### Recover steps if graphic driver cannot be loaded
The below steps are described how to recover systesm if black screen after boot.

1. Boot into recovery mode on GNU GRUB
2. Select root (Drop to root shell prompt)
3. Remount with write permission: `mount -o rw,remount /`
4. Uninstall Nvidia graphics driver that caused problem
   1. `apt update`
   2. `apt search 'nvidia'`
   3. `apt remove '^cuda-drivers'`
   4. `apt autoremove`
   5. ` sudo systemctl set-default graphical.target` (*this is optional step, if ensure return to graphical interface*)
   6. `reboot`

### Post Instalaltion 
1. Enviorment setup, edit /etc/profile and add below lines:
    1. `export PATH=/usr/local/cuda-12.6/bin${PATH:+:${PATH}}`
    2. `export LD_LIBRARY_PATH=/usr/local/cuda-12.6/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}`
2. Verify Driver version `cat /proc/driver/nvidia/version`
3. Install CUDA samples 
   1. download `git clone https://github.com/NVIDIA/cuda-samples.git`
   2. build `make TARGET_ARCH=x86_64`
   3. find the target location of tool `find . -name 'deviceQ*' -print`
4. Verify device status : `./deviceQuery`
5. Verify device driver status: `nvidia-smi`
6. Bandwidth test: `./bandwidthTest`

## Install & upgrade pythyon to the latest version

[Deadsnakes PPA](https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa/) has made the packages for all supported CPU architecture types: amd64, arm64/armhf, ppc64el, and s390x.

1. update `sudo apt update && sudo apt upgrade -y`
2. add repository `sudo add-apt-repository ppa:deadsnakes/ppa`
3. `sudo apt update`
4. `sudo apt install python3.12`

## Install Stable Diffusion Model

This installation uses Stable Diffusion XL model.

Go to the [Stability AI image model](https://stability.ai/stable-image), select **Stable Diffusion XL** and [dowload Code](https://github.com/Stability-AI/generative-models), and select [SDXL-base-1.0](https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0), that will bring hugging face repository, and downnload [sd_xl_base_1.0_0.9vae](https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/blob/main/sd_xl_base_1.0_0.9vae.safetensors)

## Install Stable Diffusion UI

This installation uses [stable diffusion webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui) by automatic1111











