# `useMemo`

`useMemo` is a built-in **React Hook** that caches the result of a calculation between re-renders.

> **Memoization** means storing the result of a previous calculation so React can reuse it instead of performing the same work again.

## How It Works

`useMemo` takes two arguments:

1. **Calculation function** — a function that returns the value you want to memoize.
    
2. **Dependency array** — a list of values that the calculation depends on.
    

```jsx
const memoizedValue = useMemo(() => {
  return calculateSomething(data);
}, [data]);
```

The calculation runs again only when one of the dependencies changes.

## Key Idea

`useMemo` is useful when:

- A calculation is **expensive** or time-consuming.
    
- You want to avoid repeating the same calculation on every render.
    
- The calculation depends on specific values that can be tracked in a dependency array.
    

**In short:** `useMemo` → **cache a calculated value and recompute it only when its dependencies change.**