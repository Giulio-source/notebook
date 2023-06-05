# Typescript
  https://www.typescript-training.com/

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
```

## is
```
type CarLike = {
  model: string;
  year: number;
  make: string;
};

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

## Indexed Access Types

```
type Person = {
    name: string
    surname: string
    age: number
    favorites: {
        food: string
        series: string
    }
}

const favorites: Person['favorites'] = {
    food: 'pizza',
    series: 'black mirror'
}
```

## Mapped Types

Dictionary: As general as it gets, can have any key of type string

```
type Dictionary = {
  [key: string]: any;
};

function showDictionary(dictionary: Dictionary) {
  const item = dictionary.mango; // Typescript does not see this as an error
}
```

Record: Set of keys, more specific than dictionary

```
type FruitRecord = {
  [FruitKey in "apple" | "orange"]: any;
  // [key: "apple" | "orange"]: any; WRONG SYNTAX! DO NOT USE!
};

function showFruitRecord(fruits: FruitRecord) {
  const item = fruits.mango; // Typescript shows error cause it knows only apple and orange keys are allowed
  const item2 = fruits.apple; // Typescript is happy
}
```

## Template literal

```
type FunctionPrefix = "is" | "get" | "set";
type Values = "height" | "width";

type FunctionValues = `${FunctionPrefix}${Capitalize<Values>}`;
// "isHeight" | "isWidth" | "getHeight" | "getWidth" | "setHeight" | "setWidth"
```

## Key mapping

```
type ApiResponse = {
  stores: string;
  userPreferences: {
    mobile: boolean;
    theme: "dark" | "light";
  };
  id: number;
};

// Using keyword `as` for renaming (remapping) the keys
type ApiSDK = {
  [K in keyof ApiResponse as `set${Capitalize<K>}`]: (
    arg: ApiResponse[K]
  ) => void;
};

// MAGIC HAPPENS ❤️
// type ApiSDK = {
//     setStores: (arg: string) => void;
//     setUserPreferences: (arg: {
//         mobile: boolean;
//         theme: "dark" | "light";
//     }) => void;
//     setId: (arg: number) => void;
// }

function load(apiSdk: ApiSDK) {
  apiSdk.setId(4);
  apiSdk.setStores("milano");
  apiSdk.setUserPreferences({ mobile: false, theme: "dark" });
}
```