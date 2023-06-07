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