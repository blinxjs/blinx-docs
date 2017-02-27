### 201 Counter Application

This tutorial is continues after [101 Counter Application](//hands-on-counter-application.md). Here we will look into Blinx Providers and how to use some of the community built providers to improve the experience.

#### Blinx Providers

Blinx Provider is the piece of code which can be used in Blinx Stack to add any new feature over any module. This feature of Blinx opens up a completely new world of possibilities. You can modify any of the lifecycle methods, or you can add any new method over module's context, you can plug pre/post method call hooks etc.

**How to use Blinx Provider?**

To use any provider in your Blinx application,  add that provider using Blinx use method like:

```
Blinx.use(MyCoolProvider);
```

**Signature of Provider**

Any valid provider should return an object.

**How does it work?**

Object returned by Provider will be merged with module object. If we have added any method which is not present over module it will be added to module object. If a method with the same name is already present on the module, it will be overridden.

Basically, module will be somewhat like:

moduleObj = Object.assign\(moduleObj, providerObj\)

---

Let us get back to counter application now and make sure you are using latest version of Blinx\(0.9.73 at time of writing this\). We will be using 3 providers in this application namely:

1. bind-ext: To bind events using module configuration.

2. smart-render: This provider smartly differentiates between the two HTML blocks and figures out the changes which needs to be patched in DOM.

3. observer: Adds \_ over the module context. Whenever any data changes over \_, it calls the dependent function mentioned in configuration.

Get started and import these packages in our counter application. First we will need to install package for all these providers. All the three mentioned provider is part of blinx-extensions package. Go ahead and install that using npm

```
npm install blinx-extensions --save
```

Once installed in project directory we need to import in our project like:

```
// entry.js

import eventBind from "blinx-extensions/lib/bind-ext";
import smartRender from "blinx-extensions/lib/smart-render";
import observer from "blinx-extensions/lib/observer";
```

and also start using it

```
// entry.js

Blinx.use(eventBind);
Blinx.use(smartRender);
Blinx.use(observer);
```

Now, entry.js should look like:

```
// entry.js

import Blinx from "Blinx";

import eventBind from "blinx-extensions/lib/bind-ext";
import smartRender from "blinx-extensions/lib/smart-render";
import observer from "blinx-extensions/lib/observer";

Blinx.use(eventBind);
Blinx.use(smartRender);
Blinx.use(observer);

import CounterCompositeModule from "./src/apps/counter/counterComposite/index";

Blinx.createInstance({
    "moduleName": "CounterCompositeModule",
    "module": CounterCompositeModule,
    "instanceConfig": {
        "container": "#app-container",
        "placeholders": {
            initialCount: 10
        }
    }
});
```

Now, come to counterComposite/index.js, we have to use smart render and observer to make operations easier. Lets initialise the initialCount in resolveRenderOn

```
function resolveRenderOn(){
    this._.initialCount = this.modulePlaceholders.initialCount;
} 
```

also, lets create a function to update the count

```
function updateCount(op) {
  if(op === "+"){
    ++this._.initialCount;
  } else {
    ++this._.initialCount;
  }
}
```

So complete counterComposite should look like:

```
// src/apps/counter/counterComposite/index.js

import config from "./config";
import moduleTemplate from "./counter.html";

function resolveRenderOn(){
    this._.initialCount = this.modulePlaceholders.initialCount;
}

function updateCount(op) {
  if(op === "+"){
    ++this._.initialCount;
  } else {
    --this._.initialCount;
  }
}

export default {
    resolveRenderOn,
    config,
    template: moduleTemplate,
    updateCount,
    observe_For: ["render"]
}
```

If you will check the view, you will find counter initialised and two button for incrementing and decrementing this counter. But these buttons wont work as we haven't bound any event listeners.

We will be using bind-ext to bind events through config. Let us got to counterComposite/config.js and add event listeners:

```
{
    ...
    
}
```



