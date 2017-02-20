### Counter Application

Prerequisite:

* [NodeJS](https://nodejs.org/) should be installed on the system.
* Also we will be using [generator-blinx](https://github.com/blinxjs/generator-blinx) to generate modules for the application. 

```
// Once Node and npm is installed, run this command to install generator-blinx

npm install -g yo;
npm install -g generator-blinx;
```

* Clone the boilerplate for this tutorial. This boilerplate contains a static server, webpack and handlerbars configuration.

```
git clone https://github.com/blinxjs/101-tutorial.git
```

* Once clone is complete, we need to install the required packages. Packages and its versions are mentioned in package.json of 101-tutorial. Run command.

```
npm install
```

* Open terminal, cd to the project directory and run the static server to serve this application on localhost:8080

```
cd 101-tutorial;
npm start;
```

* Open another terminal tab to run the build job. This job will transpile ES6 code to ES5 and will do other heavy liftings for us.

```
npm run dev:watch
```

Now we are ready to get our hands dirty. Please note, all the commands stated to run should be run inside 101-tutorial directory unless stated otherwise.

#### STEP 1

File entry.js will be the entry point of the application, so will start editing it first. The very first step to create any Blinx application should be to import Blinx in our application. Lets install Blinx

```
npm install blinx --save
```

This will fetch blinx from npm and will add it in our project's package.json

Once blinx package is fetched and made available for our application, we will import it in our entry.js file.

```
// File: entry.js

import Blinx from "Blinx";
```

Any Blinx application can be broken down into sub applications, we will be creating a subapp named counter. Lets create a directory named counter inside "src/apps".

Now we will create a new module for this counter application. We will use generator-blinx to create this module named "counterComposite".

```
yo blinx:module
```

This will ask several questions to generate required files for the mentioned module.

```
yo blinx:module
? What type of module you want to create? (Use arrow keys)
❯ App specific
  Common
```

We need a module for counter application and it will be specific to our sub app. We dont require it for other modules, so will choose "App specific here".

```
yo blinx:module
? What type of module you want to create? App specific
? Where?
  .gitkeep
❯ counter
```

Next question will be about the location where we require to create this module. In our case, we need to create inside counter sub app. Will choose counter.

```
yo blinx:module
? What type of module you want to create? App specific
? Where? counter
? Module Name counterComposite
```

Will name our module as counterComposite.

```
yo blinx:module
? What type of module you want to create? App specific
? Where? counter
? Module Name counterComposite
? Would you like to create stylesheet for module? No
? Would you like to create config file for module? No
? Would you like to create template file for module? No
? Would you like to BLinx to take care of rendering? No
```



