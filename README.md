# UnstableFusion
A Stable Diffusion desktop frontend with inpainting, img2img and more!

https://user-images.githubusercontent.com/6392321/191858568-0550f52d-e89c-4b37-aa07-23df605b4807.mp4

## Step-by-step installation to first-use (ubuntu 20) 

### Install pip3 if not present
```shell
sudo apt update
sudo apt upgrade -y
sudo apt install python3-pip -y
```

### Check executable
```shell
pip -V
> pip 20.0.2 from /usr/lib/python3/dist-packages/pip (python 3.8)
```

### Install dependencies

> Note : small diff from doc `pytorch` => `torch`

``` shell
 pip install PyQt5 numpy torch Pillow opencv-python requests flask diffusers transformers protobuf
```

### Install HuggingFace hub

```
pip install huggingface_hub
```





## How to run locally?
1. Install the dependencies (for example using `pip`). The dependencies include :
* `PyQt5`, `numpy`, `pytorch`, `Pillow`, `opencv-python`, `requests`, `flask`, `diffusers`, `transformers`, `protobuf`

Note that if you want to run StableDiffusion on Windows locally, use requirements-localgpu-win64.txt
```
pip install -r requirements-localgpu-win64.txt
```

Note: On linux, if you encounter `Could not load the Qt platform plugin "xcb"` error, run this:
```
pip uninstall opencv-python     (solve a xcb compatibility issue)
pip install opencv-python-headless     (solve a xcb compatibility issue)
```

2. Create a huggingface account and an [access token](https://huggingface.co/settings/tokens), if you haven't done so already.
Request access to the StableDiffusion model at [CompVis/stable-diffusion-v1-4](https://huggingface.co/CompVis/stable-diffusion-v1-4).

3. Clone this repository and run `python unstablefusion.py`

## How to run using colab servers?

1. Install the dependencies (see the previous section)

2. Open [this notebook](https://colab.research.google.com/github/ahrm/UnstableFusion/blob/main/UnstableFusionServer.ipynb) and run it (you need to enter your huggingface token when asked).

3. When you run the last cell, you will be given a url like this:

![colab_output - Copy](https://user-images.githubusercontent.com/6392321/191857962-0601fa88-7f03-46cf-98a0-c38cb3fd25b9.png)
copy this URL.

4. Run `python unstablefusion.py`

5. In the runtime section, select server and enter the address you copied in the server field. Like this:

![using_server - Copy](https://user-images.githubusercontent.com/6392321/191858215-9db78f1a-50f8-4394-8bd4-fc14d981561b.png)

## How to use?

* You can select a box by clicking on the screen. All of your operations will be limited to this box. You can resize the box using mouse wheel.
* You can erase the selected box by right clicking, or paint into it by middle clicking (the paint color can be configured using `Select Color` button)
* To generate an image, select the destination box, enter the prompt in the `prompt` text field and press the `Generate button` (inpainting and reimagining work similarly)
* You can undo/redo by pressing the `undo`/`redo` button or pressing `Control+Z`/`Control+Shift+Z` on your keyboard. In fact most other functions are bound to keys as well (you can configure them in `keys.json` file)
* `Increase Size`/`Decrease Size` buttons adjust the size of the image by adding/removing extra space in the margins (and not by scaling, this is useful when you want to add more detail around an image)
* You can open a scratchpad by pressing `Show Scratchpad` button. This window is capable of doing everything the main window can (using keyboard shortcuts only). The selected box in scratch pad will be mirrored and scaled into the selected box in the main window.
This is useful when trying to import another generated/local image into the main image.

### How to use advanced inpainting?

Admittedly, the UI for advanced inpainting is a little unintuitive.
Here is how to works:

1. You clear the part of the image that you want to inpaint (just like normal inpainting)
2. Select the target box (again, like normal inpainting), but instead of clicking on the inpaint button, click on `Save Mask` button. From now on, the current mask and current selected box will be used for inpainting no matter how you change the box/image (until you press `Forget Mask` button)
3. This means that you are free to edit the initial image as you please using other operations. For example, you can autofill the masked area using `Autofill` button, or manually paint the target area or paste any image from scratchpad. Since this initial image will be used to initialize the masked part, it will heavily affect the final result. Therefore by controlling this initial image, you can modify the final result to your will.
