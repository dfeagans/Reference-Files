My [set-up script](https://github.com/dfeagans/setup/blob/master/setup.sh) should install nvm and then use it to install node and npm's most recent stable releases.  nvm lets you change versions of node quickly using the following:

- `nvm ls` = shows what versions of node/io.js are installed on your computer.
- `nvm ls-remote` = shows all available versions of node and io.js
- `nvm install version#` = install that version of node.
- `nvm uninstall version#` = uninstalls that version.
- `nvm use version#` = actually uses that version of node/io.js. Let's you toggle between different versions of node!
- `nvm exec version# npm test` = runs the command "npm test" using that specific version of node. This is the main selling point; testing using different versions of node/io.js.
- `nvm alias ten 0.10.0` = creates an alias ten with that specific number. 
- `nvm alias default stable` = sets the default alias to equal the current stable version. Could have also used a version number (ex. 0.10.0). I run this right after installing nvm in my setup script, so I don't have to have a nvm use version# in my .bashrc like everyone else. I don't like that it's hidden within the .bashrc. I want the nvm configs in the nvm.

**The biggest concern with changing node versions is that it switches entire entire folder structure with the version. That causes the global node modules in the node_modules directory to no longer be there. So if you change versions, you have to install all your global packages again for that version of node. If you change back to the version of node where you originally installed them they'll be there.**
