# Core

- [Project Structure](#project-structure)
- [File Naming](#file-naming)
- [Component Naming](#component-naming)
- [Component Tree Design](#component-tree-design)
  - [Avoid Coupling](#avoid-coupling)
- [Component Structure](#component-structure)
- [Component Design](#component-design)
  - [Single Responsibility](#single-responsbility)
  - [No Render Functions](#no-render-functions)

## Project Structure

```
src
|
+-- components   # Shared components across the application, may be domain-specific
|  |
|  +-- ui        # Shared UI components, not domain-specific (ie. button or dropdown)
|
+-- state
|  |
|  +-- atoms
|  |
|  +-- contexts
|
+-- features
|  |
|  +-- [NAME]
|
+-- queries
+  |
```

## File Naming

Name files in **kebab-case**.

## Component Naming

**Short Locally, Long Globally**: Names can be short and generic if they are locally scoped. For any global

## Component Tree Design

### Branch High Up

Rather than

```jsx
const Tell = () => <div>{}</div>;
```

## Component Structure

**Imports** should be ordered by relative distance. Absolute imports first, project imports second, then relative imports last. Use a prettier plugin like [prettier-plugin-sort-imports](https://github.com/trivago/prettier-plugin-sort-imports) to automate this.

**Exports** should be always be named, as default exports introduce renames.

```tsx
// Absolute, Project-Wide, then Relative Imports
import { useState } from "react";

import { Button } from "@/components/button";

import { SubmitButton } from "./SubmitButton";

// State, then Derived State, then JSX
const DatabaseList = ({ databases }) => {
  const [query, setQuery] = useState('');

  const handleChange = (e) => setQuery(e.target.value);
  const filtered = databases.filter((db) => db.name.toLowerCase().includes(query.toLowerCase()));

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

// Named exports, rather than "export default"
export { DatabaseList };
```
## Component Design

### Small Components

Components should be small -- under 50 lines if possible, and under 200 lines if not.

```tsx
// DO THIS ✅

// NOT THIS ❌
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

### Single Responsibility

Components should adhere to the principle of **self-containment**, and **single responsibility**.

```tsx
// DO THIS ✅

// NOT THIS ❌
```

> References
>
> - React Docs


```tsx
// Encourages naming consistency ✅
import { Button } from "@/components/Button";

// Encourages inconsistencies in naming ❌
import MyButton from "@/components/Button";
```

### No Render Functions

If it returns JSX, it should be a component. And if it's a component, it should almost always be in its own file.

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
```

```tsx
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

