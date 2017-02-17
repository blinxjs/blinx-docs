# Events

In any Blinx application, its not recommended to refer any module directly. Communication between modules should be through events only. Events is the primary way for the modules to communicate.

Before diving into the usage of events, lets understand the types of events. These are basically three types of events

1. `KEEP_ON`

2. `REPLAY`

3. `PLAY_AFTER_RENDER`

###### PLAY\_AFTER\_RENDER

This event basically works the same way as normal events. If the module view is rendered after the event is published, the module will not listen to those events.

###### REPLAY

This event will be replayed to the subscribed module with all the data which occured in the past. For example the sequence of events is something like :

1. Event occurred
2. Event occurred
3. Event subscribed with type replay
4. Event occurred

In this case the subscribing module will not miss any events and it will occur thrice for that module.

###### KEEP\_ON

This kind of subscription will listen to the events even when the module has not been rendered. This way any event occurred before the rendering is not missed.

#### Subscribe Events

A module can subscribe for any event in two ways. To subscribe for any event we need to have answers of following points:

* Name of the event to which we want to subscribe.
* If we need to be specific about the source of event, we should be knowing the css selector of the source module.
* If we need to subscribe just for once and trash the subscription
* Which function of the module should be called when the event mentioned is triggered.
* Subscription behaviour based on type \(mentioned in Events section above\)

###### Static Subscriptions

Static subscription is the way to subscribe for any event through the module configuration. This is best suited when we exactly know the parameters of events which we want to subscribe \(means we have answers of above 5 question\).

listensTo is the attribute in config where we can configure the module subscription. Like:

```
listensTo: [{
    eventName: "ADD_TIMESTAMP",
    eventPublisher: '#header-container',
    callback: 'addTimestamp', // Method present over module context
    type: "KEEP_ON",
    once: false // true/false
}]
```

###### Dynamic Subscriptions

We can subscribe to any events through module's context. Its advisable to use this way of event subscription only if don't have answers of above 5 questions ahead of time.

```
this.subscribe({
    eventName: "ADD_TIMESTAMP",
    eventPublisher: '#header-container',
    callback: function(data){
    
    },
    type: "KEEP_ON",
    once: false // true/false
})
```

#### Publish Events

While subscribing to any event, the module can mention three types of subscription to the events.

