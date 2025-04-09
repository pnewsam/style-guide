# 🎨 STYLING

## CSS Over JS

In most cases, CSS will be more performant than JS. Opt to use CSS whenever the alternative is presented.

```jsx
// ❌ Don't do this
const DropdownMenu = () => {
    const [open, setOpen] = useState(false);
    return (
        <button onClick={() => { setOpen(!open) }}>
            {isOpen
                ? <ChevronRight />
                : <ChevronDown />
            }
        </button>
    )
};

// ✅ Do this
const DropdownMenu = () => {
    const [open, setOpen] = useState(false);
    return (
        <button aria-open={open} onClick={() => { setOpen(!open) }}>
            <ChevronRight className="[aria-open='true']:rotate-90" />
        </button>
    );
}
```
