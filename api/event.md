# Class: event.Emitter

The Game Closure DevKit provides several mechanisms for
handling events that allows cross-object communication. To
see what type of events a particular class supports, a
list is provided in that component's API documentation. For
example, here are the [available events](./ui-view.html#events) 
on all objects that inherit from `ui.View`.

The event emitter follows a common message passing pattern
known as [publish-subscribe](http://en.wikipedia.org/wiki/Publish–subscribe_pattern),
where objects communicate with each other by sending
events. An event is labeled by a text string, and any object
that inherits from `event.Emitter` can subscribe to this
event and execute a callback function when it receives the
event. Any number of callbacks can be associated with a
particular event, allowing multiple listeners to register
callbacks for the event without the emitter object knowing
anything about who the listeners are or what the callbacks
do. Many classes in the Game Closure DevKit inherit from
`event.Emitter` which allows the developer to listen for
certain events and run code at a given time.

## Examples

* [Subscribe to an event](../example/events-on/)
* [Subscribe to an event once](../example/events-once/)


## Methods

### new Emitter ()

Create an Emitter instance.

~~~
import event.Emitter as Emitter;

var emitter = new Emitter();
~~~

Create a new class which inherits from `event.Emitter`:

~~~
var Dog = Class(Emitter, function (supr) {
  this.init = function () {
    supr(this, 'init', arguments);
  };
});

var monty = new Dog();

monty.on('mailman', function (event) {
  console.log("Woof-woof!");
});
~~~

### emit (type [, args ...])

Parameters
:    1. `type {string}` ---The name of the event type.
     2. `args {...*}` ---Optional arguments to pass to the subscriber's handler function.

Returns
:    1. `{boolean}`

Emit an event to the object. Any of its handler functions
subscribed to the given event type are executed. Returns
`true` if the event was handled, otherwise `false`.

Using the previously defined `Dog` (which inherits from
`Emitter`), its listener callback can be triggered by
publishing an event it's subscribed to:

~~~
monty.emit('mailman'); //=> console prints "Woof-woof!"
~~~

### on (type, callback)

*Alias: `addListener`*

Parameters
:    1. `type {string}` ---The name of the event type.
	 2. `callback {function|string}` ---The dispatch function, or method name on an object.

Returns
:    1. Return: `{this}`

Add a callback function that is subscribed to a given event
type. The function will execute when that event is emitted
to the object. The method `addListener` is an alias for `on`.

~~~
var emitter = new Emitter();

emitter.on('add', function (a, b) {
  var sum = a + b;
  console.log("The sum is: " + sum);
});

emitter.emit('add', 3, 5);  //=> true (console prints "The sum is 8")
~~~

### once (type, callback)

Parameters
:    1. `type {string}` ---The name of the event type.
     2. `callback {function|string}` ---The dispatch function, or method name on an object.

Returns
:    1. `{this}`

Subscribe to an event, and remove the listener after it is received once.

~~~
emitter.once('one-time', callback);

emitter.emit('one-time'); //=> true
emitter.emit('one-time'); //=> false
~~~

### removeListener (type, callback)

Parameters
:    1. `type {string}` ---The name of the event type.
	 2. `callback {function|string}` ---The dispatch function, or method name on an object.

Returns
:    1. `{this}`

Unsubscribe a listener. This function will no longer execute
when its event type is emitted to the object.

~~~
emitter.on('myevent', callback);
emitter.emit('myevent');                     //=> true
emitter.removeListener('myevent', callback);
emitter.emit('myevent');                     //=> false
~~~

### removeAllListeners (type)

Parameters
:    1. `type {string}` ---The name of the event type.

Returns
:    1. `{this}`

Unsubscribe all listeners for a given event type.

~~~
emitter.on('myevent', callback);
emitter.on('myevent', callback);

emitter.removeAllListeners('myevent');
emitter.emit('myevent');               //=> false
~~~

### listeners (type)

Parameters:
:    1. `type {string}` ---The name of the event type.

Returns
:    1. `{array}`

Returns an array of event handler functions for a given
event type. This is a mutable reference to the array,
changes to it will affect the emitter.

~~~
emitter.on('myevent', callback);

emitter.listeners('myevent');  //=> [callback]
~~~

### setMaxListeners (n)

Parameters:
:    1. `n {number}`

By default, a warning message will display in the console if
more than 10 listeners are added to an object, which may
indicate a potential memory leak. Use this method to
increase this limit.

~~~
emitter.setMaxListeners(20);
~~~

## Events

### \'newListener\', callback (type, handler)
1. `type {string}`
2. `handler {function}`

Emitted when a new listener is added to the object.


# Class: event.input.InputEvent

Input events provide an object that encapsulates data about
the input gesture that created it. For example, when the
`'InputSelect'` event fires, a developer will want to know
when, where, and which view the user touched. The objects
provided to the handler functions of an event listener are
instances of `event.input.InputEvent`

When an input event is created it is passed sequentially up
a hierarchy of `ui.View` objects, with the top-most view being
the root of application.

Events are assigned a *target*, the view where it occurred,
and a *root*, the root view where it's dispatched. Event
propagation has two phases: *capturing* and *bubbling*. Capturing
occurs when the event starts at the root node and follows the
view hierarchy down to the target, whereas bubbling starts at the target node and
follows the view hierarchy up to the root. You may attach a callback
during the capturing phase by appending `'Capture'` after the event name.  
  
Usage of an input handler would look like:  

~~~
view.on('InputSelect', function (evt, pt) {
  ...
});
~~~  

More information on the types of input events which can
occur can be found in [ui.View](./ui-view.html) and
[ui.widget.ButtonView](./ui-widget-buttonview.html).

~~~
import event.input.InputEvent as InputEvent;
~~~


## Methods

### new InputEvent (id, type, x, y, root, target)

Parameters
:    1. `id {string}` ---Unique ID representing the input, useful for multi-touch input events.
	 2. `type {number}` ---Represents the type of event which occurred.
	 3. `x {number}` ---The x-coordinate position of the input event.
	 4. `y {number}` ---The y-coordinate position of the input event.
	 5. `root {View}` ---Root view which the input event bubbled up to.
	 6. `target {View}` ---The view which the input event occurred on.

### clone ()

Returns
:    1. `{InputEvent}`

Return a new `InputEvent` using this event's property values.

### cancel ()

Cancel an event from propagating further.


## Properties

### id `{number}`

Each input type is identified by a unique ID, for example,
the mouse will have the same identifier. For multi-touch
input, there will be a distinct ID for each finger
throughout a complete input event cycle (such as moving several 
fingers across a device at the same time).  This way you can effectively
manage multi-touch events which occur (for actions such as
multi-touch gestures, i.e. pinching, multi-finger swiping, etc).

### type `{number}`

A integer value 0-4 representing an event type. An event
type can be one of the following enumerable values:

~~~
from event.input.dispatch import eventTypes;
~~~

* `eventTypes.START {number} = 1` ---Event input has started.
* `eventTypes.MOVE {number} = 2` ---Event input has moved.
* `eventTypes.SELECT {number} = 3` ---A view has been selected by input.
* `eventTypes.SCROLL {number} = 4` ---A view has been scrolled.
* `eventTypes.CLEAR {number} = 5` ---An event is no longer over the active input.

### srcPoint `{Point}`

Object containing `x` and `y` coordinate position where the
event was dispatched.

### root `{View|null}`

Top view where event is dispatched (the tree root).  When the input event is bubbled up, this is where the bubbling ends.

### target `{View|null}`

Bottom view where the event occurred.  This should be the view which the
handler is being registered on.

### when `{number}`

Elapsed time (in milliseconds) from when the app started.

### depth `{number} = 0`

Number of levels of the tree from root to target.

### cancelled `{boolean} = false`

Check if an event has been canceled, and therefore, will not
propagate further.


# Class: event.Callback

Construct and chain callbacks for asynchronous flow
control. For advanced features with a more concise syntax,
see the [ff Node.js module](https://github.com/hashcube/ff).

Some APIs do not care when an event has fired, or even if it
has fired already. An example of this is preloading an
image. You need to register a callback for when the image
has loaded, but you don’t want to write:

~~~
if (image.isLoaded()) {
  renderImageToBuffer();
} else {
  image.on('load', renderImageToBuffer);
}
~~~

each time you care about the load event. For something like
this, you can wrap the underlying `load` event with an
instance of `event.Callback`. This class queues up callback
functions to be run when an event fires. Any callbacks
queued up after the event has fired will be immediately
executed. Now, the code can read:

~~~
image.doOnLoad(renderImageToBuffer); //Fires immediately if the image has already loaded
~~~

## Methods

### new Callback ()

~~~
import event.Callback as Callback;

var callback = new Callback();
~~~

### run ([thisArg,] callback [, args ...])

Parameters
:    1. `thisArg {object}` ---Optional object to use as `this` in the callback function, defaults to the callback itself.
	 2. `callback {function}`
	 3. `args {...*}` ---Arguments to pass to the callback function.

Returns
:    1. `{this}`

Add a callback to the callback chain by specifying the
object and the method to call within that object. If
callback has fired, run it straight away.

~~~
var cb = new Callback();
cb.run(function (name) { console.log("Hi, " + name); });
cb.fire("Louis"); //=> "Hi, Louis"

cb.run(function () { console.log("Runs immediately!"); }); //=> "Runs immediately!"
~~~

If arguments are curried to the function then the `thisArg`
object is required.

~~~
function meet_and_greet (a, b) {
  console.log(a + " say hi to " + b);
}
var cb = new Callback();
cb.run(null, meet_and_greet, "Carl");
cb.fire("Tim"); //=> "Carl say hi to Tim"
~~~

### runOrTimeout (onFire, onTimeout, duration)

Parameters
:    1. `onFire {function}` ---Run if the callback is fired before the timeout.
     2. `onTimeout {function}` ---Executed if the timeout occurs before the callback is fired.
	 3. `duration {number}` ---Amount of milliseconds before a timeout occurs.

Run two separate callbacks depending on whether the
specified timeout occurs or the callback is triggered.

~~~
var cb = new Callback();
cb.runOrTimeout(function () {
  console.log("Run if fired before 5 seconds.");
}, function () {
  console.log("Run after 5 seconds.");
}, 5000);
~~~

### chain ()

Returns
:    1. `{function}`

Chaining allows the loaded callback functions to run *after* another
function is explicitly executed. Each call to `chain` returns
a function that acts as a trigger, and when every trigger is
called, the callback functions execute.

This can be used for managing the flow of asynchronous
tasks, such as loading external resources or making
connections over a network.

~~~
var cb = new Callback();
cb.run(function () {
  console.log("Resources loaded!");
});

var fn1 = cb.chain(),
    fn2 = cb.chain();

function load_images () {
  do_load_images():
  fn1();
}

function load_sounds () {
  do_load_sounds():
  fn2();
}

load_sounds();
load_images(); //=> "Resources loaded!"
~~~

### fire ([args ...])

Parameters
:    1. `args {...*}`

Fire all the registered callbacks. Any arguments passed to
this will be passed into the registered callbacks.

### hasFired ()

Returns
:    1.	`{boolean}`

Check if the callback chain has been fired.

### reset ()

Allow the callback chain to be fired again.

### clear ()

Clears all of the registered callbacks and executes `callback.reset ()`.


# Drag Events: event.input.drag

You can make any `View` instance draggable like so:

~~~
import ui.ImageView; // any View subclass will work
import event.input.drag as drag;

// ...

var imageView = new ImageView({
    superview: someSuperView,
    image: 'resources/images/someImage.png',
    autoSize: true
});

drag.makeDraggable(imageView);
~~~

And voila, your `ImageView` is draggable. You can make any instance of a `View` or `View` subclass draggable with `drag.makeDraggable`.

## event.input.drag.makeDraggable (view, opts)

Parameters
:    1. `view {view}` ---A View instance.
     2. `opts {object}` ---Optional arguments to modify the drag behavior of the view.
     * `radius {number} = 10` ---How far the pointer must move to trigger a drag.
     * `unbound {boolean} = false` ---Whether the view can be dragged outside of its superview (superview is larger than view).
     * `cover {boolean} = false` ---Whether the superview must always be covered by the view (superview is smaller than view).

Returns
:    1. `{view}`

## Example

* [Draggable views](../example/events-input-drag/)


# Rotation Events

You can subscribe to rotation events like this:

~~~
import device;

device.setRotationHandler(function(e) {
    console.log(e.orientation);
});
~~~

`e.orientation` is one of:
:    1. `portrait`
     2. `portraitUpsideDown`
     3. `landscapeRight`
     4. `landscapeLeft`
     5. `unknown`
     6. `faceUp` -- iOS only
     7. `faceDown` -- iOS only


# Back Button

Want to know when the back button is pressed on an Android device? Here's how to find out:

~~~
import device;

device.setBackButtonHandler(function() {
    console.log("someone hit the back button!");
});
~~~

If your handler returns `false`, the code will background your application.