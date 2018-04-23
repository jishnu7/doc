# Utilities

A variety of helper functions for working and managing JavaScript code.

## Modules

The hashcube DevKit provides a framework to make
development easier in JavaScript. This includes a class
system to help structure your code and an import system so
you can work in different source files.

### import

The `import` statement is used to refer to external modules,
as defined in a separate file, and include them in this
file. Modules may be arranged in a namespace hierarchy to
indicate a usage category. Additionally, modules may be
aliased using the `as` keyword for easier reference.

*Note: the `import` keyword is specific to the hashcube DevKit and not standard JavaScript.*

~~~
import device;
import ui.View;
import src.path.to.Module as Module;

var view = new ui.View(),
    mod = new Module();

console.log(device.defaultFontFamily); //=> 'Helvetica'
console.log(typeof ui);                //=> 'object'
~~~

The path of a user defined module is relative to the
project's root directory. So, in the above example, the
import path of `Module` refers to the file located at
`{project}/src/path/to/Module.js`.

Files may be imported realtive to the current source
directory by prepending the path with a dot (`.`). To
reference a file up a directory, use two dots.

~~~
import .MyModule; //=> ./MyModule.js (In this directory)
import ..foo;     //=> ../foo.js     (In parent directory)
import ...foo;    //=> ../../foo.js  (In grandparent directory)
~~~

### Class ([name, superConstructor,] constructor)

Parameters
:    1. `name {string}` ---An optional name to assign to the class for debugging purposes.
	 2. `superConstructor {function}` ---Optional superclass which this class inherits from.
	 3. `constructor {function(supr)}` ---The function that will define this class.

Returns
:    1. `{function}` ---Class constructor.

Our class system for simplifying JavaScript object
inheritance. Objects of a class-type inherit the properties and methods
defined on that class. A sub-class inherits the attributes of its super-class.

Properties and methods defined on the `this` object within
the constructor function are considered public.

If an `init` method exists on the constructor definition, it
is automatically called when an object is instantiated.

The constructor function is passed a `supr` argument---an
accessor function which is used to call a given method name
on the superclass. Typically, this is used to initialize all
the objects in a class hierarchy.

~~~
var MySuperClass = Class(function () {
  this.init = function (name) {
    this.name = name;
  };
});

var MyClass = Class(MySuperClass, function (supr) {
  this.init = function (name) {
    //Call the init of the super class with the arguments passed to this class.
    supr(this, 'init', arguments);  
  };

  this.say = function () {
    console.log("Hello, " + this.name + "!");
  };
});

var a = new MyClass('Arjun'),
    b = new MyClass('Billy');

a.say(); //=> "Hello, Arjun!"
b.say(); //=> "Hello, Billy!"
~~~


## GLOBAL `{object}`

The global object. Properties assigned to this object are
available in the global scope, within any class or
module. In a browser, this is the same as assigning a
property to the `window` object.

For example, to create a property that is accessible
throughout your modules and classes:

~~~
GLOBAL.myprop = "This is available throughout your code."
~~~

### bind (thisArg, callback [, args ...])

Parameters
:    1. `thisArg {object}`
     2. `callback {function}`
     3. `args {...*}`

Returns
:    1. `{function}`

Binds the first parameter as the `this` object in the callback
function. Additional arguments are curried to the given function.

~~~
function add_to_point (x, y) {
  return {
    x: this.x + x,
    y: this.y + y
  };
}
var pt = {x: 100, y: 400};
var add_to_y = bind(pt, add_to_point, 23);

add_to_y(56); //=> {x: 123, y: 456}
~~~

This convenience function is provided for readability over the
traditional JavaScript [`.bind`](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/Function/bind) method:

~~~
view.on('InputSelect', bind(this, function (evt) {
  //...
}));

view.on('InputSelect', (function (evt) {
  //..
}).bind(this));
~~~

In the first example, it's clear that a new context is
assigned to `this` in the callback function at the
beginning, where in the second example, the new binding of
`this` doesn't occur until the end of the function declaration.


