# Scaling the Game to Fit the Device

In this guide you'll see how to scale your game in a way
that fits multiple device aspect ratios. This example is
part of the *examples* add-on, which can be installed in to
devkit with the following at your command line:

~~~
$ devkit install examples
~~~

In the devkit web interface, navigate to **Projects >
Examples > Layout: Scale Region** to run the example in the simulator.

It can also be [viewed online](../example/basics-scalescreen/)
along with the other examples. 

## Aspect Ratio

The aspect ratio of a screen describes the proportional
relationship between its width and its height.

Here's how the aspect ratios of 4:3 (green box) and 3:2 (red
box) can be fit within a 16:9 aspect ratio (blue box) while
preserving their aspect ratios and without stretching. Notice the
extra padding on the right and left margins of the green and
red boxes.

<img src="./assets/scaling/aspect-ratio-1.png" alt="aspect ratio 1" class="screenshot">

And here's how the 16:9 and 3:2 aspect ratios are confined
within a 4:3 ratio; now there is vertical padding at the top
and bottom:

<img src="./assets/scaling/aspect-ratio-2.png" alt="aspect ratio 2" class="screenshot">

In these diagrams the different ratios are centered within
the constraining view. If desired, the constrained ratio
could be positioned along an edge, leaving the extra padding
along the opposite edge.

Popular dimensions of different aspect ratios:

16:9
:    * 1280 × 720 (Galaxy Nexus)
     * 1136 × 640 (iPhone 5)
     * 1024 × 576
     * 720 × 405

3:2
:    * 1080 × 720 (High-definition TV)
     * 960 × 640 (iPhone 4)
     * 864 × 576
     * 480 × 320 (iPhone 3)

4:3
:    * 2048 × 1536 (iPad 3 & 4)
     * 1024 × 768 (iPad 1 & 2)
     * 960 × 720
     * 768 × 576
     * 427 × 320
     * 320 × 240


## Calculate Scaling

We're going to define the maximum displayable region for the
game as **576 x 1024 pixels**. This is because, on most
phones, the maximum width a texture can be is 1024
pixels. Game developers should work within this 576 x 1024
coordinate space, that way their in-game positions can be
consistent across all different aspect ratios by scaling
this space. We use these pixel coordinates because,
currently, the most extreme resolution on a popular phone is
720 x 1280, and we can easily scale our working dimensions up
by 25 percent to reach this: 576 * 1.25 = 720, 1024 * 1.25 = 1280.

Everything within this 576 x 1024 rectangle will be
displayed on the phone, regardless of the device
dimensions. Any art assets should be delivered relative to
these dimensions.

So, in our example code listed below, we'll define the
maximum displayable region for the visible portion of the game:

~~~
var boundsWidth = 576;
var boundsHeight = 1024;
~~~

Here, we've defined this region as 576 by 1024 pixels, and
as you'll see below, these are the dimensions used for the
background image in this example. This is a 16:9 aspect
ratio in portrait mode, the widest available on the more
popular phones as shown above.

We'll use these values to calculate the base dimensions and
scale used for the top-level views. This gives us a 576 x
864 rectangle that we'll use as the base of our game and any
screen coordinates for the in-game positions should be given
in these dimensions. In the the code below, we'll calculate
the scaling to apply to these coordinates that will fit it
to the device.

For portrait mode, here's how to calculate our base
rectangle dimensions and scale:

~~~
var baseWidth = boundsWidth; //576
var baseHeight =  device.screen.height * (boundsWidth / device.screen.width); //864
var scale = device.screen.width / baseWidth; //1
~~~

Replacing the variables for calculating height we get: 480 * (576 / 320) = 864.

And in landscape mode, we can simply reverse these calculations:

~~~
var baseWidth = device.screen.width * (boundsHeight / device.screen.height); //864
var baseHeight = boundsHeight; //576
var scale = device.screen.height / baseHeight; //1
~~~

### Scale the Top-Level View

Scaling a view ensures that all its child views are also
scaled. To center the game views on a screen, set the size
and scale of the root view in the scene graph, which is `GC.app.view`:

