# Principles of Blinx

To create any application with Blinx, there are some basic principles. These principles allow us to easily create highly extensible, manageable applications without any hassle.

#### Make your application modular

The idea is to break down the application into small, reusable and independently testable components. These components do not contain any reference to any other module. This gives us the facility to plug and play any module we need. We can think of our whole application as something like a lego structure.

* **Modules** are the **basic building blocks** of any Blinx-based application. These modules are reusable, self-contained, independently executable and independently testable UI widgets.
* Every Blinx module should contain their own template and styles.
* Every element on a Blinx application page comes from a module.
* There are two types of modules: simple and composite. 
* The composite modules are a collection/group of simple or composite modules.

Every single block below can be called as a module

![](/assets/Screen Shot 2017-01-27 at 12.02.49 pm.png)

#### Make your modules configurable

Create a modifiable "config" file to maintain all reusable components of any module.

We want to reuse our modules as far as possible. For this, every module should be configurable enough to be re used. Blinx allows you to have a separate configuration file for a module. This is very similar to ‘options’ we provide to any Jquery component. For example, you want to create a navigation view, It should be configurable to create a vertical/horizontal navigation or the options provided to the navigation pane are provided via its config.

In the case of composite modules, all the child modules which it contain can also be a part of configuration. Example you want to create a generic list view which displays a list of items on the screen. Now each item itself can be a module too. and these items are passed to the list view in a config.

#### Event Driven Communication

![](/assets/Screen Shot 2017-01-27 at 12.16.19 pm.png)

We have mentioned before that no module contains a reference to any other module. But these modules need to interact with each other for data. So the principle here is the modules will always interact with each other via events. These events are Blinx events. Events are open-ended in nature. If an event is fired, it’s not necessary that someone is listening to that event. Similarly, if some module has subscribed to that event, it's again not mandatory to have the event being fired. This makes our application more flexible. we can actually replace or enhance our modules without being worried about dependent modules. We just need to take care that it is generating same events.

#### Abstraction from third party components

Blinx advocates to create an abstraction layer to import the functionality from any 3rd party library. Only the APIs required by your application should be exposed through this abstraction layer. We call this abstraction layer  "Extensions".

* Import any 3rd party library for your application.
* Expose the methods from 3rd party library to your application.

#### Application should be Extensible

This is the last but equally important principle. Blinx understands that **requirement varies from project to project**. And that's the reason Blinx allows you to modify any of its existing feature or add your own. Providers are the way to enrich Blinx with new feature.

