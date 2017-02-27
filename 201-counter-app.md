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

Let us get back to counter application now. We will be using 3 providers in this application namely:

1. bind-ext: To bind events using module configuration.

2. smart-render: This provider smartly differentiates between the two HTML blocks and figures out the changes which needs to be patched in DOM.

3. observer: Adds \_ over the module context. Whenever any data changes over \_, it calls the dependent function mentioned in configuration.

Get started and import these packages in our counter application. First we will need to install package for all these providers. All the three mentioned provider is part of blinx-extensions package. Go ahead and install that using npm

```
npm install blinx-extensions --save
```





