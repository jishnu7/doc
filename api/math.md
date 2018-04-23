# Module: math.util

Math is an important aspect of any game and the modules
below provide assistance with many common tasks. Modules are
included that provide geometric primitives for representing
in game objects and functions to handle collisions between
them. For additional convenience, utility functions for
numbers, arrays, and angles are provided. [Source code](https://github.com/hashcube/js.io/blob/master/packages/math/util.js)

~~~
import math.util as util;
~~~

## Methods

### interpolate (a, b, t)

Parameters
:    1. `a {number}`
     2. `b {number}`
	 3. `t {number}`

Returns
:    1. `{number}`

Returns the linear interpolation between values `a` and `b`
by the amount `t`.  Usually, `t` is a decimal value that
ranges between 0 and 1. The result of the interpolation is
`a` when `t = 0` and `b` when `t = 1`.

~~~
util.interpolate(0,10,0);   //=> 0
util.interpolate(0,10,0.5); //=> 5
util.interpolate(0,10,1);   //=> 10
~~~

### random (a, b [, generator])

Parameters
:    1. `a {number}`
     2. `b {number}`
	 3. `generator {function} = Math.random`

Returns
:    1. `{number}`

Generate and return a random integer between `a` to `b`
(excluding `b`). Note that `a` and `b` should be integer
values. Optionally, a random generator function can be
provided with the same signature as Math.random().

~~~
util.random(0,2);  //=> 0 
util.random(0,2);  //=> 1  
util.random(1,10); //=> 3
~~~

### clip (n, min, max)

Parameters
:    1. `n {number}`
	 2. `min {number}`
	 3. `max {number}`

Returns
:    1. `{number}`

Clips the value `n` to be within the range `min` to
`max`. If `n` is larger than `max`, then `max` is
returned. If `n` is smaller than `min`, then `min` is
returned. Otherwise, `n` is returned.

~~~
util.clip(10,0,20); //=> 10 
util.clip(-3,0,20); //=> 0  
util.clip(55,0,20); //=> 20  
~~~

### sign (n)

Parameters
:    1. `n {number}`

Returns
:    1. `{number}` ---Possible values are `1`, `-1`, or `0`.

Returns `1` if the value for `n` is positive, and `-1` if it
is negative. If `n` is zero, return `0`.

~~~
util.sign(40);  //=> 1
util.sign(-30); //=> -1  
util.sign(0);   //=> 0  
~~~


# Module: math.array

Basic functions for operating on one or more arrays. [Source code](https://github.com/hashcube/js.io/blob/master/packages/math/array.js)

~~~
import math.array as array;
~~~

## Methods

### average (array [, count])

Parameters
:    1. `array {array}`
	 2. `count {number} = array.length`

Returns
:    1. `{number}`

Compute the average of an array. `count` is an optional
parameter which specifies the number of elements to
average. By default, the entire array is averaged.

~~~
array.average([5,4,2,3,1]);   //=> 3
array.average([5,4,2,3,1],2); //=> 4.5
~~~

### weightedAverage (array, weight [, count])

Parameters
:    1. `array {array}`
	 2. `weight {number}`
	 3. `count {number} = array.length`

Returns
:    1. `{number}`

Compute the average of an array and weighs the result by
`weight`. `count` is an optional parameter which specifies
the number of elements to average. By default, the entire
array is averaged.

~~~
array.weightedAverage([5,4,2,3,1],0.5); //=> 1.5
array.weightedAverage([5,4,2,3,1],4,2); //=> 18
~~~

### subtract (array1, array2)

Parameters
:    1. `array1 {array}`
	 2. `array2 {array}`

Returns
:    1. `{array}` ---Returns `b - a`.

Subtracts the elements in `array2` from the elements in
`array1` and returns a new array containing the
differences. The length of `array1` should not be longer
than the length of `array2`.

~~~
array.subtract([5,4],[3,2]);     //=> [2,2]  
array.subtract([3,1],[0.5,2.1]); //=> [2.5,-1.1] 
~~~

### stddev (array [, count])

Parameters
:    1. `array {array}`
	 2. `count {number} = array.length`

Returns
:    1. `{number}`

Compute the sample standard deviation of the values in
`array`. Count is an optional parameter which specifies the
number of elements to use in the calculation. By default,
the entire array is used.

~~~
array.stddev([1,2,3,4,5]);   //=> 1.5811388300841898
array.stddev([1,2,3,4,5],2); //=> 0.7071067811865476
~~~

### shuffle (array [, seed])

Parameters
:    1. `array {array}`
	 2. `seed {number} = null`

Returns
:    1. `{array}` ---Returns the original array shuffled.

Randomly shuffles an array in-place, which means that
`array` is modified. The shuffled array is also returned for
convenience.

~~~
array.shuffle([2,3,1,5]); //=> [5,3,1,2]
array.shuffle([2,3,1,5]); //=> [3,5,2,1]
~~~

### rotate (array, count)

Parameters
:    1. `array {array}`
	 2. `count {number}`

Returns
:    1. `{array}`

Rotates `array`'s elements left by the amount specified by
`count`. If `count` is negative, then the elements are
rotated to the right. The rotated array is returned as a new
array.

~~~
array.rotate([1,2,3,4,5],1);  //=> [2,3,4,5,1]
array.rotate([1,2,3,4,5],2);  //=> [3,4,5,1,2]
array.rotate([1,2,3,4,5],-1); //=> [5,1,2,3,4]
~~~


# Class: math.geom.Point

An object with `x` and `y` numeric properties, typically
representing coordinates on a Cartesian plane. In many cases
it refers to a location on a device's screen, where the
position `{x: 0, y: 0}` is the top-left corner. [Source code](https://github.com/hashcube/js.io/blob/master/packages/math/geom/Point.js)

~~~
import math.geom.Point as Point;
~~~

## Methods

### new Point ([x, y])

Parameters
:    1. `x {number} = 0`
	 2. `y {number} = 0`

Creates a point with the given `x` and `y` values. Both values are optional and default to 0.

~~~
var point = new Point(0, 1); //=> {x: 0, y: 1}
~~~

### new Point (pt)

Parameters
:    1. `pt {object}`
	     * `x {number} = 0`
		 * `y {number} = 0`

Creates a new Point with `x` and `y` values equal to those in `pt`.

~~~
var point = new Point({x: 10, y: 3}); //=> {x: 10, y: 3}
~~~

### rotate (radians)

Parameters
:    1. `radians {number}`

Returns
:    1. `{this}`

Rotates `point` around the origin by a value in radians given by `radians`.

~~~
var point = new Point(0,1);
point.rotate(3.14159); //=> {x:0, y:-1} 
point.rotate(1.57079); //=> {x:1, y:0}
~~~

### translate (x, y)

*Alias: `add(x, y)`*

Parameters
:    1. `x {number}`
	 2. `y {number}`

Returns
:    1. `{this}`

Translates `point` by the given `x` and `y` values. `x`
indicates how much the point should move in the x direction
and `y` indicates how much it should move in the y
direction.

~~~
var point = new Point(1,3);
point.translate(3,5); //=> {x:4, y:8}
point.translate(0,2); //=> {x:4, y:10}
~~~

### translate (pt)

*Alias: `add(pt)`*

Parameters
:    1. `pt {Point}`

Returns
:    1. `{this}`

Translates `point` by the given point `pt`. `pt.x` indicates
how much the point should move in the x direction and `pt.y`
indicates how much it should move in the y direction.

~~~
var point = new Point(1,3);
point.translate(new Point(3,5)); //=> {x:4, y:8}
point.translate(new Point(0,2)); //=> {x:0, y:10}
~~~

### subtract (x, y)

Parameters
:    1. `x {number}`
	 2. `y {number}`

Returns
:    1. `{this}`

Subtract `x` and `y` from the x and y values of `point`.

~~~
var point = new Point();
point.subtract(4,3); //=> {x: -4, y: -3}
point.subtract(9,0); //=> {x: -9, y: 0}
~~~

### subtract (pt)

Parameters
:    1. `pt {Point}`

Returns
:    1. `{this}`

Subtract the point `pt` from `point`.`pt.x` is subtracted
from from the x value of `point` and `pt.y` is subtracted
from the y value of `point`.

~~~
var point = new Point(1,2);
point.subtract(new Point(3,4)); //=> {x: -2, y: -2}
point.subtract(new Point(1,5)); //=> {x: -3, y: -7}
~~~

### scale (factor)

Parameters
:    1. `factor {number}`

Returns
:    1. `{this}`

Scale x and y values of point by the given factor.

~~~
(new Point(3,6)).scale(3);   //=> {x: 9, y: 18}
(new Point(3,6)).scale(1.5); //=> {x: 4.5, y: 9}
(new Point(3,6)).scale(0);   //=> {x: 0, y: 0}
~~~

### normalize ()

Returns
:    1. `{number}`

Sets the magnitude of `point` to one, while keeping its
direction constant. Magnitude can be thought of as the
distance from the point to the origin.

~~~
(new Point(2,2)).normalize();  //=> {x: 0.7071, y: 0.7071}
(new Point(3,5)).normalize();  //=> {x: 0.51449, y: 0.85749}
(new Point(0,-5)).normalize(); //=> {x: 0, y: -1}
~~~

### setMagnitude (mag)

Parameters
:    1. `mag {number}`

Returns
:    1. `{this}`

Set the magnitude of this point, while keeping its
direction/angle constant. Magnitude can be thought of as the
distance from the point to the origin.

~~~
(new Point(1,2)).setMagnitude(4); //=> {x: 1.78885, y: 3.57770}
(new Point(3,5)).setMagnitude(2); //=> {x: 1.02899, y: 1.71498}
(new Point(0,-5)).setMagnitude(0.5); //=> {x: 0, y: -0.5}
~~~

### addMagnitude (mag)

Parameters
:    1. `mag {number}`

Returns
:    1. `{this}`

Add a magnitude to `point`, while keeping its direction/angle constant

~~~
(new Point(1,2)).addMagnitude(4); //=> {x: 2.78885, y: 5.57770}
(new Point(3,5)).addMagnitude(2); //=> {x: 4.028991, y: 6.714985}
(new Point(0,-5)).addMagnitude(0.5); //=> {x: 0, y: -4.5}
~~~

### getMagnitude ()

Returns
:    1. `{number}`

Returns the magnitude of `point`. Magnitude can be thought
of as the distance from the point to the origin.

~~~
(new Point(1.78885,3.57770)).getMagnitude(); //=> 4
(new Point(1.02899,1.71498)).getMagnitude(); //=> 2
(new Point(0,-0.5)).getMagnitude(); //=> 0.5
~~~

### getSquaredMagnitude ()

Returns
:    1. `{number}`

Returns the magnitude of `point` squared. Magnitude can be
thought of as the distance from the point to the origin.

~~~
(new Point(1.78885,3.57770)).getSquaredMagnitude(); //=> 16
(new Point(1.02899,1.71498)).getSquaredMagnitude(); //=> 4
(new Point(0,-0.5)).getSquaredMagnitude(); //=> 0.25
~~~

### getAngle ()

*Alias: `getDirection()`*

Returns
:    1. `{number}`

Returns the angle of a point in radians. The angle is
measured counter-clockwise from the positive x axis. If
`point.y` is greater than or equal to 0, the angle
measurement is positive and ranges from 0 to 3.14159. If
`point.y` is less than 0, the angle measurement is negative
and ranges from 0 to -3.14159.

~~~
(new Point(1,0)).getAngle();  //=> 0
(new Point(-1,0)).getAngle(); //=> 3.14159
(new Point(0,1)).getAngle();  //=> 1.57079
(new Point(0,-1)).getAngle(); //=> -1.57079
~~~


# Class: math.geom.Line

A class for representing a line in a 2D plane. A line
consists of a start point and end
point. [Source code](https://github.com/hashcube/js.io/blob/master/packages/math/geom/Line.js)

~~~
import math.geom.Line as Line;
~~~

## Methods

### new Line (x1, y1, x2, y2)

Parameters
:    1. `x1 {number}`
	 2. `y1 {number}`
	 3. `x2 {number}`
	 4. `y2 {number}`

Creates a new line that starts from the point at (`x1`, `y1`) and ends at (`x2`, `y2`).

~~~
var line1 = new Line(0,0,3,4); //=> {start: {x: 0, y: 0}, end: {x: 3, y: 4}}

var line2 = new Line(2,1,0,5); //=> {start: {x: 2, y: 1}, end: {x: 0, y: 5}}
~~~

### new Line (point, point)

Parameters
:    1. `point1 {Point}`
	 2. `point2 {Point}`

Creates a new line that starts from the point at `point1`
and ends at `point2`.

~~~
var line1 = new Line(new Point(3,5),new Point(2,1)); //=> {start: {x: 3, y: 5}, end: {x: 2, y: 1}}

var line2 = new Line(new Point(0,1),new Point(9,4)); //=> {start: {x: 0, y: 1}, end: {x: 9, y: 4}}
~~~

### getLength ()

*Alias: `getMagnitude()`*

Returns
:    1. `{number}`

Returns the length of a line.

~~~
var line = new Line(0,0,2,3)
line.getLength() => 3.60555
~~~

## Properties

### start `{Point}`

Member variable containing the point of the line

~~~
var line = new Line(0,0,2,3);
line.start //=> {x: 0, y: 0}
~~~

### end `{Point}`

Member variable containing the start point of the line

~~~
var line = new Line(0,0,2,3);
line.end //=> {x: 2, y: 3}
~~~


# Class: math.geom.Circle

Inherits from
:    1. [math.geom.Point](#class-math.geom.point)

Class used for representing circles in a 2D plane. Functions
are provided to scale and make copies of the
object. [Source code](https://github.com/hashcube/js.io/blob/master/packages/math/geom/Circle.js)


## Methods

### new Circle ([x, y, radius])

Parameters
:    1. `x {number} = 0`
	 2. `y {number} = 0`
	 3. `radius {number} = 0`

Creates a circle centered at the point (`x`,`y`) with radius `radius`.

~~~
import math.geom.Circle as Circle;

new Circle(0,0,10) => {x: 0, y: 0, radius: 10}
~~~

### new Circle (circle)

Parameters
:    1. `circle {object}`
	     * `x {number} = 0`
		 * `y {number} = 0`
		 * `radius {number} = 0`

Creates a copy of the input circle.

~~~
var circle1 = new Circle(1,3,5);

var circle2 = new Circle(circle1); //=> {x: 1, y: 3, radius: 5}
~~~

### scale (factor)

Parameters
:    1. `factor {number}`

Returns
:    1. `{this}`

Scales the position *and* radius of `circle` by a factor.

~~~
var circle = new Circle(1,3,5);
circle.scale(3); //=> {x: 3, y: 9, radius: 15}
~~~


# Class: math.geom.Rect

Class used to represent rectangles in a 2D plane. Functions
are provided to perform unions and other
tasks. [Source code](https://github.com/hashcube/js.io/blob/master/packages/math/geom/Rect.js)

## Methods

### new Rect ()

Creates a new rectangle with its top left point at (0,0) with 0 width and height.

~~~
import math.geom.Rect as Rect;

var rect = new Rect(); //=> {x: 0, y: 0, width: 0, height: 0}
~~~

### new Rect (x, y, width, height)

Parameters
:    1. `x {number}`
	 2. `y {number}`
	 3. `width {number}`
	 4. `height {number}`

Creates a new rectangle with its top left point at (`x`,`y`)
with width set to `width` and height set to `height`.

~~~
var rect1 = new Rect(1,3,5,10); //=> {x: 1, y: 3, width: 5, height: 10}
var rect2 = new Rect(3,0,5,2);  //=> {x: 3, y: 0, width: 5, height: 2}
~~~

### new Rect (topleft, width, height)

Parameters
:    1. `topleft {Point}`
	 2. `width {number}`
	 3. `height {number}`

Creates a new rectangle with its top left point at `topleft`
with width set to `width` and height set to `height`.

~~~
var rect = new Rect(new Point(6, 8), 5, 10); //=> {x: 6, y: 8, width: 5, height: 10}
~~~

### new Rect (topleft, size)

Parameters
:    1. `topleft {Point}`
	 2. `size {Point}`

Creates a new rectangle with its top left point at `topleft`
with width set to `size.x` and height set to `size.y`.

~~~
var rect = new Rect(new Point(2, 3), new Point(4, 7)); //=> {x: 2, y: 3, width: 4, height: 7}
~~~

### new Rect (rect)

Parameters
:    1. `rect {Rect}`

Creates a copy of the input rectangle `rect`.

~~~
var rect1 = new Rect(2, 3, 4, 7);
var rect2 = new Rect(rect); //=> {x: 2, y: 3, width: 4, height: 7}
~~~

### normalize ()

Returns
:    1. `{this}`

Normalize negative dimensions so that the rectangle is based
on the upper left corner with positive dimensions.

~~~
var rect = new Rect(5, 3, -4, 7);
rect.normalize(); //=> {x: 1, y: 3, width: 4, height: 7}
~~~

### unionRect (rect2)

Parameters
:    1. `rect2 {Rect}`

Sets `rect` to be the union with itself another rectangle `rect2`.

~~~
var rect1 = new Rect(0, 0, 10, 5);
var rect2 = new Rect(5, 2, 6, 2);
rect1.unionRect(rect2); //=> {x: 5, y: 2, width: 5, height: 2}
~~~

### getCenter ()

Returns
:    1. `{Point}`

Returns the center point of the rectangle `rect`.

~~~
var rect = new Rect(0, 0, 10, 5);
rect.getCenter(); //=> {x: 5, y: 2.5}
~~~

### getCorner (corner)

Parameters
:    1. `corner {Rect.CORNER}`

Returns
:    1. `{Point}`

Returns a point corresponding to the specified corner of the
rectangle. Valid options for the corner object is shown in
the section below.

~~~
var rect = new Rect(0, 2, 10, 5);

//Enumerable values used to represent the four corners of the rectangle object.
rect.getCorner(Rect.CORNER.TOP_LEFT); //=> {x: 0, y: 2}
rect.getCorner(Rect.CORNER.TOP_RIGHT); //=> {x: 10, y: 2}
rect.getCorner(Rect.CORNER.BOTTOM_LEFT); //=> {x: 0, y: 7}
rect.getCorner(Rect.CORNER.BOTTOM_RIGHT); //=> {x: 10, y: 7}
~~~

### getSide (side)

Parameters
:    1. `side {Rect.SIDE}`

Returns
:    1. `{Line}`

Returns a line corresponding to the specified side of the
rectangle. The possible values for side can be found in the
section below.

~~~
var rect1 = new Rect(0, 2, 10, 5);

rect.getSide(Rect.SIDE.TOP);    //=> {start: {x: 0, y: 2}, end: {x: 10, y:2}}
rect.getSide(Rect.SIDE.RIGHT);  //=> {start: {x: 10, y: 2}, end: {x: 10, y:7}}
rect.getSide(Rect.SIDE.BOTTOM); //=> {start: {x: 10, y: 7}, end: {x: 0, y:7}}
rect.getSide(Rect.SIDE.LEFT);   //=> {start: {x: 0, y: 7}, end: {x: 0, y:2}}
~~~

## Class Properties

### Rect.CORNER `{object}`

* `TOP_LEFT`
* `TOP_RIGHT`
* `BOTTOM_LEFT`
* `BOTTOM_RIGHT`

### Rect.SIDE `{object}`

* `TOP`
* `RIGHT`
* `BOTTOM`
* `LEFT`

Enumerable values used to represent the four sides of the
rectangle object.


# Class: math.geom.Vec2D

Model a vector in a 2D plane. A vector consists of an x and
y value and is thought of as an arrow from the origin (0,0)
to the point (x,y). This means that vectors have both a
direction (The angle of this arrow) and a magnitude (The
length of the arrow). Vectors are useful for representing
position, velocity, and other properties of in game
objects. [Source code](https://github.com/hashcube/js.io/blob/master/packages/math/geom/Vec2D.js)


## Methods

### new Vec2D (options)

Parameters
:    1. `options {object}`
	     * `x {number}`
		 * `y {number}`
	
Creates a new vector with its x value set to `options.x` and y value set to `options.y`.

~~~
import math.geom.Vec2D as Vec2D;

var vec = new Vec2D({x: 1, y: 2}}); //=> {x: 1, y: 2}
~~~

### new Vec2D (options)

Parameters
:    1. `options {object}`
	     * `angle {number}`
		 * `magnitude {number}`

Creates a new vector in the direction given by
`options.angle` and with magnitude given by
`options.magnitude`.

~~~
import math.geom.Vec2D as Vec2D;

var vec = new Vec2D({angle: 3.14159, magnitude: 2}}); //=> {x: -2, y: 0}
~~~

### addForce (fvec)

Parameters
:    1. `fvec {Vec2D}` ---Force vector.

Returns
:    1. `{this}`  

Adds x and y components of `fvec` to `vec`. `vec` is
modified directly and no new Vec2D object is allocated.

~~~
var v = new Vec2D({x: 2, y: 3});
var f = new Vec2D({x: 1, y: 5});
v.addForce(f) => {x:3, y: 8}
~~~

### getAngle ()

Returns
:    1. `{number}`

Returns the angle of `vec` in radians. The angle is measured
counter-clockwise from the positive x axis. If `vec.y` is
greater than or equal to 0, the angle measurement is
positive and ranges from 0 to 3.14159. If the `vec.y` is
less than 0, the angle measurement is negative and ranges
from 0 to -3.14159.

~~~
(new Vec2D({x: 1, y: 0})).getAngle(); //=> 0
(new Vec2D({x: 0, y: -1})).getAngle(); //=> -1.57079
~~~

### getMagnitude ()

Returns
:    1. `{number}`

Returns the magnitude of `vec`. Magnitude can be thought of
as the distance from the tip of the vector to the origin.

~~~
(new Vec2D({x: 1.78885, y: 3.57770})).getMagnitude(); //=> 4
~~~

### getUnitVector ()

Returns
:    1. `{Vec2D}`

Creates a new vector with the same direction as `vec`, but
with a magnitude of 1.

~~~
(new Vec2D({x: 2, y:2})).normalize(); //=> {x: 0.7071, y: 0.7071}
~~~

### dot (vec2)

Parameters
:    1. `vec2 {Vec2D}`

Returns
:    1. `{number}`

Returns the dot product of `vec` and `vec2`. The dot product
is computed as `vec.x * vec2.x + vec.y * vec2.y`.

~~~
var v = new Vec2D({x: 2, y: 3});
var b = new Vec2D({x: 1, y: 5});
v.dot(b); //=> {x:2, y: 15}
~~~

### add (vec2)

Parameters
:    1. `vec2 {Vec2D}`

Returns
:    1. `{Vec2D}`

Returns a new vector representing the sum of `vec` and `vec2`.

~~~
var v = new Vec2D({x: 2, y: 3});
var b = new Vec2D({x: 1, y: 5});
v.add(b); //=> {x:3, y: 8}
~~~

### minus (vec2)

Parameters
:    1. `vec2 {Vec2D}`

Returns
:    1. `{Vec2D}`

Returns a new vector representing `vec2` minus `vec`.

~~~
var v = new Vec2D({x: 2, y: 3});
var b = new Vec2D({x: 1, y: 5});
v.minus(b); //=> {x:1, y: -2}
~~~

### negate ()

Parameters
:    1. `vec {Vec2D}`

Returns
:    1. `{Vec2D}`

Returns a new vector with the same magnitude as `vec`, but with the opposite direction.

~~~
(new Vec2D({x: 2, y: 3})).negate();  //=> {x: -2, y: -3}
(new Vec2D({x: 4, y: -7})).negate(); //=> {x: -4, y: 7}
~~~

### multiply (scalar)

Parameters
:    1. `scalar {number}`

Returns
:    1. `{Vec2D}`

Returns a new vector with the same direction as `vec` and
the magnitude of `vec` multiplied by `scalar`.

~~~
(new Vec2D({x: 2, y: 3})).multiply(2);  //=> {x: 4, y: 6}
(new Vec2D({x: 4, y: -7})).multiply(3); //=> {x: 12, y: -21}
~~~


# Module: math.geom.angle

Class containing functions for working with angles. The
functions simplify keeping angles within the range -PI to PI
and also assist with calculating angle
ranges/differences. [Source code](https://github.com/hashcube/js.io/blob/master/packages/math/geom/angle.js)

~~~
import math.geom.angle as angle;
~~~

## Methods

### average (angle1, angle2 [, weight])

Parameters
:    1. `angle1 {number}`
	 2. `angle2 {number}`
	 3. `weight {number} = 0.5` ---Float between 0.0 and 1.0

Returns
:    1. `{number}`

Returns the weighted average of `angle1` and `angle2`. When
`weight` is 0 then the result is `angle1`. When `weight` is
1.0 then the result is `angle2`.

~~~
angle.average(2.0,-2.5);   //=> 2.89156
angle.average(2.0,-2.5,0); //=> 2.0
angle.average(2.0,-2.5,1); //=> -2.5
~~~

### normalize (angle)

Parameters
:    1. `angle {number}`

Returns
:    1. `{number}`

Normalize an angle to be within the range -PI to PI. *Does not work with negative angles.*

~~~
angle.normalize(6);   //=> -0.28318
angle.normalize(-4);  //=> -4 <-- This is a bug with angle.normalize
angle.normalize(1.2); //=> 1.2
~~~

### add (angle1, angle2)

Parameters
:    1. `angle1 {number}`
     2. `angle2 {number}`

Returns
:    1. `{number}`

Compute the sum of `angle1` and `angle2`, within the range
of -PI to PI. Note that `angle1` and `angle2` should be
within the range of -PI to PI.

~~~
angle.add(1.5,2.2); //=> -2.58318
angle.add(-3,3.1);  //=> 0.1
angle.add(1.5,-2);  //=> -0.5
~~~

### difference (angle1, angle2)

Parameters
:    1. `angle1 {number}`
	 2. `angle2 {number}`

Returns
:    1. `{number}`

Compute the difference between `angle1` and `angle2`, within the range
of -PI to PI. The angle returned is the angle with the
smallest magnitude. Note that `angle1` and `angle2` should
be within the range -PI to PI.

~~~
angle.difference(1.5,2.2); //=> 0.7
angle.difference(-3,3.1);  //=> -0.183185
angle.difference(1.5,-2);  //=> 2.783185
~~~

### range (angle1, angle2)

Parameters
:    1. `angle1 {number}`
	 2. `angle2 {number}`

Returns
:    1. `{number}`

Angular distance measured clockwise from `angle1` to
`angle2`, in the range of 0 and 2PI. Note that `angle1` and
`angle2` should be within the range -PI to PI.

~~~
angle.range(1.5,2.2); //=> 0.7
angle.range(-3,3.1);  //=> 6.1
angle.range(1.5,-2);  //=> 2.783185
~~~

# Module: math.geom.intersect

The intersect module provides functions for checking
intersections between the shape classes defined in the
previous sections.

~~~
import math.geom.intersect as intersect;
~~~

## Methods

### pointAndRect (point, rect)

Parameters
:    1. `point {Point}`
     2. `rect {Rect}`

Returns
:    1. `{boolean}`

Returns true if `point` lies within or on the boundary of
the rectangle `rect` and false otherwise.

### pointAndCircle (point, circle)

Parameters
:    1. `point {Point}`
	 2. `circle {Circle}`

Returns
:    1. `{boolean}`

Returns true if `point` lies within or on the boundary of
`circle` and false otherwise.

### rectAndRect (rect1, rect2)

Parameters
:    1. `rect1 {Rect}`
	 2. `rect2 {Rect}`

Returns
:    1. `{boolean}`

Returns true if `rect1` and `rect2` overlap and false otherwise.

### circleAndRect (circle, rect)

Parameters
:    1. `circle {Circle}`
	 2. `rect {Rect}`

Returns
:    1. `{boolean}`

Returns true if `circle` and `rect` overlap and false otherwise.

### circleAndLine (circle, line)

Parameters
:    1. `circle {Circle}`
	 2. `line {Line}`

Returns
:    1. `{boolean}`

Returns true if `circle` and `line` overlap and false otherwise.


# Module: math.geom.intersect.util

~~~
import math.geom.intersect.util as util;
~~~

## Methods

### util.pointToLine (point, line)

Parameters
:    1. `point {Point}`
	 2. `line {Line}`

Returns
:    1. `{Line}`

Returns a `Line` from `point` to the nearest point on `line`.

### util.rectAndRect (rect1, rect2)

Paramaters
:    1. `rect1 {Rect}`
	 2. `rect2 {Rect}`

Returns
:    1. `{Rect}`

Returns the overlap between `rect1` and `rect2` as a new rectangle `Rect`.
