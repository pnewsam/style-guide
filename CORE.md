# Core

These principles should apply to any React project, whether you are working in big NextJS app or a minimal SPA using Vite.

- [Core](#core)
  - [1. Project Structure](#1-project-structure)
  - [2. File Naming](#2-file-naming)
  - [3. Component Naming](#3-component-naming)
  - [4. Tree Design](#4-tree-design)
    - [4.1. Keep State Low](#41-keep-state-low)
    - [4.2. Branch High Up](#42-branch-high-up)
  - [5. Component Layout](#5-component-layout)
  - [6. Component Design](#6-component-design)
    - [6.1 Small Components](#61-small-components)
    - [6.2 Single Responsibility](#62-single-responsibility)
    - [6.3 Composition Over Configuration](#63-composition-over-configuration)
    - [6.4 Reusable Logic in Hooks](#64-reusable-logic-in-hooks)
    - [6.5 No Render Functions](#65-no-render-functions)

## 1. Project Structure

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

## 2. File Naming

Name files in **kebab-case**. Projects such as NodeJS and ShadCN/UI have adopted kebab-case for their code. Kebab-case is compatible across different operating systems.

```
// ✅ Do this
alert-dialog.tsx

// ❌ Not this
AlertDialog.tsx
```

> Reference: https://x.com/rwieruch/status/1836434009041035635

## 3. Component Naming

**Short Locally, Long Globally**

```tsx
const CarsListHeader = () => {};
```

> Reference: https://x.com/_georgemoller/status/1892197768053002581

## 4. Tree Design

### 4.1. Keep State Low

State should be be lifted only as high in the component tree as necessary.

### 4.2. Branch High Up

Rather than

```jsx
const Tell = () => <div>{}</div>;
```

## 5. Component Layout

**Imports** should be ordered by relative distance. Absolute imports first, project imports second, then relative imports last. Use a prettier plugin like [prettier-plugin-sort-imports](https://github.com/trivago/prettier-plugin-sort-imports) to automate this.

**Exports** should be always be named, as default exports introduce renames.

```tsx
// Absolute, Project-Wide, then Relative Imports
import { useState } from "react";

import { Button } from "@/components/button";

import { SubmitButton } from "./SubmitButton";

// State, then Derived State, then JSX
const DatabaseList = ({ databases }) => {
  const [query, setQuery] = useState("");

  const handleChange = (e) => setQuery(e.target.value);
  const filtered = databases.filter((db) =>
    db.name.toLowerCase().includes(query.toLowerCase())
  );

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

## 6. Component Design

### 6.1 Small Components

Components should be small -- under 50 lines if possible, and under 200 lines if not. It is only very rarely that they need to be longer than ~200 lines.

```tsx
// DO THIS ✅

// NOT THIS ❌
```

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

### 6.2 Single Responsibility

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

### 6.3 Composition Over Configuration

Rather than "configuring" a component by passing down many props, "compose" it by breaking it up into smaller subcomponents, each with its own props.

Besides resulting in smaller, self-contained components that are easier to reason about, update, and replace, this also more effectively leverages Reacts reconciliation to minimize re-renders.

```tsx
// Do this ✅
const ItemsTable = ({ items }) => {
  const [selectedItem, setSelectedItem] = useState(null);
  return (
    <Table>
      <TableHeader title="Items" subtitle={`${items.length} total`} />
      <TableBody items={items} selectedItem={selectedItem} />
      <TableFooter numItems={items.length} pageSize={10} />
    </Table>
  );
};

// Not this ❌
const ItemsTable = ({ items }) => {
  const [selectedItem, setSelectedItem] = useState(null);
  return (
    <Table
      title="Items"
      subtitle={`${items.length} total`}
      selectedItem={selectedItem}
      items={items}
      numItems={items.length}
      pageSize={10}
    />
  );
};
```

### 6.4 Reusable Logic in Hooks

### 6.5 No Render Functions

Render functions are functions that return JSX but are broken out from the component's return statement. Although they

If it returns JSX, it should be a component. And if it's a component, it should almost always be in its own file.

```tsx
// DON'T DO THIS ❌
const CarsTable = () => {
  const [cars, setCars] = useState([]);

  const renderRows = () => {
    return (
      <>
        {cars.map((car) => (
          <tr>
            <td>{car.make}</td>
            <td>{car.model}</td>
          </tr>
        ))}
      </>
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
        {renderRows}
      </tbody>
    <table>
  )
}

// DO THIS ✅

// CarsTableRow.tsx
const CarsTableRow = ({ car }) => (
  <tr>
    <td>{car.make}</td>
    <td>{car.model}</td>
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
        {cars.map(car => <CarsTableRow car={car} />)}
      </tbody>
    <table>
  )
}
```
