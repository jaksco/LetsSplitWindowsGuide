# A Guide to Flashing a Let's Split for Beginners (Windows)

This guide will be a walk through on how to set up and flash your let's split to get it up and running.

### 3 Main Steps

- **Setting Up** and Installing Software
- **Building** your keymap and keyboard firmware
- **Flashing** your Let's Split with your created firmware


### Helpful references

- [Flashing for Linux (_zsh's overly verbose guide has a good secion for this)](https://gist.github.com/nicinabox/3582fc89470a3f4efc9ed194f12fabfb)
- [/u/wootpatoot's Original Flashing Instructions](https://www.reddit.com/r/MechanicalKeyboards/comments/4w81ft/guidelets_split_flashing_instructions_windows/?ref=share&ref_source=link)

---

# SETTING UP

1) Install Bash Shell on Windows, follow [this guide](http://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/)

2) Download and install **AVRDudess**, [link here](http://blog.zakkemble.co.uk/avrdudess-a-gui-for-avrdude/)

3) Download and install **WinAVR or LIBUSB** to get the `LIBUSB0.DLL`
> * If you don�t then attempting to flash the pro micro won�t do anything     
> * I�m going to be using [WinAVR](http://winavr.sourceforge.net/index.html) for this, so download and install it

4) Go to install location of WinAVR and go to to the following path, using mine as an example 

**`C:\Program Files (x86)\WinAVR\utils\libusb\bin`**

![](http://i.imgur.com/0QiBvd0.png)

5) Copy the **4 `libusb0` files** from the WinAVR folder into the AVRDUDESS Program Files location and it should look like this now 

![](http://i.imgur.com/JPpaRmA.png)

6) **Clone with Git or Download the QMK Zip and unzip it** 

https://github.com/qmk/qmk_firmware 
> * For this guide I�m going to make it easy and just download the Zip 

![](http://i.imgur.com/v3sh3XQ.png)
      
7) **Extract the zip** so you have a folder named **QMK or qmk_firmware_master**
> * Place this in a nice easy location, that doesn�t require administrator access, like your documents folder or desktop

8) **Open a bash shell window** through the start menu (should be called bash.exe or Bash on Ubuntu on Windows)

9) **Change the directory to your QMK copy**, which you just unzipped.

![](http://i.imgur.com/z7NG8W0.png)

* Do this using the `cd` command (Hard drives are accessed in bash by typing `cd /mnt/<driveletter>`)

Example `cd /mnt/c/Users/Nick/Documents/qmk_firmware_master`

10) Run **`sudo util/install_dependencies.sh`**, This will run `apt-get upgrade`

11) Wait a while (5 minutes for me), and then you are good to go


---

# BUILDING HEX

## For the default keymap, with no customization

1) Change the directory to your Lets_Split folder 

![](http://i.imgur.com/C4omjSV.png)

* Do this using the **`cd`** command 

Hard drives are accessed from `cd /mnt/<driveletter>`

Example `cd /mnt/c/Users/Nick/Documents/qmk_firmware_master/keyboards/lets_split`

2) Type **`make`** and it will build all of the hex�s that are available by default (Serial and i2c, for both Rev 1 & Rev 2) 

![](http://i.imgur.com/1Kz4znI.png)

3) Your hex file and a folder will be generated for you at the following location **`qmk_firmware_master/.build`**	 	

> * This folder will be generated and your appropriate hex files will be inside, all you need is your .hex file, so copy that somewhere safe, or you can just drag it in the let�s split folder in QMK 

![](http://i.imgur.com/DaOXSil.png)

## For customization

1) **Change the directory to your Lets_Split folder**

![](http://i.imgur.com/C4omjSV.png)

* Do this using the **`cd`** command 

Hard drives are accessed from `cd /mnt/<driveletter>`

Example `cd /mnt/c/Users/Nick/Documents/qmk_firmware_master/keyboards/lets_split`

2) Customize and configure your files in the Rev 2 folder (keymap.c for layout, but check QMK wiki on GitHub for more advanced)

> * Only if using Rev 2 PCB, **if you are using Rev 1 just do everything in Rev 1 folder**, and type **`make rev1`**

* For me, I had to reverse the right side, because it was flipped, read the instructions for that above, at the end of the _zsh's guide, he goes over how to fix this. I�m not going to go into customization in this guide, so just check the QMK Wiki

3) Type **`make rev2`**

4) Your hex file and a folder will be generated for you at the following location **`qmk_firmware_master/.build`**

> * This folder will be generated and your appropriate hex files will be inside, all you need is your .hex file, so copy that somewhere safe, or you can just drag it in the let�s split folder in QMK 

![](http://i.imgur.com/DaOXSil.png)

---

# FLASHING (Not the one that gets you in trouble)

1) Open up AVRDUDESS

> * If you get an error saying that it can�t start because libusb0.dll is missing then **read step 2 of the SETUP GUIDE** above

2) Select **Atmel AppNote AVR109 Boot Loader** for the **programmer**

![](http://i.imgur.com/ks0bPdL.png)

3) Select **ATmega32U4** for the **MCU** 

![](http://i.imgur.com/wefkngw.png)

4) Select your .hex file in the for the flash box (click the � box to the right)

![](http://i.imgur.com/ZR0UON1.png)

5) Select the **`eeprom-lefthand.eep`** file in the QMK Lets_split folder for the **EEPROM box** 

> * Make sure to select only one file, as you will have to change this file when flashing the right side

6) Connect **only the left side** of your board to the computer through a USB cable

7) Take note of the port drop down menu and see if you have any COMM listings, if you do, then Don�t select those particular ones in the next step 

![](http://i.imgur.com/b55RQa5.png)

8) Locate the **ground and reset** pins on your pro micro and short them, was 2 from the top on the right side of the pro micro for me

![](http://www.14core.com/wp-content/uploads/2015/12/Reset-Button-Place-Micro-Mini-Pro.jpg)

9) Take something metal and touch between the **ground and reset** pins to short them out, or if your�s has a reset button then press that to enter the bootloader mode

10) Quickly open the Port drop down menu and select the new **COMM** port that has popped up, then click program 

![](http://i.imgur.com/uYK8bpd.png)

> * If the pro micro came out of bootloader mode, then you won�t be able to flash it, but all you have to do is enter bootloader mode again by shorting the pins again.

11) **Now the left side is done, we have to flash the right side**, so connect it to your PC through USB 

12) **Be sure to change the EEPROM file to the right hand one (eeprom-righthand.eep)**

13) Locate the ground and reset pins on your pro micro and short them, was 3 from the top on the right side of the pro micro for me

14) Take something metal and touch between the ground and reset pins to short them out, or if your�s has a reset button then press that to enter the bootloader mode

15) Quickly open the Port drop down menu and select the new COMM port that has popped up, then click program 

> * If the pro micro came out of bootloader mode, then you won�t be able to flash it, but all you have to do is enter bootloader mode again by shorting the pins again.

16) Time to test it out, plug in your TRRS cable to both sides, and then plug in the USB on the left side and open a notepad window to test out all the keys and make sure your soldering skills were good

17) **Congrats on correctly flashing your own Let�s Split, and hope this helps those out who are having trouble with programming their Let�s Splits like I did**

---

**-Written by /u/CampAsAChamp**

Help and references from

- /u/wootpatoot

- /u/_zsh

- /u/jackhumbert

- /u/Jolly_Green_Giant