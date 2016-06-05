# Ubuntu and Windows Desktop Configurations #
## New Ubuntu **DESKTOP** Computer Configuration ##
1. **File Structure** - Adjust home directory file structure. Renaming and moving the folders to other's automatically updates the folder explorer's links on the left, but you can manually adjust the links using `~/.config/user-dirs.dirs`. This files allows the user to specify a custom path for the special folders. So you can have your Music folder in, say, "$HOME/My Music", or "/datapartition/mp3". I renamed "Documents" to "Projects". I also moved "Pictures" to within "Projects". If you delete a standard home directory folder, like "Documents", "Music", etc.  Ubuntu will automatically recreate them on login unless you change "enabled" to "=false" in `/etc/xdg/user-dirs.conf`. All those default created folders are set-up in `/etc/xdg/user-dirs.defaults`. Move "Templates" to within "Projects". Anything in the Templates folder becomes a template that can be used to create files using the right click > New Document. Delete the "Examples" directory in your home directory. It's just a remnant of the Ubuntu demo. I hate thumbnails, so to make the file explorer (Nautilus) show just file lists as default, open the file explorer then on the top-left of the banner click Files>Preferences and change "View new folders using" to "List View."
2. **Remove Guest Session** - Just makes things cleaner by using `sudo nano /etc/lightdm/lightdm.conf` and adding the line `allow-guest=false` to the end. Restart. This is just there to let people play with Ubuntu in labs or something. Not needed.
3. **Remove Superfluous Programs** - Remove all unneeded programs using Ubuntu Software Center: Ubuntu One, all games, Simple Scan, Thunderbird, Empathy Internet Messaging, Orca Screen Reader, Onboard, Amazon, finally Firefox. After removing firefox, remove .mozilla, .macromedia, and .etc from your home direction and `/etc/firefox` , `/usr/lib/firefox`, and `/usr/lib/firefox-addons/` manually to completely clean it out. There are more but they aren't worth it (you can find them with `locate firefox`). Also, search for "Privacy" and turn off the "return online search results" option. This stops the amazon searches from being returned on your start bar search.
4. **Chromium** - Install Chromium using Ubuntu Software Center. Login and sync Extensions and bookmarks. Go ahead and install Chrome if required for Netflix (Chromium doesn't use the non-open-source video codec's required for Netflix).
5. **Tweak Style and Background** -  I hate thumbnails, so to make the file explorer (Nautilus) show it as default, open the file explorer then on the top-left of the banner click Files>Preferences and change "View new folders using" to "List View." Under "Appearance" change the launcher icon size to the smallest possible. That's the same place you can change the background, but the picture must be in your "Picture" directory, or whatever you specified as your "Picture" directory earlier.
6. **Ctrl Swap** - Until Ubuntu 13.10, Ubuntu exposed a simple checkbox in settings to do this (They might have since added it back, so try original method below first). To get it to work after v13.10, I had to install Gnome Tweak Tool. If you have to use the tweak tool you might as well use the dark dorian theme. Just copy it off my hard drive and put it in a .ssh hidden directory in my user directory.

#### Original Method: ####
This was my Swap Caps Lock w/ Ctrl by going to Keyboard Layout > Options > "Ctrl key position" and then scrolling down to check "Swap Ctrl Key and Caps Lock."

THIS MAY OR MAY NOT BE NECESSARY DEPENDING ON MAC OR PC KEYBOARD: Also swap the Windows/Command key with Alt, so that hitting Alt is easier. Alt is the meta key and is used a lot in emacs. Under Keyboard Layout > Options > Alt/Win Key Behavior check "Left Alt is swapped with left Win." 

