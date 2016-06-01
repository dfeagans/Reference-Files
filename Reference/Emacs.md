# Emacs and Vim #

__Notes from Lecture 4b of Startup Class and then Personal Modifications)__

Built in Tutorial is pulled up using: `M-x help-with-tutorial`

## GENERAL ##
**Legend**: Ctrl(Control-Key) = C / Meta(Alt-Key) = M*  
`emacs -nw` opens emacs without a window, since I'm working in a terminal. I have it aliased to just `emacs` in my .bashrc. Sometimes it's useful to start emacs without customizations to debug emacs itself (or your init.el file), to do so use the -q option (open without customizations), or -Q (open without customizations or spash screen).  

`C-x C-c` = Exit (it will prompt you in-case you need to save anything.  
`C-xz` = disconnects Emacs and puts it in background to let you run bash, etc.  
`fg` = foreground - brings Emacs back up where you were at.  
`C-l` = redraws the screen with the current line in the middle of the screen. Hitting it again, or `C-l C-l` = brings current line to the top of the screen.  
`C-g or <esc><esc><esc>` = quits the current Emacs command a resets it for input. Good if Emacs stops responding.  
`M-/` will autocomplete whatever you're typing. Keep hitting it to scroll the entire list. Alternatives are [hippie-expand](http://trey-jackson.blogspot.com/2007/12/emacs-tip-5-hippie-expand.html) and yasnippet.  
`C-/` will undo a change. C-x u, C-_ also work.  
`M-;` = comments whatever you have selected. If you don't have anything selected, it adds the comment to the end of the row.  
`Alt+!` = prompts you to run one shell command.

## FILES ##
`C-x C-f` = find file to open or create. You can type to search for it or use the arrows to navigate around and select it.  
`C-f C-b` shows all the files you've opened and modified.  
`C-x b` = lets you quickly switch between the currently opened buffers just by typing it's name. Tab auto-completes like Bash which is very useful.  
`C-x C-s` = saves current buffer.  
`C-x s` = individually prompts to save ALL the buffers that have been modified.  
`C-x s d` = runs `diff-buffer-with-file` and shows you the modifications you've made before saving. Super useful.  
`C-x k` lets you select which buffer or window to kill using the typical mini-buffer methods at the bottom (typing or arrow movement).  
`C-x C-b` = shows current buffers in a separate dir'd window. Navigate to line and hit enter to open. Could be useful for huge project.

## AUTOSAVE BACKUPS ##
Emacs preiodically creates an autosave file (denoted by # before and after the name). If you want to recover that, find the file like normal using `C-x C-f` then use `M-x recover-file` then say yes to retrieve it. **Note that I turned off the auto-saves in my init.el though**.

## MOVEMENT ##
Moving cursor - `C-f` forward / `C-b` back / `C-p` previous line up / `C-n` next line down / (same as GNU screen)  
Moving cursor fast - `M-f` forward a word / `M-b` backward a word. I've got custom functions mapped in my init.el that use `M-n` and `M-p` to move up and down 5 lines.  
`C-v` or `PgUp` button = page up. Useful with meta-key for scrolling other buffer window.  
`M-v` or `PgDn` button = page down. Useful with meta-key for scrolling other buffer window.  
`C-j` = just like Enter or RET key, but does indent as well! If you want to get RET to do the same thing, use: `(local-set-key (kbd "RET") 'newline-and-indent)` in the init.el.  
`C-a` = moves to the __beginning of the line__.  
`M-a` = moves to the __beginning of the sentence__.  
`C-e` = moves to the __end of the line__.  
`M-e` = moves to the __end of the sentence__.  
`M-g-g` = lets you move to a specific line.  
`M->` = moves to __end of the file__.  
`M-<` = moves to the __start of the file__.

PREFIX ARGUMENTS  
You can add prefix arguments to commands using C-u to do things multiple times ex. C-u 8 C-p moves the cursor up 8 lines. C-u 100 A puts down 100 "A"s.

## CUT/PASTE ##
SELECTING TEXT to kill: `C-<spc>` then use typical move keys to highlight.  
`C-w` to kill(cut) your selection.  
`M-w` to copy.  
`C-k` = (kill) cut from cursor to end of the line.  
`M-k` = (kill) cut from cursor to the end of the sentence.  
`M-w` just copies your selection to the kill ring.  
`C-y` = (yank) paste the killed text in.  
`M-y` = yanks the previous kill before the C-y one.

## UNDO ##
`C-/` = undoes last command.

## EMACS COMMANDS ##
`C-x` = single character commands (as seen before, C-s saves the file, C-f find files, C-b lists the buffers, etc.
`M-x` = long macs commands ex. `M-x replace-string` then enter will prompt you to replace a word with another word everyplace after the current cursor position. `M-x revert-buffer` lets you quickly reload your modified file back to it's original state. `M-x shell` opens a bash shell in the buffer.

## MACROS ##
`f3` = lets you start recording a macro (just like Excel macro, lets you do things multiple times).
`f4` = stops recording the macro, and then after that pressing f4 would run the macro you just created.

## SEARCHING ##
`C-s` = forward search. Everytime you hit `C-s` it will move to the next match (backspace to go back). Hit enter to leave cursor that position. As always C-g escapes out.  
`C-r` = reverse search. Everytime you hit `C-r` it will move up to the next match (backspace to go back). Hit enter to leave cursor in that position. As always, `C-g` escapes out.  
`M-s o` = occur, searches current buffer and presents results in nicely formatted second window.  
`M-s s` = multi-occur on all open windows. This is a custom added function in my init.el

## WINDOWS ##  
`C-x 1` = Gets back to one window.  
`C-x 2` = Splits into 2 windows (one ABOVE the other). Cursor stays in top frame.  
`C-x 3` = Splits into 2 windows (one BESIDE the other). Cursor stays in the original frame.  
`C-x 0` = closes the current window.  
`M-o` (and possibly i?)= moves to (o)ther frame if available.  
`M-PageUp and M-PageeDown` = scrolls up and down the frame you DON'T have selected.  
`C-M-v` = scrolls the frame that you DON'T have selected.  
`C-x ^` = increases size of current frame when split horizontally. use C-u # C-x ^ to increase frame size by # rows.  
`C-x }` or `C-x {` = adjusts size of current frame when split vertically.  
`C-x +` =  evens out window sizes.

### FUNCTIONS FOR HELPING KEY-BINDINGS  (employed using M-x) ###
describe-function = lets you type in a function name and gives you all the info about it. (`C-x h f` due to my custom keybindings where `C-h` is used for backspace).  
describe-key = prompts you for a key-chord, then describes the function you have bound to it. (`C-x h k` due to my custom keybindings where `C-h` is used for backspace).  
describe-variable = tells you the value of the variable current. Bound to `C-h v` (`C-x h v` due to my custom keybindings where `C-h` is used for backspace) Useful for debugging things. It's also useful because variables control whether modes are on and off and different settings (like column-width for example).  
describe-mode =  tells you all the info about the current mode and most importantly all the keybindings (tied to `C-x h m` due to my use of `C-h` as backspace). It also tells you the hooks for the mode.

The above functions are really useful for determining what keys and what functions to bind to different keys. The other thing that was useful was digging into the source for the modules I was loading to figure out what keybindings I had to remove and what functions they were calling to move them.

Check my init.el and my-packages.el because I have examples on setting up the different key-bindings:
  1. Simple global keybindings that go over everything. (global-set-key ...)
  2. mode-map specific keybingings for specific required modes (define-key MODE ...)
  3. mode-map specific keybindings for specific autoloaded modes (eval-after-load ...)

For further information on customizing the font-lock keywords reference: http://emacswiki.org/emacs/AddKeywords

Insanely Good Article about Creating Your own Emacs Extensions:  
http://toumorokoshi.github.io/emacs-from-scratch-part-3-extending-emacs-with-elisp.html  
Check emacs from scratch part 1&2 for a note about hooks.

### Customizing an Installed Module ###
You can learn all the options, settings, and commands using M-x customize-group RET package-name RET. Then you navigate to the option you're looking for (note you can hit enter over More to get a more verbose description. Then hit enter while on [ State: ] to see a list of options to select. After changing the option, hit enter while on [ State: ] again and it will prompt you to save it. If you save it for future session it will add the line to your .init.el. It will have extra formatting with it, but you can just steal the config line and add it in the appropriate place to be cleaner.

### Customizing Color Themes ###
`M-x` describe-face RET identifies what emacs calls the region your cursors is on so you can add a line to the init.el to change the color. Even better, hitting enter again actually shows you every parameter.  
`what-cursor-position` is even more powerful an bound to `C-u C-x =`.
`list-faces-display` lists ALL the faces avialable.

### Actual Themes ###
If I really want to use themes, the I have to change $TERM to xterm-256color. Definitely in my .screenrc and then somehow in normal terminal (likely export it in my .bashrc). Then I can just use install the theme, test it out using M-x load-theme, and make it start with it by adding `(load-theme 'themeName t)`.

### Performance Analysis ###
To find what's slowing Emacs down:  
Invoke `M-x profiler-start RET RET` (the second RET is to confirm cpu); Do some typing, preferably an entire paragraph or more;Invoke `M-x profiler-report` in my init.el.

### General Emacs Lisp Commands ###
#### emacs List and Lisp-Interaction-Mode ####
(Download This and Go Through It ----> Summary of Learn Emacs Lisp in 15 minutes)
Being in "M-x lisp-interaction-mode" lets you evaluate lisp expressions using:
C-j = when the cursor is at the end of the line, it runs the command before it and prints the results to the next line.
C-x C-e = when the cursor is at the end of the line, it runs the command before it and prints the results to the mini-buffer.
M-: = lets you eval a command you type in the mini-buffer.
C-u before C-x C-e or M-: make the result of the command dump to the cursor point.
(quote variableName) is the same as 'variableName = that's when you want to the name of the variable instead of it's value. It stops emacs lisp from evaluating it and just gives you the reference. If you don't have the quote, it would evaluate the variable there and return it's value instead of just providing the variable to the operation. Example, in (add-hook 'emacs-lisp-mode-hook 'turn-on-eldoc-mode), emacs-lisp-mode-hook is a variable that contains a list of functions. If you use the quote it just provides a reference to the variable instead of the contents (the list of functions).
(setq variableName "stringOrData") = setq sets GLOBAL variables. setq actually stands for set quoted, so it takes care of the single quote before the variableName. It's the equivalent of (set 'variablename "data").
(setq-default variable "stringOrData") = set the variable value default for all buffers. There are some varaibles that are "buffer local", this allows you to have custom settings per buffer. If it's "buffer local", then setq just changes the variable for that buffer. setq-default sets the default value for all buffers. If the variable isn't buffer local, then setq and setq-default do the same thing.
(insert "string" "multiple strings") = inserts string at cursor position
(defun functionName(arguments) processArguments) = defun lets you define functions, for later use by calling: (functionName arguments).
(progn bunchOfStuff) = progn acts like an an IIFE (immedietely invoked function expression). You can define a bunch of stuff to happen in a row. Used most regularly to run a lot of stuff within sections of if statements (which would otherwise just run one command).
Lists (Arrays) in LISP
(setq variableListName '("item1" "item2" "item3")) = creates a list (array in other languages). The single apostrophe is to tell lisp that it's not an symbolic expression (s-expression), so don't evaluate them. Otherwise it would think "item1" is the name of a function with parameters "item2" "item3" and try to evaluate it.
(car list-of-names) = returns the first element of a list.
(cdr list-of-names) = returns all the elements of a list except the first.
(push "newFirstElement" listName) = adds an atom to the beginning of the list
(mapcar 'functionName listName) = maps the function to all elements of the list.
Random EMACS Commands
(switch-to-buffer-other-window "*test*") = switches to the buffer *test*. Creates it if necessary.
(erase-buffer) = clears the active buffer.
(other-window 1) = returns to the other EMACS window.
(let (varList) (body)) = let let's use define local variables and then mess with them in the body (body can be multiple lisp expressions). The let returns the final evaluation from the body. An example varlist would be ((a 3) (b "test")). Then you could use a and b in the body expressions. You don't have to use progrn here obviously. If that var list just has one, you have to define it using: ((a 3)).
(format "Hello %s!\n" "visitor") = works like printf.
(read-from-minibuffer "Enter your name: ") = lets you prompt for input from the mini-buffer.
(goto-char (point-min)) = goes to the first character of the buffer.
(while (search-forward "Hello")  = (while x y) does y when x returns something. If x is 'nil' then the loop is exited.
         (replace-match "Bonjour")) = a better search-forward syntax is (search-forward "Hello" nil t), the nil means it's not bound to position and the t means fail silently when you don't find the string. There is also (re-search-forward "string") that uses regular expressions.
(add-text-properties (match-beginning 1) | 
          (match-end 1) |
          (list 'face 'bold)))   = This was used to make the text found above bold.
Vi Notes (type vimtutor for a quick lesson or use)
i - insert mode that allows typing BEFORE the current character. Shift+i results in typing at the start of the line.
a - append mode that allows typing AFTER the current character. Shift+a results in typing at the end of the line.
esc - control mode
     a - go into insert mode after current cursor position
     A - go into insert mode on the end of the line
     ZZ - save and close
     :q - quit (will fail if not saved)
     :q! - quit without save
      x - delete character
     dd - delete line
     Moving - b: beginning work and moves backwards, e: end of work and moves forwards, w: beginning of word and moves forwards
