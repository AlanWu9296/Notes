# Object Oriented Programming

## Constructor Function and Own Property

```js
function Bird(name,color) {
  this.name = name;
  this.color = color;
  this.numLegs = 2;
}
```

## Prototype, prototype property, prototype chain

**constructor and prototype (private & public)**

```js
// Constructor method to define own property(that different instance may have different values)
function Bird(name,color) {
  this.name = name;
  this.color = color;
}
// prototype object to define prototype property that(different instances have the same value)
Bird.prototype = {
  constructor: Bird, // define the constructor property
  numLegs: 2,
  eat: function() {
    console.log("nom nom nom");
  },
  describe: function() {
    console.log("My name is " + this.name);
  }
}

let bird = new Bird()

```

**prototype chain (inheritance)**

```js
function Animal() { };

Animal.prototype = {
  constructor: Animal,
  describe: function() {
    console.log("My name is " + this.name);
  }
};

function Bird() { }
//inherite the supertype Animal
Bird.prototype = Object.create(Animal.prototype);
//reset the constructor to itsown(or will be the supertype constructor it inherited)
Bird.prototype.constructor = Bird;
//create its own method
Bird.prototype.fly = function() {
  console.log("I'm flying!");
};
//reset supertype method(not really reset supertype's method but makes a method with the same name but is ealier searched by the run time than the supertype's one)
Bird.prototype.eat = function() {
  return "peck peck peck";
};
```

**Private property with Closure**

```js
function Bird() {
  // comparing to this.hatchedEgg = 10 which is public property
  let hatchedEgg = 10; // private property

  // since the property becomes private, it needs a get/set method  
  this.getHatchedEggCount = function() { // publicly available method that a bird object can use
    return hatchedEgg;
  };
}
```

## Mixin Module with IIFE

IIFE stands for *immediately invoked function expression*

mixin is basically a function that takes an object as the input and make a method for the object which could make shared methods that may not be share by different objects without inheritance relations like duck and boat

using the IIFE , it prevents to pollute the global name space

```js
let motionModule = (function () {
  return {
    glideMixin: function (obj) {
      obj.glide = function() {
        console.log("Gliding on the water");
      };
    },
    flyMixin: function(obj) {
      obj.fly = function() {
        console.log("Flying, wooosh!");
      };
    }
  }
}) (); // The two parentheses cause the function to be immediately invoked

motionModule.glideMixin(duck);
motionModule.glideMixin(boat);
duck.glide();
```

# Functional Programming

## Basic

**Features**

1. Cacheable
2. Portable / Self-Documenting
3. Testable

Functional programming is all about creating and using non-mutating functions.

Functional programming follows a few core principles:

1. Functions  are independent from the state of the program or global variables. They  only depend on the arguments passed into them to make a calculation
2. Functions try to limit any changes to the state of the program and avoid changes to the global objects holding data 
3. Functions have minimal side effects in the program

Another principle of functional programming is to always declare your  dependencies explicitly. This means if a function depends on a variable  or object being present, then pass that variable or object directly into the function as an argument.

Array.prototype:

1. .map(function(currentValue,index,arr), thisValue){}
2. .filter(function(currentValue,index,arr), thisValue){}
3. .reduce(*function(initialTotal,currentValue, index,arr)*)
4. .slice(begin,end)
5. .concat(arr)
6. sort((a,b)=>{}) *a mutation function should be coupled with concat to deep copy the array*
7. .every(item=>{}) *check whether every item satisfy one condition returning true/false*
8. some(item=>{}) *check whether there is any item satisfy one condition returning true/false*

## Curring and Partial Application

The `arity` of a function is the number of arguments it requires. `Currying` a function means to convert a function of N `arity` into N functions of `arity` 1.

Similarly, `partial application` can be described as applying a few arguments to a function at a time and returning another function  that is applied to more arguments.

```js
//Curried function
function curried(x) {
  return function(y) {
    return x + y;
  }
}
curried(1)(2) // Returns 3

//Impartial function
function impartial(x, y, z) {
  return x + y + z;
}
var partialFn = impartial.bind(this, 1, 2);
partialFn(10); // Returns 13
```

