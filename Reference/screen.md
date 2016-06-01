# GNU Screen #

Lets you have multiple tabs and saves your state incase you lose your network connection.

https://www.gnu.org/software/screen/manual/screen.html

(Notes Below Summarized From Start-up 101 Lecture 4 Notes / [IBM Screen Reference](http://www.ibm.com/developerworks/aix/library/au-gnu_screen/))

---

Screen is configured by your .screenrc file. [Here's mine](https://github.com/dfeagans/dotfiles/blob/master/.screenrc). The most important thing it does is change the control key to `C-t` to mesh better with emacs. Mine also makes it so screen set-ups a default two window session when you start it.

[List of All Commands](http://aperiodic.net/screen/commands:start): Can be used within screen or in .screenrc to automate startup.

In all the shortcuts, `C-t` means `Ctrl+t`
- `C-t ?` = Help menu

#### OPEN/CLOSE ####
- `screen` = open. It's possible to open a screen session and give it a sessionname (see below) using `screen -S desiredSessionName`.
- `exit` = close (might take several, by default my screen opens 2 windows)
- `C-t :quit` = actual close

#### CREATION/DELETION ####
- `C-t c` = creates a new tab
- `C-t A` = lets you rename that screen tab
- `C-t K` = kills the current tab (will prompt for approval)

#### NAVIGATION ####
- `C-t "` = opens menu to change screen tab with up and down arrows. Enter is select.
- `C-t tab#` = changes to that tab number
- `C-t C-t` = toggles back to most recent screen tab.

#### SIDE-BY-SIDE ####
- `C-t S` = creates a horizontal divider
- `C-t |` = creates a vertical divider
- `C-t TAB` = switches between screen divider sections
- `C-t :resize [+-]#` = resize a divide section +/- # of lines
- `C-t X` = removes current divided section
- `C-t F` = resizes the window to the current region size.

#### DETACH/REATTACH ####
- `C-t C-d` = detaches from your current screen session and saves everything in it's current state! You can even log out of your SSH connection.
- `screen -r` = reattaches to your screen that you dettached from.
- `screen -ls` = lists all the screen sockets/sessions you have going (you can disconnect/createnew/dettach multiple times. To reattach to a specific one, `screen -r ProcessNumber`. You can also kill any ones you don't need with kill ProcessNumber.
- `screen -x` = Reattach to failed SSH session: If your SSH session fails it will still say (Attached), so you can't just `screen -r`.
- `C-t :sessionname NEWNAME` = While you have a screen session going, this lets you rename the session. If you use `screen -ls` it will return processID.NEWNAME now. More usefully, you can then `screen -r NEWNAME`.

#### COPY/PASTE or Just Scrolling in Screen Window ####
- `C-t [` to enter copy mode
- Move around with (`C-f`) forward (`C-b`) back (`C-p`) up (`C-n`) down
- `Spacebar` = place copy cursor select the rest of it with the standard movement keys (above).
- `>` to write to buffer (basically copy selected text). This buffer is saved in tmp/.
- `C-t ]` to paste buffer.
- `Esc` = gets out of copy/paste mode.

#### General Notes ####
- Screen resets the windows when you disconnect. If you do `C-t` and type `:layout save default` it saves it for next time. It will save it until you genuinely :quit screen, but keep if for detach/reattaches.
- You can add a password to your screen with `C-t :password`, then it will prompt you to enter it twice. Next time you connect to that session it will prompt you for the password. To remove it use `C-t :password none`. 
