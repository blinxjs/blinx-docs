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

Will name our module as counterComposite. It will follow up with several question related to requirement of module. For this tutorial, we wont opt for any of these options.

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

Now, lets import this newly created module in our entry.js file to use it.

```
// In entry.js, import counter composite along with blinx.

import Blinx from "Blinx";
import CounterComposite from "./src/apps/counter/counterComposite";
```

Now, to make this counterComposite live, we need to create instance of counterComposite.

```
// In entry.js, import counter composite along with blinx.

import Blinx from "Blinx";
import CounterComposite from "./src/apps/counter/counterComposite";


Blinx.createInstance({
    "moduleName": "counterComposite",
    "module": CounterComposite,
    "instanceConfig": {
        "container": "#app-container",
        "placeholders": {

        }
    }
});
```

Now, we have a live working module which does nothing. To see it in action and understand sequences of module method call, lets add few consoles in our counterComposite module.

```
// src/apps/counter/counterComposite/index.js

function resolveRenderOn() {
    console.log("resolveRenderOn called");
}

function render() {
    console.log("render called");
}

function onRenderComplete() {
    console.log("onRenderComplete called");
}

export default {
    resolveRenderOn,
    render,
    onRenderComplete
}
```

Now, go to the application @ [http://localhost:8080/](http://localhost:8080/) , open browser console \(Cmd+Opt+J\). It will show the logs in the appropriate sequences \(make sure your server and build job is running\).

Thats all for this step. In get the working copy by this stage, you can also checkout to branch named "step-1".

```
git checkout step-1
```

#### STEP 2

In this step we will add view for our own counterComposite. Lets create a html file for view of counterComposite. Create a file named counter.html inside "src/apps/counter/counterComposite". And add a &lt;div&gt; with default count as 0.

```
<!--src/apps/counter/counterComposite/counter.html-->

<div>0</div>
```

To use this view, we will import it inside "src/apps/counter/counterComposite/index.js".

```
// src/apps/counter/counterComposite/index.js

import moduleTemplate from "./counter.html";
```

This will compile template.html to the template function and will store it in moduleTemplate. Blinx recommends to use render function of the module to stitch any view in DOM, so lets edit our render function and add the code to stitch view to DOM.

```
document.querySelector(this.getModuleContainer()).innerHTML = moduleTemplate();

// this.getModuleContainer() provides the container selector for the module.
// which we passed while configuring our module in entry.js
// check "container": "#app-container" in entry.js
```

Complete src/apps/counter/counterComposite/index.js should look like:

```
// src/apps/counter/counterComposite/index.js

import moduleTemplate from "./counter.html";

function resolveRenderOn() {
    console.log("resolveRenderOn called");
}

function render() {
    document.querySelector(this.getModuleContainer()).innerHTML = moduleTemplate();
}

function onRenderComplete() {
    console.log("onRenderComplete called");
}

export default {
    resolveRenderOn,
    render,
    onRenderComplete
}
```

We should be able to find default counter 0 in browser now.

This default count 0 is hardcoded in out view, which is not as per the philosophy of Blinx. We should be getting all the important data from configuration. Lets add the count in configuration

as

```
"initialCount": 0
```

inside placeholders. Complete file should look like:

```
// entry.js

import Blinx from "Blinx";

import CounterCompositeModule from "./src/apps/counter/counterComposite/index";

Blinx.createInstance({
    "moduleName": "CounterCompositeModule",
    "module": CounterCompositeModule,
    "instanceConfig": {
        "container": "#app-container",
        "placeholders": {
            "initialCount": 0 // Add initial count 0 placeholder
        }
    }
});
```

To accept the property from outside, we need to modify our view i.e "src/apps/counter/counterComposite/counter.html". Change

```
<div>0</div>
```

to

```
<div>{{initialCount}}</div>
```

. Basically we ask asking our template to show the data available in attribute "initialCount" inside div. Also, we will require to pass on this configuration to template.

```
moduleTemplate(this.getInstanceConfig());
// this.getInstanceConfig() provides the configuration for the module.
// We need to pass this configuration to the view template.
```



