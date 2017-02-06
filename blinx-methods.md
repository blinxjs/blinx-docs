## Blinx Architechture

![](/assets/Screen Shot 2017-01-27 at 3.36.38 pm.png)

### Blinx as a whole

Blinx handles the lifecycle of the modules. In the diagram above, Blinx modules are search, Filter list and card. These modules interact with each other via an event bus. The events flow through event bus and are received by the subscriber modules. Every module \(like in other frameworks\) has the following things:  
Templates  
Styles  
Configuration  
Model layer for business logic or connecting to backend.  
Controller which is also the main file responsible for exporting all the methods or generating events.

Here controller is the most important and mandatory part of a module. The controller file creates the module. A blinx module ideally would contain some of the bLinx methods explained later and the methods exported by the controller file.



In a nutshell, any blinx application will have 

1. **Life cycle manager**
2. **Event management system**
3. **Module Utilities**
4. Blinx modules \(definition created by U!\)
5. Series of extensions \(over any third party library or component\)
6. Set of providers \(to enhance blinx!!\)

Out of these the bold ones are a part of core Blinx whereas the rest of them are left to the user to use.



