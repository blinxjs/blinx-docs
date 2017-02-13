# APIs

As we have seen in Principles Of Blinx, modules are the building blocks of any Blinx application. Blinx manages the lifecycle of modules and stitches it together to build the application. BlinxJS provides two methods for the purpose

* createInstance
* destroyInstance

and are available over Blinx object.

## createInstance

_Blinx.createInstance_ method can be used to create an instance of Module and stitch in the page.

```
// Import Blinx object.
import Blinx from "blinx";

let onCreatePromise = Blinx.createInstance({
    "moduleName": "MySuperAwesomeModuleInstance",
    "instanceConfig": {

        //CSS selector where the rendered view of module instance should be attached.
        "container": "body",

        // other configurations for Module Instance
    },
    "module": MySuperAwesomeModule
});
```

Blinx.createInstance returns a promise which gets resolved once Module instance is created and stitched in DOM. In case it fails to do so, it will reject the promise.

```
onCreatePromise.then(function(rootModulesArr){

    // resolve function receives one argument of array type.
    // rootModulesArr is an array which contains unique ids of independent modules.


    // Function body
    // Resolved if created module instance.

}, function(){

    // Function body
    // Rejected if failed.
})
```

Resolve function block receives one argument of type array. This array contains unique ids of independent created module instances and this id can be used to destroy all the created module instances.

## destroyInstance

_Blinx.destroyInstance_ method can be used to destroy the module instance and remove its view from DOM. There are different ways to destroy specific modules.

* Destroy module instance using unique id returned in createInstance promise

```
onCreatePromise.then(function(rootModulesArr){

    rootModulesArr.forEach(function(id){
        Blinx.destroyInstance(id);
    });

}, function(){

    // Function body
    // Rejected if failed.
})
```

* Destroy module instance using moduleName provided while creating instance.

```
Blinx.destroyInstance({
    name: "MySuperAwesomeModuleInstance"
});
```

* Destroy using createIntance configuration

```
import Blinx from "blinx";
let instanceConf = {
    "moduleName": "MySuperAwesomeModuleInstance",
    "instanceConfig": {
        "container": "body"
    },
    "module": MySuperAwesomeModule
};
let onCreatePromise = Blinx.createInstance(instanceConf);


// At some later stage
Blinx.destroyInstance(instanceConf);
```



