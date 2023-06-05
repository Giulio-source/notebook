# React
https://react-v8.holt.courses/

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