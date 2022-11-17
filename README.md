# UnstableFusion
A Stable Diffusion desktop frontend with inpainting, img2img and more!

https://user-images.githubusercontent.com/6392321/191858568-0550f52d-e89c-4b37-aa07-23df605b4807.mp4

## Custom doc

### Usage
- enter a prompt 
- in the square with transparent texture click and scroll the mouse wheel to determine image size.

#### Troubleshooting
If nothing happens when clicking on the generate button you should take a look at the output 

Problem I encountered :

##### Access to model is restricted and you are not in the authorized list.
```shell
Access to model runwayml/stable-diffusion-v1-5 is restricted and you are not in the authorized list. Visit https://huggingface.co/runwayml/stable-diffusion-v1-5 to ask for access.
```
Easy fix => go to url and request access


### Step-by-step installation to first-use (ubuntu 20) 

#### Install pip3 if not present
```shell
sudo apt update
sudo apt upgrade -y
sudo apt install python3-pip -y
sudo pip install --upgrade pip
```

Close and reopen your shell

#### Check executable
```shell
pip -V
pip 22.3.1 from /usr/local/lib/python3.8/dist-packages/pip (python 3.8)
```

#### Install latest dependencies

> Note : small diff from doc `pytorch` => `torch`

``` shell
python3 -m pip install --upgrade PyQt5 numpy torch Pillow opencv-python requests flask diffusers transformers protobuf qasync httpx
```

#### Try to run
```
git clone REPO_URL (i encourage you to fork)
cd LOCAL_REPO_PATH
python3 unstablefusion.py
```

If it work congratulations !
Else here are the 3 troubleshooting I encountered


##### Troubleshooting 1 (linux `Could not load the Qt platform plugin "xcb"`) :
 error, run this:
```
pip uninstall opencv-python     (solve a xcb compatibility issue)
pip install opencv-python-headless     (solve a xcb compatibility issue)
```

Close shell and retry

You may have a similar looking error but a bit more precise, see next step

##### Troubleshooting 2 (linux `Could not load the Qt platform plugin "xcb" in "" even though it was found.`)
I needed to install one more dependency

```
sudo apt-get install libxcb-xinerama0
```

##### Troubleshooting 3 (linux under WSL) The problem may be that you do not yet have a way to run GUI apps.
Follow https://techcommunity.microsoft.com/t5/windows-dev-appconsult/running-wsl-gui-apps-on-windows-10/ba-p/1493242 to install and configure VcXsrv Windows X Server.

###### Follow the tutorial (option 1) to install  VcXsrv Windows X Server
- Follow the configuration as described

- After running and configuring you should see the server running in your taskbar

![image](https://user-images.githubusercontent.com/1665550/202497111-deb254a7-1b66-44c7-87cf-8376f979a243.png)


###### Add in the shell config (~/.bashrc | ~/.zshrc ) the DISPLAY var

```vim
# DISPLAY var for vcXsrv
export DISPLAY=$(ip route|awk '/^default/{print $3}'):0.0
```

###### Add a ~/.xsession file in home
```
echo xfce4-session > ~/.xsession
```

Close and reopen your shell and the following command should give you your host machine ip

``` shell
echo $DISPLAY
> 172.27.117.1:0.0 (may be different)
```

###### you can easily check that the server works by launching small gui app such as xclock
```
sudo apt-get install x11-apps
xclock
```

##### Run the GUI
```
cd LOCAL_REPO_PATH
python3 unstablefusion.py
```

![image](https://user-images.githubusercontent.com/1665550/202498582-65ad47b4-a4a5-4d14-ab3c-144185915c52.png)

Success ! :D

## Original Documentation

## How to run locally?
1. Install the dependencies (for example using `pip`). The dependencies include :
* `PyQt5`, `numpy`, `pytorch`, `Pillow`, `opencv-python`, `requests`, `flask`, `diffusers`, `transformers`, `protobuf`

Note that if you want to run StableDiffusion on Windows locally, use requirements-localgpu-win64.txt
```
pip install -r requirements-localgpu-win64.txt
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