```js
var curry = require('lodash/curry');
// a good practice is to always set the resources (like Strings, Arrays, Objects) at last
var match = curry(function(what, str) {
  return str.match(what);
});

// equals to 
var match = function(what){
    return function(str){
        return str.match(what)
    }
}

// empower pre-load
var replace = curry(function(what, replacement, str) {
  return str.replace(what, replacement);
});

var filter = curry(function(f, ary) {
  return ary.filter(f);
});

var map = curry(function(f, ary) {
  return ary.map(f);
});

// the power of curring
match(/\s+/g, 'hello world');
// [ ' ' ]

match(/\s+/g)('hello world');
// [ ' ' ]

var hasSpaces = match(/\s+/g);
// function(x) { return x.match(/\s+/g) }

hasSpaces('hello world');
// [ ' ' ]

hasSpaces('spaceless');
// null

filter(hasSpaces, ['tori_spelling', 'tori amos']);
// ['tori amos']

var findSpaces = filter(hasSpaces);
// function(xs) { return xs.filter(function(x) { return x.match(/\s+/g) }) }

findSpaces(['tori_spelling', 'tori amos']);
// ['tori amos']

var noVowels = replace(/[aeiouy]/ig);
// function(replacement, x) { return x.replace(/[aeiouy]/ig, replacement) }

var censored = noVowels("*");
// function(x) { return x.replace(/[aeiouy]/ig, '*') }

censored('Chocolate Rain');
// 'Ch*c*l*t* R**n'
```

## Composing

**ATTENTION!!! **

in this case, every function should take exact **one parameters**. l

```js
//looks like a python decorator???
var compose = function(f, g) {
  return function(x) {
    return f(g(x));
  };
};
// as the defination, the excute direction is from "right to left"

var toUpperCase = function(x) {
  return x.toUpperCase();
};
var exclaim = function(x) {
  return x + '!';
};
var shout = compose(exclaim, toUpperCase);

shout("send in the clowns");
//=> "SEND IN THE CLOWNS!"
```

**Traits**

```js
//Associativity
var associative = compose(f, compose(g, h)) == compose(compose(f, g), h);
```

**Pointfree** style means never having to say your data

```js
// not pointfree,need the data: word
var snakeCase = function(word) {
  return word.toLowerCase().replace(/\s+/ig, '_');
};

// pointfree
var snakeCase = compose(replace(/\s+/ig, '_'), toLowerCase);
```

### Categoty Theory

In category theory, we have something called... a category. It is defined as a collection with the following components:

- A collection of objects
- A collection of morphisms
- A notion of composition on the morphisms
- A distinguished morphism called identity

**A collection of objects** The objects will be data types. For instance, `String`, `Boolean`, `Number`, `Object`, etc. We often view data types as sets of all the possible values. One could look at `Boolean` as the set of `[true, false]` and `Number` as the set of all possible numeric values. Treating types as sets is useful because we can use set theory to work with them.

**A collection of morphisms** The morphisms will be our standard every day pure functions.

**A notion of composition on the morphisms** This, as you may have guessed, is our brand new toy - `compose`. We've discussed that our `compose` function is associative which is no coincidence as it is a property that must hold for any composition in category theory.

![category composition 1](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/images/cat_comp1.png)

![category composition 2](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/images/cat_comp2.png)

**A distinguished morphism called identity** Let's introduce a useful function called `id`. This function simply takes some input and spits it back at you. 

```js
// identity
compose(id, f) == compose(f, id) == f;
// true
```

## Hindley-Milner Type Signature

```js
//  strLength :: String -> Number
var strLength = function(s){
  return s.length;
}

//  join :: String -> [String] -> String
var join = curry(function(what, xs){
  return xs.join(what);
});

//  match :: Regex -> String -> [String]
var match = curry(function(reg, s){
  return s.match(reg);
});

//  replace :: Regex -> String -> String -> String
var replace = curry(function(reg, sub, s){
  return s.replace(reg, sub);
});

//use type constraints as an interface
// sort :: Ord a => [a] -> [a]
// assertEqual :: (Eq a, Show a) => a -> a -> Assertion
```

## Functor

```js
var Container = function(x) {
  this.__value = x;
}

Container.of = function(x) { return new Container(x); };

// (a -> b) -> Container a -> Container b
Container.prototype.map = function(f){
  return Container.of(f(this.__value))
}
```

A Functor is a type that implements `map` and obeys some laws

```js
// identity
map(id) === id;

// composition
compose(map(f), map(g)) === map(compose(f, g));
```



```js
// a Functor that do a null/undefined check it self
var Maybe = function(x) {
  this.__value = x;
}

Maybe.of = function(x) {
  return new Maybe(x);
}

Maybe.prototype.isNothing = function() {
  return (this.__value === null || this.__value === undefined);
}

Maybe.prototype.map = function(f) {
  return this.isNothing() ? Maybe.of(null) : Maybe.of(f(this.__value));
}

// map :: Functor f => (a -> b) -> f a -> f b
const map = curry((f, anyFunctor) => anyFunctor.map(f));
```

the **Either** functor which can always indicate the error message

