# Introduction

This is a fork of Rishi Thakkar's (Github: Atrifex) ECE385-HelperTools repository. It includes an extra script that can potentionally be useful to some users when doing their final project for the course. My partner and I used it heavily so thought I would share! All credit goes to the original author for writing the tools that do much of the heavy lifting.

Now, the new script is called **gif\_palette\_breakdown.py** and it is a modification of an already included script **png\_to\_palette\_relative\_resizer.py**. The purpose of this script is to allow you to generate step sized increments to your image to give the appearance that the object is coming towards you on screen. In general, it allows you to present a 3D feel to your game. This script works great for gifs such as the one we used, shown below. You can break the gif you want to use through an online gif decomposer. These are very easily available with a simple google search. This will give you a bunch of image files that you can then change to a .png format. With those images, a starting size and an ending size in mind, you should be ready to roll.


## How To Use: Step-by-Step Instructions


To prepare the script, you can follow the palette based approach mentioned in the original repository. The synopsis is provided below.

-   Generating the images from the gif:
    1.  Go to an online gif decomposer and decompose your gif into separate images saved as .png files
    2. Place the images into the sprite_originals folder
-   Generating the Palette:
	 1. Go to <http://www.piskelapp.com> and click on create a sprite
    2.  Import an image and look at the bottom right corner of the screen. There you will see a list of colors that appear in your image.
    3.  Make a list (record RGB values) of important colors that appear in the image.
        -   The more accuracy you want, the more colors you should include.
        -   In my opinion, you should try to gather the RBG values of the colors that look visually unique to you.
    4.  Since all your frame images are pieces of the same gif, you do not need to repeat steps 2 and 3 for all of your images. Your palette will be the same.
    5.  Take this list and place it within `palette_hex` as hex values in the format of 0xRGB
        -   Example `palette_hex = ['0xFF0000' , '0x047cc0', '0x14b8eb', '0x42fdff', '0x37dcff']`
-   Running the Scripts:
    1.  Open up cmd or a terminal and then navigate to the directory that contains the python `scripts` and all 3 `sprites_*` folders. 
    2.  Execute the python script by typing: `python scripts/gif_palette_breakdown.py`
    4.  When this script is running, it will not ask you to enter any information. All of the parameters should be set in the code prior to running. The script is well commented to let you know what you can/should change.
    5.  Let the script run - might take some time for very large images or number of iterations. The script will report the progress on the terminal.
    6.  Once the script has finished, go to the `sprites_converted` folder and check if the output images match what you are hoping for.
    7.  If they are correct, then the `*.txt` output from `sprites_bytes` should also be correct. This is what you care about essentially.

The following is a code snippet to help you read the textfile you generated and use it in SystemVerilog. Let's pretend your original image for the first frame of the gif is 64*64. The sprite\_bytes text file that is generated holds the index into your palette for each pixel's supposed color. You need to create an array, that is the size of your image, to hold each pixel's index. You need to create a local array of your palette in your file as well. Typically, the file will be **color_mapper.sv**. Then use the snippet below to read from the text file into your created array. Now you will have the file loaded onto On Chip Memory so that you can draw the image onto the VGA display using DrawX and DrawY.


> Initial begin:
>
> $readmemh("/path/to/sprite\_bytes/text/file/", array\_name)
> 
> end


## Script Results

This is the gif that we used for our game. 

![](https://thumbs.gfycat.com/UnacceptableNeglectedGuillemot-max-1mb.gif)

We used an online gif decomposer to break it down into the following 7 frames. Note we had to get rid of the white background and make it transparent manually. The images are as follows.

![alt text](https://github.com/hkmodhera/ECE385-HelperTools/blob/master/PNG%20To%20Hex/On-Chip%20Memory/sprite_originals/collageBowser.png)

Based on our current configuration for the script, here are example converted images. These are the first 5 varying size images of the first frame of the gif, starting from the original image on the far left.

![alt text](https://github.com/hkmodhera/ECE385-HelperTools/blob/master/PNG%20To%20Hex/On-Chip%20Memory/sprite_converted/frames21_conv/bowserT_f1_96_128.png)
![alt text](https://github.com/hkmodhera/ECE385-HelperTools/blob/master/PNG%20To%20Hex/On-Chip%20Memory/sprite_converted/frames21_conv/bowserT_f1_106_141.png)
![alt text](https://github.com/hkmodhera/ECE385-HelperTools/blob/master/PNG%20To%20Hex/On-Chip%20Memory/sprite_converted/frames21_conv/bowserT_f1_116_154.png)
![alt text](https://github.com/hkmodhera/ECE385-HelperTools/blob/master/PNG%20To%20Hex/On-Chip%20Memory/sprite_converted/frames21_conv/bowserT_f1_126_167.png)
![alt text](https://github.com/hkmodhera/ECE385-HelperTools/blob/master/PNG%20To%20Hex/On-Chip%20Memory/sprite_converted/frames21_conv/bowserT_f1_136_180.png)

Note that although the above images looked stretched, they look perfect on the VGA monitor. Try and see for yourself!

# FAQ

1.  What is PIL and why is it missing?
    -   PIL is the Python Imaging Library and it is missing because you haven't downloaded it.

