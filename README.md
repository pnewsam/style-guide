# React Style Guide

An opinionated guide on best practices in React in 2025. It goes so far as to recommend specific libraries you should be using.

**References**

- [AirBnB Style Guide](https://github.com/airbnb/javascript) - Good But Outdated
- [Bullet Proof React](https://github.com/alan2207/bulletproof-react) - Good But Basic

**Values**

- 🧠 Reduce **Cognitive Load**
- 🪢 Avoid **Coupling**

**Recommended Libraries**

- `jotai`
- `@tanstack/react-query`
- `zod`

## 📂 DIRECTORY STRUCTURE

```
src
--\components
--\--\ui
--\state
--\--\contexts
--\--\atoms
--\pages
--\lib
--\queries
```

## 📄 FILE STRUCTURE

### Order Imports From Absolute to Relative

> Contributed by []

Package imports should come first, followed by top-level directories, and then local imports. Use a prettier plugin like [prettier-plugin-sort-imports](https://github.com/trivago/prettier-plugin-sort-imports) to automate this.

```jsx
import { useState } from "react";

import { Button } from "@/components/button";

import { SubmitButton } from "./SubmitButton";
```

### State/Hooks, Then Derived State & Business Logic, Then JSX

```tsx
const DatabaseList = ({ databases }) => {
  // 1. State
  const [query, setQuery] = useState("");

  // 2. Derived State & Business Logic
  const handleChange = (e) => setQuery(e.target.value);
  const filtered = databases.filter((db) =>
    db.name.toLowerCase().includes(query.toLowerCase())
  );

  // 3. JSX
  return (
    <div>
      <input value={query} onChange={handleChange} />
      <ul>
        {filtered.map((db) => {
          <li key={db.id}>{db.name}</li>;
        })}
      </ul>
    </div>
  );
};
```

### Avoid Default Exports

Default exports introduce the possibility of renames, and should be avoided.

```tsx
// DO THIS ✅
export const Button = ({ children }) => <button>{children}</button>;

// NOT THIS ❌
const Button = ({ children }) => <button>{children}</button>;
export default Button;
```

```tsx
// ENFORCES NAMING CONSISTENCY ✅
import { Button } from "@/components/Button";

// ENCOURAGES NAMING INCONSISTENCIES  ❌
import MyButton from "@/components/Button";
```

## STATE MANAGEMENT

Dependencies:

- `jotai`
- `@tanstack/react-query`

### Jotai for Global Atomic State

Use `jotai` for independent, atomic state that must be available globally. Common use-cases for global, atomic state include **User Sessions**, **Theme**

```tsx
// /state/atoms/user.ts
import { atom } from "jotai";

const userAtom = atom(user);

// /components/layout.tsx
const Layout = () => {
  const [user, setUser] = useAtom(userAtom);

  if (!user) return redirect("/login");

  return <main>{/* ... */}</main>;
};

// /components/user-menu.tsx
import { useAtom } from "jotai";
const UserMenu = () => {
  const [user, setUser] = useAtom();
  return <div></div>;
};
```

### Keep State Low

State should be be lifted only as high in the component tree as necessary.

```tsx
const FlavorsTable = () => {
    const [selected, setSelected] = useState<Flavor>(null);

    return (

    )
}

const FlavorsTableRow = () => {
    const [selected, s] = useState();

    return (

    );
}
```

### Contexts For Localized State

## 🌲 COMPONENT ARCHITECTURE

### Branch High Up

Rather than

```jsx
const Tell = () => <div>{}</div>;
```

## 🧩 COMPONENT DESIGN

### Functional Over Class

```

```

### One Job

Components should adhere to the principle of **single responsibility**.

```tsx
// DO THIS ✅

// NOT THIS ❌
```

> References
>
> - React Docs

### Short Names Locally, Long Names Globally

A name should only be as long as it needs to be.

### Small Components

Components should be under 50 lines if possible, and under 200 lines if not.

```tsx
// DO THIS ✅

// NOT THIS ❌
```

### Composition Over

```tsx
// DO THIS ✅

// NOT THIS ❌
```

### If It Returns JSX, Make It A Component

Avoid creating render functions that return JSX.

```tsx
// DON'T DO THIS ❌
const CarsTable = () => {
  const renderRow = ({ make, model }) => {
    return (
      <tr>
        <td>{make}</td>
        <td>{model}</td>
      </tr>
    )
  }

  return (
    <table>
      <thead>
        <tr>
          <th>Make</th>
          <th>Model</th>
        </tr>
      </thead>
      <tbody>
        {cars.map(car => renderRow(car))}
      </tbody>
    <table>
  )
}

// DO THIS ✅

// CarsTableRow.tsx
const CarsTableRow = ({ make, model }) => (
  <tr>
    <td>{make}</td>
    <td>{model}</td>
  </tr>
)

// CarsTable.tsx
import { CarsTableRow } from "./CarsTableRow"

const CarsTable = () => {
  return (
    <table>
      <thead>
        <tr>
          <th>Make</th>
          <th>Model</th>
        </tr>
      </thead>
      <tbody>
        {cars.map(car => <CarsTableRow {...car} />)}
      </tbody>
    <table>
  )
}
```

## 🎨 STYLING

## Prefer Tailwind

Tailwind

```

```

### CSS Over JS

CSS is usually more performant than Javascript. Opt to use CSS wherever possible.

```

```
