# Class: GC.Application

Inherits from
:    1. [event.Emitter](./event.html#class-event.emitter)

This document contains information on the classes and
instances that are initialized when the game engine is
created and the scene graph is drawn to the screen.

If you are just getting started with the DevKit, you should
first read our guides, in particular
[Hello World](../guide/hello-world.html) and the more
advanced [Game Walk-Through](../guide/game-walkthrough.html).

## Events

### initUI ()

A user defined function that is executed when the game
engine is set up and the scene graph is constructed. This
is called before the splash screen is removed and the
`launchUI` function is executed. This is where you should
put game setup code and construct the view hierarchy.

### launchUI ()

A user defined function that serves as the entry point for
applications built on the hashcube DevKit. This is executed
after the scene graph has been constructed, the `initUI`
function is called, and the splash screen has been
removed. This is where your game entry point logic should be
placed and the game loop set up.

### onPause ()

Executed when the application process is sent to the background.

### onResume ()

Executed when the application resumes from a backgrounded state.


# Singleton: GC.app

A reference to the running application. When the application
is instantiated, the scene graph hierarchy and rendering
engine are created, and the application's `initUI` and
`launchUI` methods are called.

Inherits from
:    1. {project}/src/Application ---The user defined application.
     2. [GC.Application](#class-gc.application)
     3. [event.Emitter](./event.html#class-event.emitter)


## Methods

### run ()

Renders the application's scene graph and starts the animation loop.


## Properties

### view `{ui.StackView}`

The root of the application's scene graph. All child views
attached to this are rendered to the screen.


# Class: ui.Engine

Manages a view hierarchy by rendering its scene graph and
controlling the animation loop. It has native, Canvas, and
DOM rendering backends.

Inherits from
:    1. [event.Emitter](./event.html#class-event.emitter)

~~~
import ui.Engine as Engine;
~~~


## Methods

### new Engine ([options])

Parameters
:    1. `options {object}`
	     * `width {number} = device.width` ---Used to set the width of the rendering context.
		 * `height {number} = device.height` ---Used to set the height of the rendering context.
		 * `alwaysRepaint {boolean} = false` ---Redraw the screen every animation tick. If the display requires continuous updates, you should set this to `true`.
		 * `clearEachFrame {boolean} = false` ---Clear the screen on each animation frame. If set to `false`, the image will be drawn on top of the previous one.
		 * `view {ui.View} = null` ---The root view of the scene graph.
		 * `showFPS {boolean} = false` ---Display the running speed of the animation in frames per second.
		 * `dtFixed {number} = 0` ---Fixed delta time.
		 * `dtMinimum {number} = 0` ---The minimum delta time to use between ticks in the game loop.
		 * `keyListenerEnabled {boolean} = true` ---Enable the listener to detect keyboard events.
		 * `continuousInputCheck {boolean} = true` ---Continually triggers the most recent `input:move` event on each tick. This allows for more responsive input. On a browser this defaults to `true`, and `false` on a mobile device.
		 * `repaintOnEvent {boolean} = true` ---Redraw the screen when a user event occurs, such as touch or click.
		 * `mergeMoveEvents {boolean} = false` ---Multiple move events in an animation frame are merged together by taking the last event.
		 * `noReflow {boolean} = false` ---Disable the reflow manager, can be done to improve performance if the application does not use layout properties like `flex`.
		 * `scaleUI {boolean} = false` ---If `true`, scale root view to 576x1024. If `scaleUI` is an array of two numbers, scale to these dimensions instead.

To tweak any of these parameters from your `Application.js` file, simply define `this._settings` as an object with one or more of these options.

### Class Method: Engine.get ()

Returns
:    1. Return: `{Application}`

If a rendering engine has been initialized, return
`GC.app.engine`, otherwise return `null`.


# Singleton: GC.app.engine

Inherits from
:    1. [ui.Engine](#class-ui.engine)
     2. [event.Emitter](./event.html#class-event.emitter)

The game engine initializes a number of components,
including the input and key event listeners, the game loop,
the view hierarchy, and rendering the scene graph. A single
`ui.Engine` is instantiated for games at `GC.app.engine`.


## Methods

### updateOptions (options)

Parameters
:    1. `options {object}`

Update the options of of the instantiated `ui.Engine`.

### getEvents ()

Returns
:    1. `{Array}`

Returns any events that haven't been dispatched by the engine yet.

### getCanvas ()

Returns
:    1. `{Canvas}`

Returns the internal Canvas element used for rendering. If
not in a browser, return a Canvas-like object. Not all of
the HTML5 API is available across all devices.

### getView ()

Returns
:    1. `{View}`

Returns the root view of the scene graph.

### setView (view)

Parameters
:    1. `view {ui.View}`

Returns
:    1. Return: `{this}`

Sets the root view of the scene graph.

### hide ()

Returns
:    1. `{this}`

Hide the root Canvas element of the scene graph.

### show ()

Returns
:    1. `{this}`

Display the Canvas element that contains the scene graph, but only when `useDOM` is turned off.

### pause ()

Pauses the game engine and disable the input listeners.

### resume ()

Resumes the game engine and enables the input listeners.

### stopLoop ()

Returns
:    1. `{this}`

Stops the engine loop timer.

### startLoop ([dtMin])

Parameters
:    1. `dtMin {number} = options.dtMinimum` ---Optional interval to use for the game loop timer.

Returns
:    1. `{this}`

Starts the engine loop timer.

### render ()

Renders the entire scene graph by emitting a `'Render'`
event to the rendering context.

### needsRepaint ()

Returns
:    1. `{this}`

Explicitly signals to the engine that the scene graph needs
to be redrawn.


## Events

### \'Tick\', callback (dt)

Parameters
:    1. `dt {number}`

Emitted on each frame of the game render loop.

For example, to keep track of the amount of frames rendered
over an application's lifetime:

~~~
var frame = 0;

GC.app.engine.on('Tick', function (dt) {
  frame += 1;
  console.log(frame);
});
~~~

### \'Render\', callback (context)

Parameters
:    1. `context {Context2D}` ---The internal canvas context.

This event is emitted every time the scene graph is drawn.