### merge (obj1, obj2 [, obj3 ...])

Parameters
:    1. `base {object}`
     2. `props {...object}`

Returns
:    1. `{object}`

Combine the properties of multiple objects in to a single
object. All properties are collected on the object passed as
the first argument, and if a property already exists,
subsequent values are ignored.

~~~
var obj1 = {a: 1, b: 2},
    obj2 = {b: 3, c: 4};

merge(obj1, obj2); //=> obj1

console.log(obj1); //=> {a: 1, b: 2, c: 4}
~~~

This function is particularly useful for combining objects
that contain options that are passed up a class
initialization hierarchy. However, there is a caveat for
this common usage, since an options object may not exist,
the properties can not be merged to an `undefined` value. To
account for this, make sure to use the returned value of
`merge`, as illustrated here:

~~~
function not_merged (options) {
  merge(options, {x: 1});
  return options;
}

function merged (options) {
  options = merge(options, {x: 1});
  return options;
}

not_merged();  //=> undefined
merged();      //=> {x: 1}
~~~

This way you can be assured that the options object contains
all the properties you intend for it to have.

### CACHE

This is a cache of the JSON files in your project. Use it like this:

~~~
var price = JSON.parse(CACHE['resources/conf/storeItems.json']).item3.price;
~~~

# Module: util.ajax

The hashcube DevKit provides functions which make it easy to
download data from remote locations. These functions can be found
in the `ajax` package and wrap the `XMLHttpRequest` object.

## Examples

* [Ajax populated list](../example/ui-list-ajax/)

## Queuing

When there are too many requests made within a short period
of time, the requests are placed in a queue. The maximum
number of requests which can be handled simultaneously is 4.

## Error Handling

The Ajax routines handle all errors and the first parameter of
the callback is the error status. On success, the value is
`null`, and on failure this parameter will give information
about the error.


## Methods

### get (opts [, callback])

Parameters
:    1. `opts {object}` ---Options
	     * `url {string}` ---The URL.
		 * `type {string}` ---Optional, can be unset or `json`.
		 * `data {object}` ---Optional data to send.
		 * `headers {object}` ---Optional list of http headers.
	 2. `callback {function}` ---Optional callback which is executed when the data is downloaded.

The following code fragement shows how to get data from the
Facebook search API. The url parameter has the location of
search API. The header is set to `text-plain` which is
actually the default value.

The data option has two fields which are specific to this
API, they are `q` which is the text to search for and `type`
which is where to search (in posts). If the get function is
invoked as demonstrated here then the values of `data` will
be url encoded and appended to the url.

When the result is downloaded it will be parsed as JSON
because the type was set to `json`.

~~~
import util.ajax as ajax;

ajax.get({
  url: 'http://graph.facebook.com/search',
  headers: {'Content-Type': 'text/plain'},
  data: {q: 'game', type: 'post'},
  type: 'json'
}, function (err, response) {
  if (err) {
    console.error('someting went wrong');
  } else {
    console.log(response);
  }
});
~~~

### post (opts [, callback])

Parameters
:    1. `opts {object}` ---Options
	     * `url {string}` ---The URL.
		 * `type {string}` ---Optional, can be unset or `json`.
		 * `headers {object}` ---Optional list of http headers.
	 2. `callback {function}` ---Optional callback which is executed when the data is downloaded.

The following demo posts data to the Facebook search
API. Because the type value is not in the options the
response will be a string.

The data will be posted and won't be appended to the url as
a query string.

~~~
ajax.post({
  url: 'http://graph.facebook.com/search',
  data: {q: 'closure', type: 'post'}
}, function (err, response) {
  if (err) {
    console.error('someting went wrong');
  } else {
    console.log(response);
  }
});
~~~

Note that iOS devices commonly split POST requests into separate packets for the header and body. This is proper HTTP. Just make sure your server reads the request in using Content-Length instead of stopping at packet boundaries, which are arbitrary. For more information, see [this discussion of the BSD socket layer](http://lists.apple.com/archives/macnetworkprog/2004/May/msg00042.html).