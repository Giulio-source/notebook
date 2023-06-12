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

```function outer() {
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