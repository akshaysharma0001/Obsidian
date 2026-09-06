**What is Jotai?**

Jotai is a primitive, flexible, and ultra-lightweight state management library for React based on an **atomic** model. It was heavily inspired by Recoil but built to be fully compatible with modern React.

**Core Concepts**

- **Atoms:** The smallest units of state. You define them using `atom(initialValue)` without needing string keys. State can be numbers, strings, arrays, or objects.

- **Derived Atoms (Selectors):** Read-only atoms that calculate values dynamically based on other atoms by passing a `get` function: `atom((get) => get(baseAtom) * 2)`.

- **Zero Config Setup:** Works globally out of the box without requiring a root provider wrapper for basic use cases.

**Essential Hooks**

- `useAtom(atom)`: Reads the value and returns a setter function (behaves like `useState`).
    
- `useAtomValue(atom)`: Extracts only the value, preventing unnecessary re-renders if you only need to read data.

- `useSetAtom(atom)`: Returns only the dispatcher/setter function without subscribing the component to state changes.

**Key Advantages**

- **Automatic Optimization:** Components only re-render when the specific atom they subscribe to updates.

- **No Prop Drilling:** Cleanly shares states globally across deeply nested component trees.
- **Active Maintenance:** Fully compatible with modern React versions and concurrent rendering modes.