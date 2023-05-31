# Typescript

## Function overload

```
function add(type: "numerico", value: number): number;
function add(type: "stringa", value: string): string;
function add(
  type: "numerico" | "stringa",
  value: number | string
): number | string {
  return value;
}

add("numerico", 4);
add("stringa", "345");
```

## this Type
```
function onClick(this: HTMLButtonElement, event: Event) {
  this.disabled = true;
}

type CarLike = {
  model: string;
  year: number;
  make: string;
};
```

## is
```
function isCarLike(value: any): value is CarLike {
  return (
    "model" in value &&
    typeof value.model === "string" &&
    "year" in value &&
    typeof value.year === "number" &&
    "make" in value &&
    typeof value.make === "string"
  );
}

const carObject: any = {
  model: "500",
  year: 2012,
  make: "Fiat",
  color: "yellow",
};

if (isCarLike(carObject)) {
  carObject;
}
```

## Ternary operator
The only way to use ternary operator is using T and extends

```
type Car = {
  model: string;
  year: number;
};

type Boat = {
  party: string;
  speed: number;
};

type Vehicle<T> = T extends "car" ? Car : Boat;

function exampleFunction(
  vehicle1: Vehicle<"car">,
  vehicle2: Vehicle<"anythingElse">
) {
  // hover over the params to see type
  vehicle1; // Car 😍
  vehicle2; // Boat 😁
}
```

## Extract
```
type Colors =
  | "dodgerblue"
  | "orchid"
  | "tomato"
  | "peru"
  | [number, number, number]
  | { r: number; g: number; b: number };

type StringColors = Extract<Colors, string>; // "dodgerblue" | "orchid" | "tomato" | "peru"
type ObjectColors = Extract<Colors, { r: number }>; // { r: number; g: number; b: number }
type TupleColors = Extract<Colors, [number, number, number]>; // [number, number, number]
```

## Exclude
```
type NonStringColors = Exclude<Colors, string>; // { r: number; g: number; b: number } | [number, number, number]
```

## Infer

In this example ConstructorArgs is able to get the type of the first argument of a newable object ( which is the constructor ).
In this case I'm using the Date class, so dateValue can only be `number | string | Date`

```
type ConstructorArgs<T> = T extends {
  new (constructor: infer A, ...args: any[]): any;
}
  ? A
  : never;

const dateValue: ConstructorArgs<typeof Date> = true; // Typescript Error
const dateValue2: ConstructorArgs<typeof Date> = '14/10/1993'; // OK
```

# React

React with Brian Holt

## Setup

- prettier
- eslint
- typescript

## General

- Do not use React StrictMode, it calls all the apis twice
- Don't make a component super reusable and abstract if you only have to use it a bunch of times, it adds complexity for no reason

## Data fetching

- It's a good idea to use a customHook when doing data fetching
- Even better is using the React Query library, which is now standard

## Forms

- Forms can be used in a uncontrolled way, we can just use `new FormData(e.target);` when the `onSubmit` event occurs

## Error boundaries

- Can only be done with class components
- Docs: https://legacy.reactjs.org/docs/error-boundaries.html

## Modals

- createPortal

## Contexts

- use it only if really necessary ( = if many parts of the application depend on it )

## Types

- refs:  `const elRef: MutableRefObject<HTMLDivElement | null> = useRef(null);`
- children: `({ children }: { children: React.ReactElement })`
- events: `(event: MouseEvent<HTMLElement>)` `import { MouseEvent } from "react";`
- event target: `(event.target instanceof HTMLElement)`
- tuple: `as [ string[], QueryStatus ]`
 
 ## Tests

 - use happy-dom and vitest
 - dont test the UI ( it changes all the time )
 - test the user story, not the implementation