# React Style Guide

An opinionated guide on best practices in React in 2025. It goes so far as to recommend specific libraries you should be using.

**References**

- [AirBnB Style Guide](https://github.com/airbnb/javascript) - Good But Outdated
- [Bullet Proof React](https://github.com/alan2207/bulletproof-react) - Good But Basic

**Values**

- 🧠 Reduce **Cognitive Load**
- 🪢 Avoid **Coupling**

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
import { useState } from 'react';

import { Button } from '@/components/button';

import { SubmitButton } from './SubmitButton';
```

### State/Hooks, Then Derived State & Business Logic, Then JSX

```tsx
const DatabaseList = ({ databases }) => {
  // State
  const [query, setQuery] = useState('');

  // Derived State & Business Logic
  const handleChange = (e) => setQuery(e.target.value);
  const filtered = databases.filter((db) => db.name.toLowerCase().includes(query.toLowerCase()));

  // JSX
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

Default exports introduce the possibility of renames.

```tsx
// DO THIS ✅
export const Button = ({ children }) => <button>{children}</button>;

// NOT THIS ❌
const Button = ({ children }) => <button>{children}</button>;
export default Button
```

## STATE MANAGEMENT

Dependencies:

- `jotai`
- `@tanstack/react-query`

### Jotai for Global Atomic State

Use `jotai` for independent, atomic state that must be available globally. Common use-cases for global, atomic state include **User Sessions**, **Theme**

```tsx
// /state/atoms/user.ts
import { atom } from 'jotai';
```

```tsx
// /components/user-menu.tsx
import { useAtom } from 'jotai';

const userAtom = atom(user);

const UserMenu = () => {
  const [user, setUser] = useAtom();
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
const Tell = () => (<div>
  {}
</div>);
```

## 🧩 COMPONENT DESIGN

### Functional Over Class

### One Job

### Small Components

### Composition Over

### Keep JSX in the Return Statement

Avoid creating render functions

```tsx

```

## 🎨 STYLING

### CSS Over JS

In most cases, CSS will be more performant than JS. Opt to use CSS wherever possible.

