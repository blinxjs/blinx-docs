# What is Blinx?

Blinx is a utility belt which enforces a set of standard to ensure scalabilty and longivity of your application. It helps in building application which is

* Modular
* Configurable
* Event Based
* Abstracted
* Extensible

![](/assets/Screen Shot 2017-01-27 at 11.03.27 am.png)

## Prerequisites and Assumptions

### Audience

We assume that Blinx developers are:

* UI and UX engineers
* Software Developers

### Technology

We assume that the audience is familiar with the following technologies:

* JavaScript
* HTML
* CSS

## Why Blinx?

JavaScript single-page applications development and maintenance has increasingly become complex.

For large scale, long running applications, we have devised the Blinx framework to:

* Keep the tech stack shielded from changes in the universe allowing for easy change management
* Allow coding and configurations to build web application with less effort
* Be lightweight for quick web application loading
* Be easy to extend in the future
* Understand the unique requirements of individual applications
* Be easy to get started with
* Empower developers to jump-start application development work with minimal learning curve
* Reduce patching for framework incompatibilities

## Installation

Run the following command to install the latest version of Blinx:

```
npm install blinx
```

Else, include the following script file in your HTML:

```
<script src="https://npmcdn.com/blinx/dist/blinx.min.js"></script>
```

Or you can generate a new Blinx application using Yeoman generator

```
npm install -g yo
npm install -g generator-blinx
yo blinx
```

# Documentation

* [Principles Of Blinx](/principles.md)
* [BlinxJS Core API](/blinxjs-apis.md)
* [Modules](/modules.md)
* [Module Config](/module-config.md)
* [Events](/events.md)
* [Providers](/providers.md)

Or checkout how a [Blinx Application is structured](/blinx-methods.md) to provide longivity.

Or [try this](/hands-on-counter-application.md) to get you hands dirty.

