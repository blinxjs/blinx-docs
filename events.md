# Events

In any Blinx application, its not recommended to refer any module directly. Communication between modules should be through events only. Events is the primary way for the modules to communicate.

#### Subscribe Events

A module can subscribe for any event in two ways.

1. Through Config
2. Through Module

#### Publish Events

While subscribing to any event, the module can mention three types of subscription to the events.

These are basically three types of events

1. `KEEP_ON`

2. `REPLAY`

3. `PLAY_AFTER_RENDER`

#### PLAY\_AFTER\_RENDER

This event is basically works the same way as normal events. If the module view is rendered after the event is published, the module will not listen to those events.

#### REPLAY

This event will be replayed to the subscribed module with all the data which occured in the past. For example the sequence of events is something like :

1. Event occurred
2. event occurred
3. event subscribed with type replay
4. event occurred.

In this case the subscribing module will not miss any events and it will occur thrice for that module.

#### KEEP\_ON

This kind of subscription will listen to the events even when the module has not been rendered. This way any event occurred before the rendering is not missed.

