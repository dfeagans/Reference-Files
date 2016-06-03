Writing Code on AWS (EMACS/NODE)
Node REPL in Emacs
Open/Close REPL
C-c ! turns the current window into a Node REPL (Read Eval Print Loop), because of that, it's useful to use C-x 3 to make a vertical split window and the M-o to select the other window.
Run Code
C-c C-j pastes the currently selecting line in the file into the REPL window and runs it.
C-c C-r pastes the currently selected region in the file into REPL and runs it. You select a region using the normal emacs select commands: C-_(space) to place the start selecting and then maneuver using C-f,b,n,p to select the region.
               Note1: This copy-paste-run only works if your "//" comments are on their own line. If you really want to put comments on the same line, after the code, you can use /* comment */ though.
               Note2: To clear the javascript buffer you type M-x delete-region while in that window. It will delete it all, without clearing the node variables and functions. If that doesn't work, you can select and delete the entire region using C-space and Emacs navigation keys to highlight and then the delete key.
REPL Functions
.break - breaks off a bad loop or a waiting for data symbol or error (... ^[[5G)
.clear - clears the context of the REPL (deletes all variations and functions)
.help - help

JSHint
C-C C-u runs the current file through JSHint.
C-m will move your cursor to the offending line/character if you have your cursor on the error line in the JSHint window. This works in the multi-occur search as well btw.
M-g g prompts for a line number and takes your cursor to it after you hit enter.

Namecheap DNS URL Set-up
I bought a domain name from Namecheap so that I could host projects from my AWS server. To connect it, you just have to grab the Public IP from the AWS dashboard and put it in the namecheap "All Host Records" section under "A Records", as the table below shows, A records point to specific IP addresses. Then your domain, okdane.com in my case, can be access using okdane.com to get to the AWS server on port 80 (port 80 is the default port for http). I like to host development projects on Port 8080, so you could use www.okdane.com:8080 to get to it. Even better it's possible to redirect a sub-domain to www.okdane.com:8080 using a URL redirect or URL frame. It takes a couple minutes for it to work on namecheap though. URL redirect with change the url in the browser back to the original IP Address/URL, whereas a URL Frame will keep the sub-domain they typed in there aka dev.okdane.com below.

Final Settings:
The below results in www.okdane.com and okdane.com being pointed to the IP in the table (which is the public IP for the AWS instance). The dev sub-domain setting directs dev.okdane.com to okdane.com:8080 where I host development stuff.

Note: You have to have root privelages (sudo) to use ports below 1024.
Further Background:
A (Address) record (IPv4)This record should be created when you want to associate your domain or subdomain with IP address (32-bit one). Please note that you can only set up A record for the domain if it's using our default nameservers.
Please enter an IP address for both the WWW and the @ fields and choose A (Address) as the Record Type from the drop down menu:
CNAME recordThis record specifies that the domain name in question is an alias of another, canonical name.
CNAME record will point your domain or subdomain to the IP address of the destination hostname. So, if the IP of the destination hostname changes, you won't need to change your DNS records as the CNAME will have the same IP.
Please don't set up CNAME for naked domain (@ hostname), since it may affect the operation of the domain's MX records and, consequently, the email service. Thus, in most cases you will need to create CNAME record for WWW (or other subdomain) and URL redirect for @ that will point to http://yourdomain.com/
URL RedirectThis record type is used when you wish to configure your domain in the following way: when the users type the name of your domain in the address bar, they are redirected to any specific web page, for example,http://maps.google.com/ or http://en.wikipedia.org/wiki/Main_Page 
The domain is redirected to the indicated page and in URL bar the destination address is shown instead ofyourdomain.com.
This is how the record should be configured in Namecheap account, All Host Records section:

URL FrameThis host record is used when you wish to redirect your domain name similarly to URL redirect, but you wish to have your domain to be shown in the browser's address bar instead of the destination link.

Please see how it should be configured in All Host Records section:

Old Setup/Background
My development environment was initially an AWS Ubuntu instance, but I couldn't SSH into it on the work network, so I had to do some work on some stuff through Cloud9 IDE. I had a Github repo on github.com. I'd work using the AWS primarily, and only use Cloud9 at work. To switch back and forth computers, I'd "git push" to the github repo and "git pull" down to the new computer (or Cloud 9) before starting to work. Cloud9 provides a URL for your page you're developing, but I had to host a site on AWS to live view the page. I would have used github paages or Heroku instead of hosting, but github pages takes a while to update, and I didn't want to have to push to Heroku. Hosting on AWS lets me set it up once and then just updates everytime you save.

Getting HTTP/HTTPS working and web-hosting on AWS
Go into your AWS EC2 management section and see what security group your running instance is using. Modify the security instance settings on the LH toolbar. You need to add HTTP and HTTPS (both with the standard 0.0.0.0 settings or whatever). Then add a custom TCP rule on port 8080 (again source will be standard 0.0.0.0/0). Apply the changes. It has to be on a high port number for security purposes. Otherwise you have to use sudo, like I had to to start the ghost blog.

To get it actually hosted, first install express using "sudo npm install -g express". Then you can grab the web.js server file from my github repo using wget https://raw.github.com/dfeagans/staticDev/master/web.js   This serves up whatever index.js is that folder. See github readme for full details. If you do "node web.js" it runs the above code to host that web page. You can then access the site using your hostname and port number i.e. ec2-184-72-196-208.compute-1.amazonaws.com:8080. If you want to kill the hosting, just hit C-c and it will take you back to the normal terminal.

Giving URL's to those development sites: Namecheap Domain Forwarding w/Masking
During my development I wanted to give a creo.okdane.com subdomain that pointed to the cloud9 hosted page (actually at
https://c9.io/dfeagans/dev/workspace/index.html) and a dev.okdane.com subdomain for my aws hosted page. To do that go to namecheap.com > manage domains > select your domain > All Host Records and under sub domain settings add "creo" then the cloud9 URL and "URL Frame". URL Frame method masks the cloud9 url, if you did "URL redirect" it would change the address back to the C9 url. Continue with dev subdomain as well.

From then on out it was just write static code, save it, and then open the url to test it out.

Alternative Local Development Process:
Use Chrome DevTools and do local development on your computer. It provides several advantages:
Edit Files within Developer Mode: http://youtu.be/O3W1yuq-ZlE another one here: http://www.youtube.com/watch?v=x6qe_kVaBpg#t=24.
    -In Developer Mode, under sources, right click the sources bar and select "Add Folder to Workspace", then "Allow" (This allows Chrome to see the directory).
    -Then right click your html, css, or whatever in the Sources list and select "Map to File System Resource" and selecte the file on your disk. (not the one in your newly visible directory). This maps it so any changes you make persist on the disk.
    - Now you can modify the mapped file using all the tools in Chrome and it saves to disk.
Terminal within Chrome Developer Mode: http://www.html5rocks.com/en/tutorials/developertools/devtools-terminal/
Then look into a site reloader for dev. It makes your browser auto-reload when you save the html,css, or js: http://livereload.com/ or http://feedback.livereload.com/knowledgebase/articles/86189-i-don-t-like-livereload-can-you-recommend-somethi

