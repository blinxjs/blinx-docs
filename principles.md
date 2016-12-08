# Best Practices


#### Use Modules.
* **Blinx Modules** are building blocks of any Blinx-based application. These modules are reusable, self-contained, independently executable UI widgets. 
  * Every Blinx module should contain their own template and styles.
* Every element on a Blinx application page comes from a module.


#### Consume as you need.
* Add features using Blinx extensions.
  * **Global Extensions** are a powerful way to add required features across all modules in a Blinx application.
  * **Local Extensions** enable you to add specific features to a particular module. These extensions can be imported in any of the module file as needed.


#### Publish and subscribe.
* Use events to **publish data and subscribe data**.
  * Do not import or call another module's method.


#### Make it configurable.
* Make your **modules configurable**. Create a modifiable ```config``` file to maintain all reusable components of any module.


#### Expose less.
* Only expose methods/properties required by truss and module.
