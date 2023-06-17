# Javascript Hard Parts

- One thread of execution = JS can only do one thing at a time
- `function addValue(value) { ... }` => `value` is the `parameter`
- `addValue(3)` => `3` is the `argument`
- `Global execution context` - `Execution context` ( each time we call a function a new execution contexts is created )
- `Global memory` - `Local memory`
- Call stack: Whatever is at the top of the stack is executed. If there is nothing left in the call stack, then runs the global execution context
- DRY principle: DON'T REPEAT YOURSELF
- Higher Order Functions: Functions that take callbacks or return functions
- Function stored as property of an object: `method`

## Closures

Exercises: http://csbin.io/closures

- The local memory (aka `variable environment` / `state`) of a function is fresh everytime

```
    function outer() {
        let counter = 0;
        function increment() { counter ++; console.log('--->', counter) };
        return increment;
    }

    const returnIncrement = outer();

    returnIncrement() // "---> 1"
    returnIncrement() // "---> 2"
```
- When `increment` is returned from `outer`, it also gets a hidden, inaccessible property of scope `[[scope]]` which is a permanent state of live data ( "backpack" / "closure" / "Closed Over Variable Environment COVE" / "Persistent Lexical/Static Scope Referenced Data PLSRD")
- We cannot access the `counter` property stored inside `returnIncrement` - ❌ `returnIncrement.[[scope]].counter` - ❌ `returnIncrement.counter`
- It's only accessible by the function itself

## Asynchronous JS

Exercises: http://csbin.io/async

Javascript is:
- Single threaded ( one command at a time )
- Synchronously executed ( each line is run in order )

Javascript engine has 3 main parts:
- Thread of execution
- Call stack
- Memory / VE ( variable environment )

The browser has got many more features that expands what we can do with javascript:
- Network requests ( `fetch` )
- HTML DOM ( `document` )
- Timer ( `setTimeout` )
- DevTools ( `console` )
- Local Storage ( `localStorage` ) / Session Storage ( `sessionStorage` ) / Cookies ( `document.cookie` )

👆 The above features are run in the web browser, not in the javascript engine!

- In the web browser we have a `Callback Queue` which then adds it's callbacks to the `Call Stack`, ONLY when the global execution context is finished AND the call stack is empty ( all regular synchronous code is run )
- The feature the checks is there is anything in the call stack or the global execution context and if there is anything in the callback queue is called: `Event Loop`

```
    function printHello(){ console.log("Hello"); }
    function blockFor1Sec(){ 
        //blocks in the JavaScript thread for 1 sec 
    }
    setTimeout(printHello,0);
    blockFor1Sec()
    console.log("Me first!");
```

```
    Me first!
    Hello
```

- Promises go to a different queue, the `Microtask Queue`
- First all the global code is run, then the microtask queue takes priority and finally the callback queue
- `Global code` > `Microtask queue` > `Callback queue`

## Class & Prototypes

- Prototype chain: Every object has got a `__proto__` property which defaults to `Object.prototype`
- `Object.prototype` also has a `__proto__` property which is `null`
- When trying to access a property on an object, first we check its own properties and then we start going down the prototype chain
- `this` defaults to the global object ( `window` )
- It's better not to use arrow functions on methods of objects ( if we have to use `this` ), because with arrow functions `this` is NOT assigned the value of the object it's called from, but it instead is assigned the value of this of were it was declared

```
function userCreator(name, score) {
   const newUser = Object.create(userFunctionStore);
   newUser.name = name;
   newUser.score = score;
   return newUser;
};
const userFunctionStore = {
   increment: function() {
        const add1 = () => { this.score++; } // this is the this of increment, which is user1   
        add1() 
    }
};
const user1 = userCreator("Will", 3);
const user2 = userCreator("Tim", 5);
user1.increment(); // this is user1
```

- To specify what `this` is, we can manually call the function with `call` or `apply`

### `new` keyword

```
function userCreator(name, score){
  this.name = name;
  this.score = score;
}
userCreator.prototype.increment = function(){ this.score++; };
userCreator.prototype.login = function(){ console.log("login"); };
const user1 = new userCreator(“Eva”, 9)
user1.increment()
```

1. `new` declares an empty object with the label of `this`, the function `userCreator` then assigns the property names and values to it
2. `new` assigns the `__proto__` property to `this` object with the value of `userCreator.prototype`
3. `new` returns the `this` object

- `new` is therefore just a shortcut to what we did earlier with `Object.create()`

### `class` keyword

- `class` is just a shortcut that allows us to write on the prototype object all in one place

```
class UserCreator {
  constructor (name, score){
    this.name = name;
    this.score = score;
  }
  increment (){ this.score++; }
  login (){ console.log("login"); }
}
const user1 = new UserCreator("Eva", 9);
user1.increment();
```