~~~
GC.app.view.style.scale = scale;
~~~

You can also position the overflow in a particular direction
by offsetting these positions.


### Positioning Game Assets

With the base rectangle set, we can dynamically position
game assets on the screen so that they maintain their
relative position regardless of the device's aspect
ratio. For example, to position a sprite in the middle the
background image, simply set it's x and y position to half
of the base region's width and height:

~~~
var sprite = new SpriteView({
  superview: background,
  x: baseWidth / 2,
  y: baseHeight / 2,
  width: 300,
  height: 300,
  url: "resources/images/sdkBot/sdkBot",
  defaultAnimation: 'idle',
  autoStart: true
});
~~~


### Art Deliverables

Since we've established the displayable region of our game
to the screen coordinates of 576 x 1024 pixels, this should
be the dimensions that art assets get delivered. 1024
pixels is the maximum width of a texture on many devices,
and we can then scale it up or down to fit the dimensions of
other devices.


### Example: Scale a Background Image and Game Asset to Fill Screen

In this example we'll create a scene that fits multiple
devices. First, here's how it looks on the iPhone simulator:

<img src="./assets/scaling/scale-iphone.png" alt="scaling iphone screenshot" class="screenshot">

By switching the device simulator to a larger device---like
the Galaxy Nexus tablet---we can see how this scene is
displayed to fill the entire screen:

<img src="./assets/scaling/scale-tablet.png" alt="scaling tablet screenshot" class="screenshot">

And here's the code to set up the background `ImageView` and
our sprite that walks along the lunar surface:

~~~
import device;
import ui.ImageView as ImageView;
import ui.SpriteView as SpriteView;

var boundsWidth = 576;
var boundsHeight = 1024;
var baseWidth = boundsWidth;
var baseHeight = device.screen.height * (boundsWidth / device.screen.width); //864
var scale = device.screen.width / baseWidth;
var rightBoundary = baseWidth; //right boundary for screen wrapping
var leftBoundary = 0;
var vx = 0;

exports = Class(GC.Application, function () {

  this.initUI = function () {
    
    //scale the root view
    this.view.style.scale = scale;
    
    var background = new ImageView({
      superview: this.view,
      x: 0,
      y: 0,
      width: baseWidth,
      height: baseHeight,
      image: "resources/images/background.jpg", //576x1024
      zIndex: 0
    });

    var sprite = new SpriteView({
      superview: background,
      x: baseWidth/2,
      y: baseHeight - 400,
      width: 300,
      height: 300,
      url: "resources/images/sdkBot/sdkBot",
      defaultAnimation: 'idle',
      autoStart: true,
      zIndex: 1
    });

    //sprite movement is determined by mouse position relative to the sprite
    this.view.on('InputSelect', function (evt, pt) {
      //localize point to sprite, which is one-level deep
      var x0 = sprite.style.x + sprite.style.width/2,
          y0 = sprite.style.y + sprite.style.height/2;

      //if the sprite is clicked, stop movement and return to idle animation
      if (sprite.containsLocalPoint({x: pt.x - sprite.style.x, y: pt.y - sprite.style.y})) {
        if (sprite.isPlaying) {
          vx = 0;
          sprite.resetAnimation();
        }
      } else if (pt.x < sprite.style.x + sprite.style.width/2) {
        //walk left
        vx = -2;
        sprite.stopAnimation();
        if (sprite.style.flipX) { sprite.style.flipX = false; }
        sprite.startAnimation('walk', {loop: true});
        
      } else {
        //walk right
        vx = 2;
        sprite.stopAnimation();
        if (!sprite.style.flipX) { sprite.style.flipX = true; }
        sprite.startAnimation('walk', {loop: true});
      }
    });

    GC.app.engine.on('Tick', function () {
      //add horizontal movement
      sprite.style.x += vx;

      //screen wrapping
      if (sprite.style.x > rightBoundary) {
        sprite.style.x = leftBoundary - sprite.style.width;
      } else if (sprite.style.x + sprite.style.width < leftBoundary) {
        sprite.style.x = rightBoundary;
      }
    });
  };
});
~~~
