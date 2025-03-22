# State Management

- [Signals](#signals)

**Recommended Libraries**

- `jotai`
- `@tanstack/react-query`

## Signals

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

## Keep State Low

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

## Contexts For Localized State
