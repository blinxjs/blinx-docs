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
    "moduleName": "MySuperAwesomeModule",
    "instanceConfig": {
        // Configuration for Module Instance
    }
});
```



## destroyInstance



