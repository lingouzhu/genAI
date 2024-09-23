# Running Stable Diffusion on Low VRAM GPU

Stable Diffusion is a generative artificial intelligence (generative AI) model that produces photorealistic images from text and image prompts. If Stable Diffusion is installed on local computer, the recommended CUDA based Nvidia GPU requirement is at least 16GB VRAM.

In this article, I am going to setup Stable Diffusion XL on Nvidia GPU 1070 (8GB VRAM). And it works pretty well.

## TCMalloc

[TCMalloc](https://github.com/google/tcmalloc) is Google's customized implementation of C's malloc() and C++'s operator new used for memory allocation within our C and C++ code. TCMalloc is a fast, multi-threaded malloc implementation. TCMalloc provides several optimizations:

- Performs allocations from the operating system by managing specifically-sized chunks of memory (called “pages”). Having all of these chunks of memory the same size allows TCMalloc to simplify bookkeeping.
- Devoting separate pages (or runs of pages called “Spans” in TCMalloc) to specific object sizes. For example, all 16-byte objects are placed within a “Span” specifically allocated for objects of that size. Operations to get or release memory in such cases are much simpler.
- Holding memory in caches to speed up access of commonly-used objects. Holding such caches even after deallocation also helps avoid costly system calls if such memory is later re-allocated.

TCMalloc is helpful, because I will offload GPU load to CPU memory. TCMalloc improves memory usage. [A blog on Automatic1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/6722) has verified memory leak has been solved after switching to TCMalloc.

Use the following steps to install TCMalloc onto Ubuntu:

1. `sudo apt update`
2. `sudo apt-get install google-perftools`

## xFormers

The [XFormers library](https://github.com/facebookresearch/xformers) provides an optional method to accelerate image generation. This enhancement is exclusively available for NVIDIA GPUs, optimizing image generation and reducing VRAM usage. 

After xFormers is installed, the funciton enable_xformers_memory_efficient_attention() for faster inference and reduced memory consumption.

Since January 23, 2023, neither Windows nor Linux users are required to manually build the Xformers library in WebUI. WebUI was implemented to automaticlaly integrates xFormers into Torch and Torchvision.

To use xFormers in webUI, just need setup a command argument in the webui-user.sh (for Linux).

`export COMMANDLINE_ARGS="--xformers"`

## Offload GPU Memory Usage

Include `--medvram or --lowvram` to change Stable Diffusion's strategy. These options will make Stable Diffusion render slower, but avoid OOM errors.

`export COMMANDLINE_ARGS="--medvram`

## Estimate VRAM Consumption Before Run

The stable diffusion web UI extension [VRAM Estimator](https://github.com/space-nuko/a1111-stable-diffusion-webui-vram-estimator) gathers a set of statistics based on running txt2img and img2img with various different settings and uses extrapolation to estimate the amount of VRAM your settings will use.

1. Gather some data from running a bunch of image generations for the extension. Go to the VRAM Estimator tab and set the Max Image Size and Max Batch Count parameters to the maximum that your system can handle when generating with txt2img and Hires Fix enabled (Hires Fix uses more VRAM than plain txt2img even when the target resolution is the same).  Then click Run Benchmark and watch the console for progress.
2. After the benchmark has finished, you can go to the txt2img or img2img tab and adjust the sliders for Width/Height/Hires Fix to see the estimated VRAM usage.

To install the VRAM Estimator extension:

1. Clone repository into a directory. `git clone https://github.com/space-nuko/a1111-stable-diffusion-webui-vram-estimator`
2. copy the directory into the web UI's extension folder. `cp -rp ~/a1111-stable-diffusion-webui-vram-estimator/ ~/stable-diffusion-webui/extensions`