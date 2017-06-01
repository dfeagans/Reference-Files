# AWS Remote Python Data Analysis System #

## Installing Python/Anaconda/Jupyter Notebook ##

- **[Anaconda](https://www.continuum.io/downloads)** was highly recommended as a convenient way to install lots of relevent libraries and manage them, so I used their Python 3.6 version.
    
    - **Download Anaconda Install Script** - First, download the install script from their website to your AWS instance using "wget" and the download URL (easily retrieved by right clicking their link at the site above and selecting `Copy Link Address`, which you can then paste after the `wget`).
	
	- **Run Install Script** - Next, in your $HOME directory, run that install script using `bash Anaconda*.sh` (If you're worried about running bash scripts off the internet, you can verify it by checking the hash). 
	
	- **Configurate Anaconda Install** - During the install, there will be a prompt on whether you want to prepend the Anaconda directory to your PATH (meaning it searches there FIRST for programs). Most importantly, that let's your `python` calls find the conda-managed python bin in "$HOME/anaconda3/bin/python" and not the generic, likely old python installed with Ubuntu by default in "/usr/bin/python". That allows anaconda/conda fulfill it's purpose as a package/environment manager. I manage my dotfiles in git, so I clicked NO and had to add this line manually to my .bash_profile to achieve the same effect: `export PATH=$HOME/anaconda3/bin:$PATH`. To confirm anaconda was added to your PATH correctly, reload your your .bash_profile using `source .bash_profile`, then use `which python` to prove the python bin is found in your "$HOME/anaconda*" directory as expected.

## Running Jupyter Notebook ##
- **Background of Process** - Jupyter Notebooks by default are set-up to be run like a simple desktop program (entirely on one computer): starting the process using `jupyter notebook` opens your browser to the Jupyter Notebook site, as served up behind the scenes on the localhost. It doesn't even look to accept requests from other IPs besides local host. The configuration items below serve to set-up Jupyter and JupyterLab to be remotely logged into and stop it opening the browser when you start it since it's no longer required.

- **Configuration of Jupyter Notebook** - To create a new Jupyter configuration file (in a new ~/.jupyter directory), use `jupyter notebook --generate-config`. To start, all configurations settings will be commented out.

	- **Password at Login** - Since we'll be operating over the internet it would be beneficial to make the notebook password protected. Instead of storing the password to check, we'll store the hash of the password. To create that password hash, in the `ipython` shell (started by running `ipython`), type:   
        ```
        from IPython.lib import passwd
        passwd()
        <ENTER DESIRED PASSWORD WHEN PROMPTED and COPY THE RESULTING HASH OUT>
        exit
        ```
        We'll then paste that hash into the uncommented password configuration line: `c.NotebookApp.password = u'sha1:c59b7ee...<PASTE FULL PASSWORD HASH IN HERE>'`

    - **SSL Encryption (HTTPS)** - We don't want to trasmit that password through the open internet to log-in, so the next step is setting up SSL (so we can operate over https://). The following lines create the necessary keys:
        ```
        mkdir certs
        cd certs
        openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.key -out mycert.pem
        ```
        Those SSL keys can then be used by adding the following lines to the Jupyter configuration file:
        ```
        c.NotebookApp.certfile = u'/home/ubuntu/certs/mycert.pem'
        c.NotebookApp.keyfile = u'/home/ubuntu/certs/mycert.key'
        ```
    
    - **Acceptable IPs** - As mentioned earlier, by default Jupyter Notebooks operated entirely on the localhost. To open the process up to accepting requests from other, remote IP's add: `c.NotebookApp.ip = '*'`.
    
    - **Port Access** - By default, Jupyter Notebook is served up on Port 8888. I add the line `c.NotebookApp.port = 8888` to the Jupyter config to explcitely remind me to add that TCP port to my AWS security group. It would also be possible run it on the default HTTPS port 443, I'd just have to open add that port to the security group instead.
    
    - **Stop Browser From Opening** - Since we'll be running this as a notebook server, we don't need it to try to open the browser in the terminal session. To stop that from happening, you can add `c.NotebookApp.open_browser = False` to the configuration file. This could otherwise be achieved by starting the process with the --no-browser flag aka `jupyter notebook --no-browser`.

- **Starting Jupyter Notebook** - With the previous settings in place, you run the Jupyter notebook server, you just type `jupyter notebook`, navigate to https://\<AWS PUBLIC IP\>:8888 or https://okdane.com:8888 in my case. Since you want it to run indefinitely, it's best to run it from a `screen` instance though and detach. Whichever directory you're in when you run `jupyter notebook` effectively becomes the working directory when you log-in, so I created a directory in my home specifically for data projects and ran it from there.

## [JupyterLab Alpha Preview](https://github.com/jupyterlab/jupyterlab) (More complete Python Notebook IDE) ##

JupyterLab expands on the Jupyter Notebook concept by adding integrated file navigation, windowing, and terminals. It requires at least Jupyter Notebook v4.2. To confirm that before attempting to install, use `jupyter notebook --version`.

- **Install** - To install the JupyterLab extension just use `conda install -c conda-forge jupyterlab`.

- **Running** - JupyterLab uses the same Jupyter Notebook configuration file (for example, to define ports, IP's, passwords, etc.). If you were running it in the default configuration, `jupyter lab` would start the normal Jupyter Notebook process with the JupyterLab extension, and open a browser directed to http://localhost:8888/lab (the normal Jupyter Notebook would still be available at http://localhost:8888). Since I'm running jupyter server remotely, you can use either `jupyter notebook` or `jupyter lab` to start the "server" and then just manually navigate to whichever path you want: the normal Jupyter Notebook (https://okdane.com:8888) or the JupyterLab (https://okdane.com:8888/lab).

- **Default URL Config** - If you really fall in love with the JupyterLab though, adding `c.NotebookApp.default_url = '/lab'` to the configuration file will make the root URL default to JupyterLab. That means https://okdane.com:8888 will actually direct you to the JupyterLab https://okdane.com:8888/lab.

## Complete Jupyter Notebook / JupterLab Configuration File ##
```
c.NotebookApp.open_browser = False
c.NotebookApp.certfile = u'/home/ubuntu/certs/mycert.pem'
c.NotebookApp.keyfile = u'/home/ubuntu/certs/mycert.key'
c.NotebookApp.password = u'sha1:c59b7ee...<PASTE FULL PASSWORD HASH IN HERE>'
c.NotebookApp.ip = '*'
c.NotebookApp.port = 8888
c.NotebookApp.default_url = '/lab'
```