```js
var Left = function(x) {
  this.__value = x;
}

Left.of = function(x) {
  return new Left(x);
}

Left.prototype.map = function(f) {
  return this;
}

var Right = function(x) {
  this.__value = x;
}

Right.of = function(x) {
  return new Right(x);
}

Right.prototype.map = function(f) {
  return Right.of(f(this.__value));
}

var moment = require('moment');

//  getAge :: Date -> User -> Either(String, Number)
var getAge = curry(function(now, user) {
  var birthdate = moment(user.birthdate, 'YYYY-MM-DD');
  if(!birthdate.isValid()) return Left.of("Birth date could not be parsed");
  return Right.of(now.diff(birthdate, 'years'));
});

getAge(moment(), {birthdate: '2005-12-12'});
// Right(9)

getAge(moment(), {birthdate: 'balloons!'});
// Left("Birth date could not be parsed")

//  either :: (a -> c) -> (b -> c) -> Either a b -> c
var either = curry(function(f, g, e) {
  switch(e.constructor) {
    case Left: return f(e.__value);
    case Right: return g(e.__value);
  }
});
```

At the time of calling, a function can be surrounded by `map`, which transforms it from a non-functory function to a functory one, in informal terms. We call this process *lifting*. Functions tend to be better off working with normal data types rather than container types, then *lifted* into the right container as deemed necessary. This leads to simpler,  more reusable functions that can be altered to work with any functor on  demand.

```js
var IO = function(f) {
  this.__value = f;
}

IO.of = function(x) {
  return new IO(function() {
    return x;
  });
}

IO.prototype.map = function(f) {
  return new IO(_.compose(f, this.__value));
}
```

![functor diagram](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/images/functormap.png)

![functor diagram 2](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/images/functormapmaybe.png)

```js
//  topRoute :: String -> Maybe(String)
var topRoute = compose(Maybe.of, reverse);

//  bottomRoute :: String -> Maybe(String)
var bottomRoute = compose(map(reverse), Maybe.of);


topRoute("hi");
// Maybe("ih")

bottomRoute("hi");
// Maybe("ih")
```

## Monad

### Pointy Functor

'of ' is to place values in what's called a *default minimal context*. Yes, `of` does not actually take the place of a constructor - it is part of an important interface we call *Pointed*.

`A *pointed functor* is a functor with an `of` method`

### Monad

```js
var mmo = Maybe.of(Maybe.of("nunchucks"));
// Maybe(Maybe("nunchucks"))

mmo.join();
// Maybe("nunchucks")

var ioio = IO.of(IO.of("pizza"));
// IO(IO("pizza"))

ioio.join()
// IO("pizza")

var ttt = Task.of(Task.of(Task.of("sewers")));
// Task(Task(Task("sewers")));

ttt.join()
// Task(Task("sewers"))

Maybe.prototype.join = function() {
  return this.isNothing() ? Maybe.of(null) : this.__value;
}
```

`Monads are pointed functors that can flatten. Any functor which defines a `join` method, has an `of` method, and obeys a few laws is a monad. 

```js
//  chain :: Monad m => (a -> m b) -> m a -> m b
var chain = curry(function(f, m){
  return m.map(f).join(); // 或者 compose(join, map(f))(m)
});

//or called flatmap or bind


//examples
// getJSON :: Url -> Params -> Task JSON
// querySelector :: Selector -> IO DOM


getJSON('/authenticate', {username: 'stale', password: 'crackers'})
  .chain(function(user) {
    return getJSON('/friends', {user_id: user.id});
});
// Task([{name: 'Seimith', id: 14}, {name: 'Ric', id: 39}]);


querySelector("input.username").chain(function(uname) {
  return querySelector("input.email").chain(function(email) {
    return IO.of(
      "Welcome " + uname.value + " " + "prepare for spam at " + email.value
    );
  });
});
// IO("Welcome Olivia prepare for spam at olivia@tremorcontrol.net");


Maybe.of(3).chain(function(three) {
  return Maybe.of(2).map(add(three));
});
// Maybe(5);


Maybe.of(null).chain(safeProp('address')).chain(safeProp('street'));
// Maybe(null);

//remember to map when returning a "normal" value and chain when we're returning another functor.
```

```js
// readFile :: Filename -> Either String (Future Error String)
// httpPost :: String -> Future Error JSON

//  upload :: String -> Either String (Future Error JSON)
var upload = compose(map(chain(httpPost('/uploads'))), readFile);

//compared to callbacks
//  upload :: String -> (String -> a) -> Void
var upload = function(filename, callback) {
  if(!filename) {
    throw "You need a filename!";
  } else {
    readFile(filename, function(err, contents) {
      if(err) throw err;
      httpPost(contents, function(err, json) {
        if(err) throw err;
        callback(json);
      });
    });
  }
}
```

### Theory

```js
 compose(join, map(join)) == compose(join, join)
 compose(join, of) == compose(join, map(of)) == id
```

![monad associativity law](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/images/monad_associativity.png)

![monad identity law](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/images/triangle_identity.png)

## Applicative Functor

```js
Container.prototype.ap = function(other_container) {
  return other_container.map(this.__value);
}

mapping f is equivalent to aping a functor of f
```

`An *applicative functor* is a pointed functor with an `ap` method`