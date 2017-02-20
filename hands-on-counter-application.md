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

Any Blinx application can be broken down into sub applications, we will be creating a subapp named counter. Lets create a directory named counter inside "src/app".

