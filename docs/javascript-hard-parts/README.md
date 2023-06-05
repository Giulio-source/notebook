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