# Install Stable Diffusion

This procedure is verified:

- Ubuntu 22.04.5 LTS (Linux Kernel 5.15.0-122-generic x86_64)
- GCC gcc (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0
- GLIBC (GLIBC 2.35-0ubuntu3.8) 2.35
- Python 3.10.12

Note: *This procedure was verfied on Ubuntu 20.04.6 LTS (GNU/Linux 5.4. 0-196-generic x86_64) and not working. The resaon is bcause core python of  Ubuntu 20.04 is 3.8. Some modules are not compiled under python3.8.*

## Install CUDA library

Refer to [NVIDA CUDA Instalaltion Guide for Linux](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/)

The required components are shown on Nvidia support matrix: *CUDA Native Linux Support: Ubuntu 22.04.z (z <= 4) LTS; Kernel 6.5.0-27; Default GCC 12.3.0; GLIBC 2.35*

Noticed that Ubuntu 22.04.5 LTS Kernel is 5.15, which is not aligned with supported requirements. It turns out this is not issue, whenever Kernel headers are matched.

### Verify CUDA GPU hardware

`sudo lspci | grep -i nvidia`

### Verify Linux version

`uname -m && cat /etc/*release`

### Verify GCC version

`gcc --version`

### Verify GLIBC version

`ldd --version`

### Install CUDA Toolkit 12.6

1. Install kernel header and development package: `sudo apt-get install linux-headers-$(uname -r)`
2. `sudo apt -y install build-essential`
3. Download [NVIDIA CUDA Toolkit](https://developer.nvidia.com/cuda-downloads)
   1. Choose Linux, x86_64, Ubuntu, 20.04, deb (network)
4. Install the new cuda-keyring package. The new GPG public key for the CUDA repository must be enrolled on the system. `wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.1-1_all.deb` and run `sudo dpkg -i cuda-keyring_1.1-1_all.deb`
5. If CUDA has been installed before, use this command to delete old key `sudo apt-key del 7fa2af80`
6. `sudo apt-get update`
7. Install CUDA SDK `sudo apt-get -y install cuda-toolkit-12-6`
8. Install CUDA Driver
   1. First remove any nvidia packages `sudo apt-get purge nvidia*`
   2. Add the repository to your repo list `sudo add-apt-repository ppa:graphics-drivers`
   3. Update, `sudo apt-get update`
   4. Detect driver `ubuntu-drivers devices`, and take the recommended driver. (*in my installation nvidia-driver-560 is recommended*)
   5. sudo apt-get install nvidia-driver-560
9. Reboot system `sudo reboot`

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
   5. `sudo systemctl set-default graphical.target` (*this is optional step, if ensure return to graphical interface*)
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

## Setup pythyon

Ubuntu 22.04.5 LTS already have python3.10 installed, use the following command to ensure 3.10 is default python.

1. Register all installed Python versions, where the last parameter indicates priority. For example: `sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.10 50`
2. Select python3.10 as default. `sudo update-alternatives --config python`
3. Verify python version, 3.10.x is expected. `python --version`

For reference, the below procedure provides ways to install additional python. The repository is from [Deadsnakes PPA](https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa/). Deadsnakes has made the packages for all supported CPU architecture types: amd64, arm64/armhf, ppc64el, and s390x.

1. update `sudo apt update && sudo apt upgrade -y`
2. add repository `sudo add-apt-repository ppa:deadsnakes/ppa`
3. `sudo apt update`
4. `sudo apt install python3.12`

## Install Stable Diffusion Model

This installation uses Stable Diffusion XL model.

Go to the [Stability AI image model](https://stability.ai/stable-image), select **Stable Diffusion XL** and [dowload Code](https://github.com/Stability-AI/generative-models), and select [SDXL-base-1.0](https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0), that will bring hugging face repository, and downnload [sd_xl_base_1.0_0.9vae](https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/blob/main/sd_xl_base_1.0_0.9vae.safetensors)

## Install Stable Diffusion UI

This installation uses [stable diffusion webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui) by automatic1111

1. Clone web ui reponsitory into a directory `git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git`
2. Install required libraries:
   1. `sudo apt update`
   2. `sudo apt install wget git python3 python3-venv libgl1 libglib2.0-0`
3. Launch webui.sh to trigger the remaining installation. `./webui.sh`

Noticed the following conditions will be checked:

- pip. pip will be upgraded. (*in my case, it is pip-24.2*)
- glibc, (*in my case, it is 2.35*)
- GCC, (*in my case, it is 11.4.0*)
- Python, (*in my case, it is 3.10.2*)

The following packages will be installed:

- pytorch, (*in my case, it is 2.1.2*)
- torchvision, (*in my case, it is 0.16.2*)
- filelock, (*in my case, it is 3.16.1*)
- typing-extensions, (*in my case, it is 4.12.2*)
- sympy, (*in my case, it is 1.13.3*)
- networkx, (*in my case, it is 3.3*)
- jinja2, (*in my case, it is 3.1.4*)
- fsspec, (*in my case, it is 2024.9.0*)
- triton, (*in my case, it is 2.1.0-0*)
- numpy, (*in my case, it is 2.1.1*)
- requests, (*in my case, it is 2.32.3*)
- pillow, (*in my case, it is 10.4.0*)
- MarkupSafe, (*in my case, it is 2.1.5*)
- charset-normalizer, (*in my case, it is 3.3.2*)
- idna, (*in my case, it is 3.10*)
- urllib3, (*in my case, it is 2.2.3*)
- certifi, (*in my case, it is 2024.8.30*)
- mpmath, (*in my case, it is 1.3.0*)
- stable-diffusion model, (*in my case, it is v1-5-pruned-emaonly.safetensors*)

## Load Stable Diffusion XL model

After web UI is running sucessfully, install the XL model further:

1. Exit web UI application
2. Copy previous downloaded `sd_xl_base_1.0_0.9vae.safetensors` to `~/stable-diffusion-webui/models/Stable-diffusion` folder
3. Lanuch web UI application
4. Select XL model from `Stable Diffusion checkpoint` field on web UI

## Share Web UI across network

Web UI application arguments are managed by COMMANDLINE_ARGS in the webui-user.sh.

Web UI provides 2 ways of sharing:

1. Share across over private LAN: `--listen`, optionally `--port <port value>`
2. Share across over internet: `--share --gradio-auth <username>:<password>`. After web UI lanuched, it will display a URL like `https//xxxx.gradio.live` on console log. (*Notice this URL lives up to 72 hours*)
