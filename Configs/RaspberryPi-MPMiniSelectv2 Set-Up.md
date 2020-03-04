# OctoPi Set-up Using Raspberry Pi 3b / v2.1 Raspberry Pi Camera #

## Rasperry Pi Set-up ##
- Downloaded OctoPi image from: https://octoprint.org/download/
- Used Etcher to write image to MicroSD Card: https://www.balena.io/etcher/
- Then when it boots up you can either go to octopi.local in your browser, or ssh into the raspberry pi using the credentials username pi and password raspberry with the octopi.local being the address. Either way will let you finalize the set-up steps like set the time and define the printer you’re using.
- Install the Malyan-Monoprice Connection Add-in and enabled it (while in the octopi-local browser app settings). This makes the USB connection to the Monoprice mini more robust.
- In the browser, you should be able to get to "octopi.local" or "192.168.0.111" now. It's a dynamic IP, so that could change though. See below for how to find the IP if necessary.
- Instead of just pulling power cord, I often used the Shutdown button in the Octopi site. Via ssh in and run `sudo shutdown -h now` and then turn the power cord off.

**Additional Commands:**
- Restart OctoPrint: `sudo service octoprint restart`
- Restart system: `sudo shutdown -r now (or sudo reboot)`
- Shutdown system: `sudo shutdown -h now`

## MP Mini Select Motion Controller Update ##
- I updated the motion controller by making sure I could and then getting the files from here: https://www.mpselectmini.com/firmware/motion_controller Then it just required me to format the sdcard a certain way and then load two files on it and put it in. It updated really fast.		 

## Camera ##
- The camera I just connected like this one, paying attention to where the contacts and the blue were facing and it immediately worked without me doing anything: https://www.youtube.com/watch?v=PyGM4Iah0cM&vl=en
- The mount I used for the Prusa required flipping the image vertically, which was trivially easy under the Octopi settings.

## Remote Access ##
- **Power Control** - I wanted to be able to turn off the printer if something went wrong on the webcam so I put a smart outlet attached to it. Then I can use the alexa app to turn the printer off from anyplace. That did mean I had to leave the raspberry pi on 24/7.

- **Remote Control** - I could just port forward to get into my home network, but there are a bunch of security concerns with that. They're usually resolved using VPNs. To avoid that work, since I already used astroprint, I added the astroprint add-in to Octoprint. That instead allows interacting with octoprint and the printer via a secure Astroprint web site. After installing the add-in, I had to go into settings for astroprint and get my key to paste it into the octopi add-in.

## Finding IP Later ##
I didn't bother installing the stuff to manage the dynamic IP of the Octopi, so I just scanned my network for all devices connected running `arp -a` on the command line.
