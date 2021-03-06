# Modules

Modules are self contained units of Blinx applications that load all functional components and dependencies.

All views and behaviour in any Blinx application page comes from some module. Typically, each section/area on the page is a module. For example, a Header module defines the look and feel and behaviour of the header section of a page. It may consist of a Logo, Title, Username and Password fields, and Login button.

Let's rephrase this in a more technical definition. Here it goes

> A Blinx module is an object which defines default configuration, a view, and its behaviour. We can define different methods in modules which will get triggered at different stages of the lifecycle. Also, while creating instances of module Blinx adds properties over it.

Now, let us break this definition and create a module.

_A Blinx module is an object_

```
let myBlinxModule = {

}
```

which defines default configuration,

```
let myBlinxModule = {

    // define module's default configuration
    "config": {
        "modules": [],
        "placeholders": {
            "name": {
                "firstName": "John",
                "lastName": "Doe"
            }
        },
        "listensTo": []
    }
}
```

a view,

```
let myBlinxModule = {

    // Removing redundant code for brevity

    // define module's view
    // this can be handlebars/underscore or
    // any template function which accepts data and returns html.
    template: templateFn
}
```

and its behaviour.

```
let myBlinxModule = {

    // Removing redundant code for brevity


    // define behaviour
    onAnchorClick: function(e){
        e.stopPropagation();
    }
}
```

We can define different methods in modules which will get triggered at different stages of the lifecycle.

    let myBlinxModule = {

        // Removing redundant code for brevity

        // These (resolveRenderOn, render, onRenderComplete, destory) are the four lifecyle methods
        // If defined over module, will be triggered by Blinx
        // at appropriate phase of module's lifecycle


        resolveRenderOn: function(){
            // If this method is defined over module object
            // This is the first lifecycle method to be triggered by Blinx
            // Body of this function can contain sync or async code
            // It is advisable to use this function for module setup / data initialisation / server call to get required data for module.

            // return can be sync like:
            return {};

            // return can be async promise like:
            return fetch("path/to/some/api");
        },


        render: function(placeholders){
            // If this method is defined over module object
            // This is the second lifecycle method to be triggered by Blinx
            // after "resolveRenderOn"

            // "render" receives data passed from "resolveRenderOn"
            // It is advisable to keep this function "SYNC"
            // This method is meant to create view for the module and stitch it to DOM

            // If this method is not defined, by default Blinx adds it over module.
            // Which does this
            const containerSelector = this.getUniqueId();
            const placeholders = placeholderData || this.instanceConfig.placeholders;

            if (!this.template) return;

            document.querySelector(`#${containerSelector}`).innerHTML = this.template(placeholders);
        },


        onRenderComplete: function(){
            // If this method is defined over module object
            // This is the third lifecycle method to be triggered by Blinx
            // after "render"

            // It is advisable to keep this function "SYNC"
            // This method is meant to add dom events and/or other post render processing.
        },

        destory: function(){
            // If this method is defined over module object
            // This will get triggered when module is destroyed.

            // This method is meant to do cleanup (if any required)
        }
    }

Also, while creating instances of module Blinx adds properties over it.

```
// in above "render" lifecycle method,
// we can see usage of few properties like getUniqueId
// Blinx adds these properties over the module's context while creating
// which can be accessed over module's context.

this.getUniqueId();
this.getCSSSelector();
this.getInstanceConfig();
this.getModuleContainer();
this.createChildInstance();
this.modulePlaceholders();
this.getAllSubscriptions();

// All of these are explained in more detail later.
```

## Module Lifecycle Diagram

![](ModuleLifecycle.png)

## 

## Module Lifecycle Methods

#### resolveRenderOn

A module can expose a `resolveRenderOn` function, which will be called before rendering the view for module.

`resolveRenderOn` is a pre-render function that can be used for bootstrapping. This function can be asynchronous or synchronous. If this function returns a promise, then the `render` function of the module will be called with the data resolved in this promise.

It is advisable to do all the setup or API calls for module in this phase.

#### render

The `render` function renders the view of the module and it should be a synchronous function. Any async operation inside this function can impede the sequence of child module rendering.

