# Structure and Configuration

A typical module looks like:
```
// Module Template
{
  config: {

  },
  template: function(){

  },
  resolveRenderOn: function() {

  },
  render: function() {

  },
  onRenderComplete: function() {

  },
  destory: function() {

  },
  __onStatusChange: function() {

  }
}
```

The following sections deconstruct the components in a Blinx module.

## ```config```

A Blinx module can expose a ```config``` object containing the details of module children and default placeholders.

```
{
  modules: [
    //child module configuration
  ],
  placeholders: {},
  listensTo: []
}
```

## ```template```

A ```template``` function can be exposed by a module. This will be used if the module does not expose ```render``` method. Blinx will try to render the view for a module automatically.


## ```resolveRenderOn```

A module can expose a ```resolveRenderOn``` function, which will be called before rendering the view for module. 

```resolveRenderOn``` is a pre-render function that can be used for bootstrapping. This function can be asynchronous or synchronous. If this function returns a promise, then the ```render``` function of the module will be called with the data resolved in this promise.

It is advisable to do all the setup or API calls for module in this phase.


## ```render```

The ```render``` function renders the view of the module and it should be a synchronous function. Any async operation inside this function can impede the sequence of child module rendering.

If the ```render``` function is not exposed by a module, Blinx automatically creates it and tries to compile the HTML using the exposed template and placeholder using ```this.modulePlaceholders```. However, if ```resolveRenderOn``` function returns a promise, Blinx will compile the HTML using data returned by the promise, and not ```this.modulePlaceholders```. 


## ```onRenderComplete```

```onRenderComplete``` function gets called after ```render``` function execution. This function should be used to bind DOM events or for post-render operations.

## ```destroy```

```destroy``` function gets called before deleting a module instance. This function should be used to perform clean up tasks.


## ```__onStatusChange```

```__onStatusChange``` function gets called when there is a major change in a module lifecycle. The first parameter of the function is string, which indicates the state of module.

Depending on the phase of the lifecycle, one or more of the following parameters is passed:
* ```LIFECYCLE:CREATED```
* ```LIFECYCLE:KEEP_ON__&_REPLAY_SUBSCRIBED```
* ```LIFECYCLE:INIT_ON_SUBSCRIBED```
* ```LIFECYCLE:RESOLVE_RENDER_ON_CALLED```
* ```LIFECYCLE:LISTENS_TO_PLAY_AFTER_RENDER_SUBSCRIBED```
* ```LIFECYCLE:ON_RENDER_CALLED```
* ```LIFECYCLE:ON_RENDER_CAOMPLETE_CALLED```