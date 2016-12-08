# Modules

Modules are self contained units of Blinx applications that load all functional components and dependencies.

All views and behavior in a Blinx application page comes from some module. Typically, each section/area on the page is a module. For example, a Header module defines the look and feel and behavior of the header section of a page. It may consist of a Logo, Title, Username and Password fields, and Login button.

Blinx manages module lifecycle by invoking predefined functions in a predetermined sequence.

## Lifecycle

When a module is registered on Blinx, the module functions are invoked at various stages of the module's lifecycle.


![](ModuleLifecycle.png)