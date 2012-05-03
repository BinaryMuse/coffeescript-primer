CoffeeScript Primer
===================

[CoffeeScript](http://coffeescript.org/) is a great language with tons of features. It's been my experience that even programmers who write CoffeeScript often are not aware of some of the languages more powerful features. Thus, while this guide will serve partly as an introduction to some CoffeeScript syntax, it is generally assumed that the reader has looked over the CoffeeScript home page and knows the general syntax of the language, and the document will go over some of the lesser known and more useful features of the CoffeeScript language.

Each example will show the compiled JavaScript for the given CoffeeScript. Be sure to check out the [CoffeeScript site](http://coffeescript.org/) for many more examples.

Functions, Default Arguments, Splats, Binding
---------------------------------------------

Functions can be declared inline or as a block:

```coffee-script
square = (x) -> x * x

cube = (x) ->
  square(x) * x
```

```javascript
var cube, square;

square = function(x) {
  return x * x;
};

cube = function(x) {
  return square(x) * x;
};
```

Notice that, as in Ruby, the last evaluated expression in a function indicates the function's return value.

Functions may also take default arguments or splats:

```coffee-script
makeInteger = (x, radix = 10) -> parseInt(x, radix)

sum = (numbers..., cb) ->
  total = 0
  total += n for n in numbers
  cb(total)

sum(1, 2, 3)
```

```javascript
var makeInteger, sum,
  __slice = [].slice;

makeInteger = function(x, radix) {
  if (radix == null) {
    radix = 10;
  }
  return parseInt(x, radix);
};

sum = function() {
  var cb, n, numbers, total, _i, _j, _len;
  numbers = 2 <= arguments.length ? __slice.call(arguments, 0, _i = arguments.length - 1) : (_i = 0, []), cb = arguments[_i++];
  total = 0;
  for (_j = 0, _len = numbers.length; _j < _len; _j++) {
    n = numbers[_j];
    total += n;
  }
  return cb(total);
};

sum(1, 2, 3);
```

You can use the `=>` symbol when defining a function or method to ensure that the function is bound to `this` when called.

```coffee-script
incr = (x) =>
  this.total += x

setInterval incr, 100
```

```javascript
var incr,
  _this = this;

incr = function(x) {
  return _this.total += x;
};

setInterval(incr, 100);
```

Conditionals, Ternary Operator, Operators, Aliases
--------------------------------------------------

Conditionals can be written inline or as a block, and support a postfix form as in Ruby. `unless` can be used as the opposite as `if`. CoffeeScript also allows chained comparisons like Python.

```coffee-script
redirect_to('/banned') if current_user.banned

if current_user.admin
  "#{current_user.name} (admin)"
else
  "#{current_user.name}"

channel = if age < 16 then "nick" else "comedy central"

expensive_rental_car = 18 < age < 25
```

```javascript
var channel, expensive_rental_car;

if (current_user.banned) {
  redirect_to('/banned');
}

if (current_user.admin) {
  "" + current_user.name + " (admin)";
} else {
  "" + current_user.name;
}

channel = age < 16 ? "nick" : "comedy central";

expensive_rental_car = (18 < age && age < 25);
```

CoffeeScript offers many aliases; one of the most common is `@` which is replaced by `this`. For a full list, check out [Operators and Aliases](http://coffeescript.org/#operators) on the CoffeeScript site.

The Existential Operator
------------------------

CoffeeScript offers the existential operator to check for a value of `null` or `undefined`, useful if comparing strings that may be empty, booleans that may be false, or the value `0`.

You can also use it as an accessor to soak up null references.

```coffee-script
logged_in = true if current_user?

name = ""
name ?= "Brandon"

user_name = current_user?.name ? "not logged in"

zip = lottery.drawWinner?().address?.zipcode
```

```javascript
var logged_in, name, user_name, zip, _ref, _ref1;

if (typeof current_user !== "undefined" && current_user !== null) {
  logged_in = true;
}

name = "";

if (name == null) {
  name = "Brandon";
}

user_name = (_ref = typeof current_user !== "undefined" && current_user !== null ? current_user.name : void 0) != null ? _ref : "not logged in";

zip = typeof lottery.drawWinner === "function" ? (_ref1 = lottery.drawWinner().address) != null ? _ref1.zipcode : void 0 : void 0;
```

Destructuring Assignment
------------------------

When you assign an array or object literal to a value, CoffeeScript breaks up and matches both sides against each other, assigning the values on the right to the variables on the left.

```coffee-script
a = 10
b = 20

[a, b] = [b, a]

[first, others..., suffix] = "Brandon Kyle Tilley Jr.".split(" ")
```

```javascript
var a, b, first, others, suffix, _i, _ref, _ref1,
  __slice = [].slice;

a = 10;

b = 20;

_ref = [b, a], a = _ref[0], b = _ref[1];

_ref1 = "Brandon Kyle Tilley Jr.".split(" "), first = _ref1[0], others = 3 <= _ref1.length ? __slice.call(_ref1, 1, _i = _ref1.length - 1) : (_i = 1, []), suffix = _ref1[_i++];
```

It is also useful for objects.

```coffee-script
programmers =
  ruby: 'brandon'
  php:
    name: 'neal'
    company: 'fpu'

{php: {name, company}} = programmers

alert name


{EventEmitter} = require 'events'
```

```javascript
var EventEmitter, company, name, programmers, _ref;

programmers = {
  ruby: 'brandon',
  php: {
    name: 'neal',
    company: 'fpu'
  }
};

_ref = programmers.php, name = _ref.name, company = _ref.company;

alert(name);

EventEmitter = require('events').EventEmitter;
```

As an aside, note you can leave out commas when defining an object literal.

Loops and Comprehensions
------------------------

Most JavaScript loops will be expressed as comprehensions over arrays (or ranges) and objects.

```coffee-script
# array comprehensions
people = [
  { name: 'brandon', gender: 'male' }
  { name: 'rachel', gender: 'female' }
  { name: 'neal', gender: 'male' }
  { name: 'andrea', gender: 'female' }
]

# select/filter
females = (person for person in people when person.gender is 'female')

# map
names = (person.name for person in people)

# each
sendEmail(person.name, person.gender) for person in people

# object comprehensions
ages = brandon: 26, jane: 23, bob: 29, allen: 27

strs = for name, age of ages
  "#{name} is #{age}"
```

```javascript
var age, ages, females, name, names, people, person, strs, _i, _len;

people = [
  {
    name: 'brandon',
    gender: 'male'
  }, {
    name: 'rachel',
    gender: 'female'
  }, {
    name: 'neal',
    gender: 'male'
  }, {
    name: 'andrea',
    gender: 'female'
  }
];

females = (function() {
  var _i, _len, _results;
  _results = [];
  for (_i = 0, _len = people.length; _i < _len; _i++) {
    person = people[_i];
    if (person.gender === 'female') {
      _results.push(person);
    }
  }
  return _results;
})();

names = (function() {
  var _i, _len, _results;
  _results = [];
  for (_i = 0, _len = people.length; _i < _len; _i++) {
    person = people[_i];
    _results.push(person.name);
  }
  return _results;
})();

for (_i = 0, _len = people.length; _i < _len; _i++) {
  person = people[_i];
  sendEmail(person.name, person.gender);
}

ages = {
  brandon: 26,
  jane: 23,
  bob: 29,
  allen: 27
};

strs = (function() {
  var _results;
  _results = [];
  for (name in ages) {
    age = ages[name];
    _results.push("" + name + " is " + age);
  }
  return _results;
})();
```

As an aside, note that you can leave off commas when defining an array.

To iterate over an object using `hasOwnProperty`, use `for own key, value of obj`.

Function Closures
-----------------

The `do` keyword allows you to create a closure, ensuring that the generated functions don't just share the final value of the iteration.

```coffeescript
for filename in list
  do (filename) ->
    fs.readFile filename, (err, contents) ->
      compile filename, contents.toString()
```

```javascript
var filename, _fn, _i, _len;

_fn = function(filename) {
  return fs.readFile(filename, function(err, contents) {
    return compile(filename, contents.toString());
  });
};
for (_i = 0, _len = list.length; _i < _len; _i++) {
  filename = list[_i];
  _fn(filename);
}
```

Classes
-------

CoffeeScript allows you to create a prototypal inheritance chain based off a classical model. There is a lot of information about classes on the CoffeeScript site and I encourage you to [check out that section in detail](http://coffeescript.org/#classes).

```coffeescript
class Animal
  constructor: (@name) ->

  move: (meters) ->
    alert @name + " moved #{meters}m."

class Snake extends Animal
  move: ->
    alert "Slithering..."
    super 5

class Horse extends Animal
  move: ->
    alert "Galloping..."
    super 45

sam = new Snake "Sammy the Python"
tom = new Horse "Tommy the Palomino"

sam.move()
tom.move()
```

```javascript
var Animal, Horse, Snake, sam, tom,
  __hasProp = {}.hasOwnProperty,
  __extends = function(child, parent) { for (var key in parent) { if (__hasProp.call(parent, key)) child[key] = parent[key]; } function ctor() { this.constructor = child; } ctor.prototype = parent.prototype; child.prototype = new ctor; child.__super__ = parent.prototype; return child; };

Animal = (function() {

  Animal.name = 'Animal';

  function Animal(name) {
    this.name = name;
  }

  Animal.prototype.move = function(meters) {
    return alert(this.name + (" moved " + meters + "m."));
  };

  return Animal;

})();

Snake = (function(_super) {

  __extends(Snake, _super);

  Snake.name = 'Snake';

  function Snake() {
    return Snake.__super__.constructor.apply(this, arguments);
  }

  Snake.prototype.move = function() {
    alert("Slithering...");
    return Snake.__super__.move.call(this, 5);
  };

  return Snake;

})(Animal);

Horse = (function(_super) {

  __extends(Horse, _super);

  Horse.name = 'Horse';

  function Horse() {
    return Horse.__super__.constructor.apply(this, arguments);
  }

  Horse.prototype.move = function() {
    alert("Galloping...");
    return Horse.__super__.move.call(this, 45);
  };

  return Horse;

})(Animal);

sam = new Snake("Sammy the Python");

tom = new Horse("Tommy the Palomino");

sam.move();

tom.move();
```

Note that a constructor that has a signature like `constructor(@thing) ->` will assign the argument passed in to `this.thing` automatically.

Also note that you can access the constructor function's name via `this.constructor.name`.