If the `render` function is not exposed by a module, Blinx automatically creates it and tries to compile the HTML using the exposed template and placeholder using `this.modulePlaceholders`. However, if `resolveRenderOn` function returns a promise, Blinx will compile the HTML using data returned by the promise, and not `this.modulePlaceholders`.

#### onRenderComplete

`onRenderComplete` function gets called after `render` function execution. This function should be used to bind DOM events or for post-render operations.

#### destroy

`destroy` function gets called before deleting a module instance. This function should be used to perform clean up tasks.

#### \_\_onStatusChange

`__onStatusChange` function gets called when there is a major change in a module lifecycle. The first parameter of the function is string, which indicates the state of module.

Depending on the phase of the lifecycle, one or more of the following parameters is passed:

* `LIFECYCLE:CREATED`
* `LIFECYCLE:KEEP_ON__&_REPLAY_SUBSCRIBED`
* `LIFECYCLE:INIT_ON_SUBSCRIBED`
* `LIFECYCLE:RESOLVE_RENDER_ON_CALLED`
* `LIFECYCLE:LISTENS_TO_PLAY_AFTER_RENDER_SUBSCRIBED`
* `LIFECYCLE:ON_RENDER_CALLED`
* `LIFECYCLE:ON_RENDER_CAOMPLETE_CALLED`

## Properties added on Module's context by Blinx

#### getUniqueId

\[Function\] Returns the unique id of the module.

#### getModuleContainer

\[Function\] Returns the container module element selector.

#### createChildInstance

\[Function\] This method creates a module with the current module as a parent. So if you have two modules A and B; you need B to be child module of A. From A you can simply call this.createChildInstance\({instance config}\)

#### modulePlaceholders

\[Function\] This gives the configuration provided to the module. you need to provide in placeholders object in config. The modulePlaceholders variable will take the same placeholder from the config.

#### getAllSubscriptions

\[Function\] This method returns the array of subscriptions.

## Module Structure

A typical module looks like:

```javascript
let myBlinxModule = {

    // define module's default configuration
    "config": {
        "modules": [],
        "placeholders": {
            "name": {
                "firstName": "John",
                "lastName": "Doe"
            }
        },
        "listensTo": []
    },


    // define module's view
    // this can be handlebars/underscore or
    // any template function which accepts data and returns html.
    template: templateFn,


    // define behaviour
    onAnchorClick: function(e){
        e.stopPropagation();
    },

    // These (resolveRenderOn, render, onRenderComplete, destory) are the four lifecyle methods
    // If defined over module, will be triggered by Blinx
    // at appropriate phase of module's lifecycle


    resolveRenderOn: function(){
        // If this method is defined over module object
        // This is the first lifecycle method to be triggered by Blinx
        // Body of this function can contain sync or async code
        // It is advisable to use this function for module setup / data initialisation / server call to get required data for module.

        // return can be sync like:
        return {};

        // return can be async promise like:
        return fetch("path/to/some/api");
    },


    render: function(placeholders){
        // If this method is defined over module object
        // This is the second lifecycle method to be triggered by Blinx
        // after "resolveRenderOn"

        // "render" receives data passed from "resolveRenderOn"
        // It is advisable to keep this function "SYNC"
        // This method is meant to create view for the module and stitch it to DOM

        // If this method is not defined, by default Blinx adds it over module.
        // Which does this
        const containerSelector = this.getUniqueId();
        const placeholders = placeholderData || this.instanceConfig.placeholders;

        if (!this.template) return;

        document.querySelector(`#${containerSelector}`).innerHTML = this.template(placeholders);
    },


    onRenderComplete: function(){
        // If this method is defined over module object
        // This is the third lifecycle method to be triggered by Blinx
        // after "render"

        // It is advisable to keep this function "SYNC"
        // This method is meant to add dom events and/or other post render processing.

        document.querySelector(".some-selector").addEventListener("click", onAnchorClick);
    },

    destory: function(){
        // If this method is defined over module object
        // This will get triggered when module is destroyed.

        // This method is meant to do cleanup (if any required)
    } 
}

export default myBlinxModule;
```



