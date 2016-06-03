# NPM NOTES #
[Documentation](https://www.npmjs.org/doc/) and [FAQ](https://npmjs.org/doc/faq.html) <--FAQ is awesome.

## INSTALLING PACKAGES - Local vs. Global ##
npm let's you install things locally and globally. Locally is in your current directory, global is in you /usr/lib/node/node_modules directory. As detailed in Hands-on node, if you var package = require("package"), it first looks for a standard Node library, then it looks in you current directory and looks for node_modules folder, then in goes up one directory and looks for the node_modules folder, and so on.
- `npm ls` = shows you whats installed in the current directory.
- `npm ls --global` = shows what you've got installed globally.
- `npm install codeUwant` is the normal, local install: If you require() a module in your code, then that means it's a dependency, and a part of your program. You need to install it locally in your program.  
- `npm install codeUwant -g` is the global install syntax. The global install method is used for downloading programs and making them command-line utilities, as it puts their bins in the system PATH. It's not for use with require().
- `npm uninstall codeUwantToRemove` uninstalls things.
- `npm install <name>@<version>` will uninstall specific versions only. When specifying the version be deliberate about it, if you know that 1.1.1 works, put 1.1.X because the rev numbers are major.minor.bugfix, so you'll get the bugfixes, but nothing will break.
- `npm outdated` - lets you know how outdated your packages are.

## PACKAGE.JSON ##
- `npm init` - creates your package.json using some questions. Then you can add your dependencies by hand and run
- `npm install` - looks for the package.json in the current directory. and installs the dependencies outlined in the package.json.
- `npm install packageName --save` - installs the package (locally because no `--global` is used) AND the `--save` line adds the dependency to the package.json. TRY TO ALWAYS DO THIS, otherwise you have to do it manually.

### devDependencies ###
There's a separate property in the package.json for dependencies just required for development, like nodemon or test packages. I think you have to manually add it, but its called devDependencies, and otherwise looks just like the dependencies (which were hopefully created using `npm install PACKAGE --save`. When you do `npm install` inside the directory with the package.json all the devDependencies will be installed unless the `--production` option is used.

### Misc NPM Package Info ###
Very simple, direct link on creating your own NPM packages [here](http://www.hacksparrow.com/how-to-write-node-js-modules.html).

Okay NPM article about creating your own NPM packages and Node command line utiliites [here](https://github.com/andrewjmead/cli-book/blob/develop/readme.md).

David Dependency Manager lets you have an image showing how up-to-date your dependencies are, just put `https://david-dm.org/<username>/<repo>.png`.

### SCRIPTS ###
You can define scripts to run within the package.json as well. There are several reasons for this:
- Define a standard command for a bunch of complicated commands so that people can use your project with one command (ex. `npm start`):
```javascript
  "scripts": {
    "start": "node index.js"
  }
```
- You can specify a standard test procedure and just run it with `npm test`:
```javascript
  "scripts": {
    "test": "mocha"
  }
```

## USEFUL MODULES ##
### [socket.io](http://socket.io/) ###
- On a new project, install socket.io module using `npm install socket.io`. Obviously, it's not installed globally since it's a dependency of the project. Ideally you'd use `npm install scoket.io --save` to populate the dependencies list in the new project's package.json though. If you are starting from this project by git cloning it, just run `npm install` to get the socket.io lib. That installs all the dependencies outline in package.json that's included in the repo.
- On the server, the socket.io is loaded using a simple `require('socket.io)`. On the client-side in the browser, you just need `<script src="/socket.io/socket.io.js"></script>`. To make a client-side program on the server, use `var socket = require('socket.io/node_modules/socket.io-client')('URL:PORT');` The previous line finds the socket.io-client lib that's installed under the socket.io lib. It's possible to install the socket.io.-client lib as a first class lib using `npm install socket.-io-client` but I haven't tested that. Then you'd just have to `var socket = require('socket.io-client');`.

#### Connections ####
The different steps in my [socketControl prototype](https://github.com/dfeagans/socketControl) have examples of how to connect both the client html and client server programs to the server program. The [repo](https://github.com/socketio/socket.io-client) also had good examples, and you could download just the socket.io.js client library to add socket.io to your client program from there (if you were doing a web-page as opposed to server where you npm it).

#### Events and Messages ####
- Send messages from the client or server using `io.emit('EVENTNAME', DATA_TO_SEND)`. DATA_TO_SEND could be just data, it could also be an object.
- Define how to handle recieved data using `io.on('EVENTNAME', function(DATA_RECIEVED){ DO_SOMETHING_WITH_DATA })`;  
- In the above example, the 'EVENTNAME' can be a made-up event like "news" or "feedback" or it could be from the pre-defined list of socket.io events that trigger under specific conditions. For example the 'connection" gets triggered upon the first connection of a new socket.

### nodemon ###
Since it's a command line tool, it gets installed globally using `npm install -g nodemon`.  It speeds up development by automatically restarts files when you modify them. Great for doing dev work. For example, in another screen window, just start your server with `nodemon SERVER.js`. You'll see it show some prompts. Then every time you modify and save server.js it will automatically restart it.

### Util ###
Util has a great function called `util.log("message")` that returns the date followed by your message.

### [async (Common Flow Control Library)](https://github.com/caolan/async) ###
The most useful flow control functions are waterfall and series:
- ([Excerpt from Here](http://www.hacksparrow.com/node-js-async-programming.html)):The difference between `series()` and `waterfall()` is that the functions in `series()` are executed one after another and their results stored in a response array in their corresponding indexes, and the response array is passed to the optional final callback function. In `waterfall()`, the result of each function is passed on to the next function, and the result of the last function is passed to the optional final callback function.
- If the result of a function is what I want it to be, such as: `callback(null,resultIwantfortheNextFunction)`, I can just use callback. This means it automatically catches the error and is shorter. See how I did this on line 53 of [serveResults](https://github.com/dfeagans/dEmail/blob/master/serveResults.js). I know the getCurrentRaceID is going to `callback(err, raceID)`, just like I had to manually code in row 44.

### [Commander](https://npmjs.org/package/commander) (Command Line Helper for Program Options and Help File) ###
#### Breakdown of Syntax: ####
Compiled from these places ([Github Examples](https://github.com/tj/commander.js/tree/master/examples),[Github API Notes](https://github.com/tj/commander.js/)).

Besides the below, there are also ways to prompt users for input and let them select from a list of options.

```javascript    
var program = require('commander');
program
  .version('0.0.1')                        //if you run your file with "version" following it, it returns '0.0.1'.
  .usage('[options] <file ...>')             //this lets you set a custom usage message in the help file, it defaults to your program name [options] otherwise
    .option('-f, --foo', 'enable some foo') //Simplest option definition. -f=short option, -foo=long option, 'enable...' shows up in help file. If this option is used, program.foo will be true. The long option becomes the property you'll look for later, if you use --foo-bar it becomes camel case later as program.fooBar
    .option('-c, --cheese <type>', 'Add the specified type of cheese [marble]', 'marble') //the <type> in this one lets the person add a custom value that will be returned when you call program.cheese. The final argument of 'marble' is the default if none is set. The < > make it a required option to fill out, if they don't do -c FETA, it will throw an error. Using [ ] makes it an optional option, and would just show true if  program.cheese === undefined if they didn't specify anything. Also, you don't have to have default case!
    .option('-l, --list [items]','Specify list items defaulting to 1,2,3',list,[1,2,3]) //This one lets the use input a list of options, but defaults to an array of [1,2,3].  Again, the [ ] make it so the user input is optional. The "list" before the array is actually a function that is run on the anything the user enters. The defaults don't get run through the function. In this case, it was the function list below, which turns the their entry into an array and forces them to be numbers. This is good use of the function: coerce their input to the appropriate form. Also Check Startup Engineering hw3 (grader.js) for another great use of that function to check that their input was valid and otherwise shuts down the program using: process.exit(1);

function list(val) {
  return val.split(',').map(Number);
}
```

Finally, you can make additional help messages (besides the automated ones) by using events:

```javascript
program.on('--help', function(){
  console.log('  Examples:');
  console.log('');
  console.log('    $ custom-help --help');
  console.log('    $ custom-help -h');
  console.log('');});

program.parse(process.argv);
```

### [url](https://nodejs.org/api/url.html) ###
core helper library to parse url's / very useful to parse requests to the http server.

First you parse the full url string using: `var targetUrl = url.parse(request.url, true)`. Then targetUrl is an object with a bunch of properties defined in the documentation, like `targetUrl.pathname`, `targetUrl.port`, or even `targetUrl.query`(an object itself). You can then find what the iso query string (in the url www.google.com/api/parsetime?iso=2013-08-10T12:10:15.474Z) is by using `targetUrl.query.iso` (it would return 2013-08...). For example. I used it extensively to get the API in dEmail working. The control parameters email was extracted from the url `http://ec2-184-72-196-208.compute-1.amazonaws.com:8080/RequestResults?email=dev` using `url.parse(request.url, true).query.email`.
