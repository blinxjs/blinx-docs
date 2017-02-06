## Blinx Providers

Blinx provides bare minimum functionalities to make it flexible to use.

However, some of the basic providers\(also called as global extensions\) are already created for use in order to enhance the functionality of Blinx. These are explained one by one below

### Logger Provider

Logger extension

Github link: [https://github.com/blinxjs/blinx-extensions/blob/master/src/logger-g-ext.js](https://github.com/blinxjs/blinx-extensions/blob/master/src/logger-g-ext.js)

This provider simply logs the lifecycle status of any module on to the console.

### Smart render provider

Github link: [https://github.com/blinxjs/blinx-extensions/blob/master/src/smart-render.js](https://github.com/blinxjs/blinx-extensions/blob/master/src/smart-render.js)

Only works for modules which have smart render functionality enabled. To enable smart render, the config should have `enableSmartRender` set as `true`

This extension internally uses set-dom library to update DOM and persist state.It has DOM to DOM diffing algorithm. By this extension only those part of DOM will be re-rendered which are changed. rest will not be changed.

### DOM event binding provider

Github link: [https://github.com/blinxjs/blinx-extensions/blob/master/src/bind-ext.js](https://github.com/blinxjs/blinx-extensions/blob/master/src/bind-ext.js)

\(uses Dom provider internally\)

This provider allows you to bind the DOM events via config of the module.

An example of config file below:

```
{
    modules: [],//no child modules
    domEvents: {
        "click": [
            {
                selectors: [".add-attr-button"],
                callback: "addItem"
            },
            {
                selectors: [".edit-value .edit"],
                callback: "editItem",
                extract: {
                    name: "getData#var"
                }
            },
            {
                selectors: [".updated-value"],
                callback: "updateValue",
                extract: {
                    name: "getData#var",
                    value: "val"
                }
            }
        ],
        "keyup": [
            {
                selectors: [".updated-value"],
                callback: "updateValue",
                extract: {
                    name: "getData#var",
                    value: "val",
                    event: "event"
                }
            }
        ]
    }
}
```

* To bind the events via the config, create a Json object with key `domEvents`  within the config. 
* The Dom events contains list of events that we want to listen to. 
* Each will be a Json with key being the Json name and value being an object with the array of handler details. 
* Each handler detail object will be of format below

```
{
                selectors: [""],//array of all the selectors
                callback: "updateValue", This method will be exported by the module
                extract: { // this object will be passed as an argument to the callback method
                    name: "getData#var",// this is a method in dom provider being used internally. argument after # 
                    value: "val",
                    event: "event" 
                }
            }
```

### DOM provider

Github link: [https://github.com/blinxjs/blinx-extensions/blob/master/src/dom-ext.js](https://github.com/blinxjs/blinx-extensions/blob/master/src/dom-ext.js)

The Dom provider helps us to get the DOM element from selctors and provides basic dom methods on them.

### Observer

Github link: [https://github.com/blinxjs/blinx-extensions/blob/master/src/observer.js](https://github.com/blinxjs/blinx-extensions/blob/master/src/observer.js)

The observer provider allows us to enable the observer pattern. Internally uses [https://www.npmjs.com/package/observa ](https://www.npmjs.com/package/observa)

Let us recall the module section earlier in the book. It says you can return template directly if not action to be done in the render method. By default the data passed to this template is `this.modulePlaceholders`

With this provider included, the default object which goes to the template becomes `this._` Everything in this.\_ object is observable. Any change in this object will call the methods exported in the main controller file.

Lets take an example of TODO app. \(You will find this code at [https://github.com/blinxjs/blinx-boilerplate/tree/master/src/apps/todos/todosComposite](https://github.com/blinxjs/blinx-boilerplate/tree/master/src/apps/todos/todosComposite)\)

This is a todo app which simply records the todo list remove item from list once they are checked.

The todo app contains four files namely

* config.js
* index.js
* style.css
* template.html

**template.html**

```
<div>
    <header class="header">
        <h1>todos</h1>
        <input class="new-todo" placeholder="What needs to be done?" value="{{view.inputText}}" autofocus="">
    </header>
    <section class="main">
        <input class="toggle-all" id="toggle-all" type="checkbox"/>
        <label for="toggle-all">Mark all as complete</label>
        <ul class="todo-list">
            {{#each todos}}
            <li data-id="{{id}}" class="{{#if completed}}completed{{/if}}">
                <div class="view">
                    {{#if completed}}
                    <input class="toggle" type="checkbox" data-id="{{id}}" checked="checked">
                    {{else}}
                    <input class="toggle" type="checkbox" data-id="{{id}}">
                    {{/if}}
                    <label>{{text}}</label>
                    <button class="destroy" data-id="{{id}}"></button>
                </div>
            </li>
            {{/each}}
        </ul>
    </section>
</div>
```

**config.js**

This file has smart render enabled and event bind provider is used to bind all events.

```
export default {
    enableSmartRender: true,
    domEvents: {
        "change": [{
            selectors: [".toggle"],
            data: ["id"],
            callback: "toggleOne",
            extract: {
                value: "is#:checked",
                id: "getData#id"
            }
        }],
        "keyup": [{
            selectors: [".new-todo"],
            callback: "changeinputText",
            extract: {
                text: "val"
            }
        },{
            selectors: [".new-todo"],
            which: [13],
            callback: "addTodo",
            extract: {
                text: "val",
                event: "event"
            }
        },{
            selectors: [".old-todo"],
            callback: "changeinputText",
            extract: {
                text: "val"
            }
        },{
            selectors: [".old-todo"],
            which: [13],
            callback: "addTodo",
            extract: {
                text: "val",
                event: "event"
            }
        }]
    }
};
```

**index.js**

```
import config from "./config"; //importing config
import template from "./template.html";//importing template

import "./style.css";

function resolveRenderOn(){
    this._.todos = []; //underscore object set to empty array

    this._.view = {
        inputText: "",
        todos: []
    }
}

function addTodo(todo) { // add todo method is called when a text is typed on input box 
    this._.todos.push({//and enter button is pressed
        text: todo.text,
        id: Date.now(),
        completed: false
    });
    this._.view.inputText = ""; //simply changing this will change remove text from input box
}

function toggleOne(data) {
    let todo = this._.todos.find((todo)=>{
        return (todo.id === data.id);
    });

    todo.completed = data.value;
}


function changeinputText(input) { //sets the text to the variable
    this._.view.inputText = input.text;
}

export default {
    config,
    addTodo,
    changeinputText,
    toggleOne,
    template,
    resolveRenderOn,
    observe_For: ["render"]
}
```

so basically both the index and config file together will make your logic clear. The dom bind on config is listening to events and calling methods in the index file. the index files changes the variable `this._` which in turn changes the dom. Now the crux lies in something written while exporting `observe_For` . This is an array of methods to be called when the observed objects are changed.

Here render is the method being called, meaning the page will be rendered again. 

We can add any other method which are exported in controller and they will be called once there is any change in `this._`  



