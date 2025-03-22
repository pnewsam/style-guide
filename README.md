# React Style Guide

An opinionated guide on best practices in React in 2025.

**Table of Contents**

- Core
  - [Project Structure](./PROJECT_STRUCTURE.md)
  - [Component Design](./COMPONENT_DESIGN.md)
- [Routing](./ROUTING.md)
- [Data Fetching](./DATA_FETCHING.md)
- [State Management](./STATE_MANAGEMENT.md)
- [Styling](./STYLING.md)

**Recommendations**


**References**

- [AirBnB Style Guide](https://github.com/airbnb/javascript) - Good But Outdated
- [Bullet Proof React](https://github.com/alan2207/bulletproof-react) - Good But Basic

**Values**

- 🧠 Reduce **Cognitive Load**
- 🪢 Avoid **Coupling**

### Contexts For Localized State

## 🌲 COMPONENT ARCHITECTURE

### Branch High Up

Rather than

```jsx
const Tell = () => <div>{}</div>;
```

### Composition Over

```tsx
// DO THIS ✅

// NOT THIS ❌
```

