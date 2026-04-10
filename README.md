
# Mazewar
This repo contains the files and instructions for creating and running Mazewar on a Raspberry PI. It was created for an exhibit of the
history of first person shooters at the Museum of Art and Digital Entertainment https://www.themade.org/. 
<img src="https://github.com/user-attachments/assets/af2aa256-2be0-450c-951e-0f2db72b2919" width="300" height="300"/>
<em>Credit: Veronica Sutter</em>

Mazewar runs the ITS operating system on a emulated Imlac Terminal and PDP-10 using the PiDP10/SimH emulator https://obsolescence.wixsite.com/obsolescence/pidp-11
![PXL_20260410_173936931](https://github.com/user-attachments/assets/84941c7b-171a-474b-a7f7-2c9f47cc5f8e)
This exhibit automates the startup of ITS and Mazewar and also creates 3 bots that wander the maze to provide targets to find and shoot. 

## To build from scratch:
Start with a Raspberry Pi 4 or better

Install 64 bit Raspberry Pi OS. Tested on Debian Bookworm based Raspberry Pi OS. Note - as of 1/7/26 Imlac does not work for me on Trixie unless you build it from source.

Follow instructions to install https://github.com/obsolescence/pidp10

Put mazewar_bot.py in the */home/pi* or edit the hardcoded paths in start_ITS.sh to point to where the python script is located

Install xdotool. *sudo apt install xdotool*

Install ncat *sudo apt install ncat*

Install the python telenetlib which the bots use. *sudo apt install python3-zombie-telnetlib* It’s labelled zombie as it was part of python but was removed. Telnet is not secure blah blah blah

Make the start_ITS script executable with *chmod +x start_ITS.sh*

Test start_ITS.sh by running it. *./start_ITS.sh*

While it’s running, it will make the Imlac terminal window active. Don’t change focus away from it, or it will likely fail. Wait until you’re in the maze before touching anything.

Make a user service directory *mkdir /home/pi/.config/systemd/user*

Copy Mazewar.service into this directory

Create the service and enable it:

*systemctl --user daemon-reload*

*systemctl --user enable Mazewar.service*

Start the service to test it. *systemctl --user restart Mazewar.service*

You should be running, and it should restart on boot.. If there is a problem, systemctl –user status Mazewar.service may give some hints.
If you’re going to be running this in an exhibit, you might unplug the mouse. There’s no need for it in the game