7. **SSH Config** for quick connecting using keys as opposed to password and not having to trpe anything besides `ssh aws` - reference SSH notes in [setup repo](https://github.com/dfeagans/setup).
8. **[PS3MediaServer](https://help.ubuntu.com/community/Ps3MediaServer)** - use the commands as follows to install:
```bash
sudo add-apt-repository ppa:happy-neko/ps3mediaserver
sudo apt-get update
sudo apt-get install ps3mediaserver
```
- To get it to automatically start running on startup: System --> Preferences --> Startup Applications --> Add. The command is `ps3mediaserver` or you can navigate to the program in /usr/share/applications. `which ps3mediaserver` actually returns /usr/bin/ps3mediaserver though.
- Since I'm just streaming to my TV (through the PS3), change the sound to just 2 channel Stereo to save bandwidth.
- The default bandwidth was set at 110, I changed it to 0 and it stuttered. Setting it to 4 made it work like a champ. On the video tutorial, they recommend around 15. The second time around, I just left it on 110.

9. **Emacs and Dotfiles for Terminal Configuration** - Install the daily build of emacs using this process (check the PPA might have changed location):
```bash
sudo apt-add-repository -y ppa:ubuntu-elisp/ppa
sudo apt-get -qq update
sudo apt-get install -y emacs24-nox emacs24-el emacs24-common-non-dfsg
```
- To grab all my dotfiles, wget them from my github repository, and then follow the symlinking instructions in the setup repository for the ones I want. Comment out the lines of the .bash_profile and .bashrc I don't need on the desktop, non-server computer. 
- To help identify if I'm ssh'd into my server or not, change the prompt to ODIN and make it red by replacing the PS1 line `(PS1="\[\033[1;32m\][ODIN:\w]$\[\033[0m\]\n")` in the .bashrc with: `PS1="\[\033[1;31m\][THOR:\w]$\[\033[0m\]\n"`

- The final part is to add `export TERM=xterm-256color` to the end of the .bashrc to get my markdown-mode color formatting to work right.

10. **KeyBindings** at the terminal use the below to get universal emacs keybindings: `gsettings set org.gnome.desktop.interface gtk-key-theme "Emacs"`

It just needs to be run once, will stay even after restart. To set it back to normal use: `gsettings set org.gnome.desktop.interface gtk-key-theme "Default"`

11. **Terminal Customizations** (/etc/skel/.bashrc is a backup of the stock Ubuntu one)
This should all be in my dotfiles, but adding `alias open='gnome-open'` to your .bashrc allows you to type "open ." to explore the current directory (xdg-open and gvfs-open are alternatives. I think gvfs-open is standard).

12. **Terminal Color Customizations**
Open terminal and do edit > profiles and add another profile. Set Foreground and Background scheme to "grey on black" and the Palette scheme to "Linux Console". It seems the least offensive and dark. Make the window a bit transparent. Finally, set the font to Ubuntu Mono, Size 9.

## Windows Customizations: ##
1. **SSH in Windows** - Use Secure Shell Chrome App installed instead of Cygwin to save memory and keep everything cleaner. I copied the keys off my other computer (the ones on my aws thing actually didn't work). Here's [the instructions](http://www.mattburns.co.uk/blog/2012/11/15/connecting-to-ec2-from-chromes-secure-shell-using-only-a-pem-file/). 

The problem with Secure Shell App is that Chrome intercepts your shortcuts. To avoid this, you have to open the app as a new window. A normal shortcut can't do this, but the app window lets you do this if it set the option by opening the settings tree with a right click. 

To change the font in Chrome SSH, you can do it in the settings or open the shell in a normal Chrome tab, then open the Javascript console and use:  term_.prefs_.set('font-family', 'Consolas')

2. **Background Image Spanning Two Screens (Two Different Images):** - Get the screen resolution for both screens (ex. 2400x1200 and 1080x920) add them together (2400+1080) x (1200+920) and create an image in Paint that size (ex. 3600x2000) then paste the first background into the top-left corner and make the rest black. Then set that file to your background tiled. The first image will be on your main monitor and the spare monitor will be black.

3. **Emacs Key-Bindings** - Xkeymacs (Emacs keybindings for Windows): [downloaded here](http://www.cam.hi-ho.ne.jp/oishi/indexen.html).

Set it up like the below, with navigation commands, delete/backspace on the home-row, new-line C-j, and kill/yank stuff. Make sure to add C-o as well. Here's the exported configuration](xkeymacsConfig.reg).

Also, use the keyboard map tool (I believe referenced in the starting pages of Startup Class) to switch the left-Control and Caps Lock keys. Prevents emacs pinky. It's also possible to use the below regedit hack:
```
1. Click Start -> Run
2. Type: regedit, and click OK
3. Go to: HKEY_LOCAL_MACHINE -> System -> CurrentControlSet -> Control -> KeyBoard Layout  (Note: KeyBoard Layout, and not KeyBoard Layouts)
4. Right-click: Keyboard Layout, and select New -> Binary value
5. Rename: New Value #1 -> Scancode Map
6. Right click: Scancode Map -> Modify

0000  00 00 00 00 00 00 00 00
0008  03 00 00 00 1d 00 3a 00
0010  3a 00 1d 00 00 00 00 00  
0018

7. Close regedit and restart your computer
```

Other methods include AutoHotKey and EmacsEverywhere scripts, but it doesn't work in Outlook.

4. **Apple Keyboard with Ubuntu (never bothered with)** - -to get the function (F1-F19) keys to work, you have to hold the "fn" button on the keyboard. fn+F2 is useful for renaming files. -Although I didn't really mess with this; to get this pernamently changed: https://help.ubuntu.com/community/AppleKeyboard.

5. **Visual Studio and Git (WINDOWS)** - After installing git, I copied the relevant parts of my .bash_profile and .bashrc (mostly so I could get alias l = ls and the cs function) into a new .bashrc. Then I copied the relevant aliases from my .gitconfig dotfile. BOTH OF THOSE FILES HAD TO BE CREATED IN THE ROOT GIT BASH DIRECTORY, which is `/C/Users/dfeagans`. At the end of the .bashrc, I put a `cd /C/Users/dfeagans/Desktop/Odin`, that means git bash will always start in that folder on my desktop that contians all my projects.

Note that it's possible to call node and npm from within Git bash.
