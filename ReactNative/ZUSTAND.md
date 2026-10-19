**Zustand** is a hook-based global state management library that stores your application state outside the React component tree. Instead of passing props down through intermediary screens (`MainScreen` \rightarrow `Assigned` \rightarrow `SampleDetailCard`) or wrapping your app in Context Providers, Zustand creates a standalone JavaScript store that any component can subscribe to directly.

## 1. Core Architecture & Mental Model

In standard React (`useState`), state lives **inside** a component. When that component unmounts (like when `activeTab` changes from `'assign'` to `'assignDetail'`), its local state is destroyed.

Zustand solves this using the **Publish/Subscribe (Pub/Sub) pattern** paired with React’s native `useSyncExternalStore` hook:

1. **External Store:** The state lives in a standalone JavaScript module in memory, completely independent of component lifecycles.
    
2. **Actions (`set`):** Functions defined inside the store update the state immutably (merging top-level properties automatically).
    
3. **Selectors:** Components subscribe to specific slices of the store (e.g., `state => state.selectedSample`). Zustand compares the previous slice value with the new value using strict equality (`Object.is`) and **only triggers a re-render in a component if its specific selected slice changed**.
    

## 2. Anatomy of Our Implementation (`useSampleStore.ts`)

Let's break down every line of the store we built for your app:

```typescript
import { create } from 'zustand';

// 1. Data Blueprint
export interface SampleData {
  SSFNumber: string;
  sampleDetail?: string;
  BD: string;
  SamplingLocation: string;
  Time: string;
  Date: string;
}

// 2. Store Contract (State + Actions)
interface SampleStore {
  selectedSample: SampleData | null;
  setSelectedSample: (sample: SampleData) => void;
}

// 3. Store Creation
export const useSampleStore = create<SampleStore>((set) => ({
  selectedSample: null,
  setSelectedSample: (sample) => set({ selectedSample: sample }),
}));
```

### Line-by-Line Mechanics

- `**create<SampleStore>(...)**`: A generic factory function from Zustand. Passing `<SampleStore>` enforces TypeScript validation so you cannot accidentally omit a required property or pass the wrong type.
    
- `**(set) => ({ ... })**`: Zustand passes a `set` function into your store initializer. `set` is the internal engine used to mutate state and notify all listening components.
    
- `**selectedSample: null**`: The initial state before the user clicks any card.
    
- `**setSelectedSample: (sample) => set({ selectedSample: sample })**`: An **Action**. When called with a sample object, `set()` shallow-merges `{ selectedSample: sample }` into the store and alerts any component listening to `selectedSample`.
    

## 3. How Data Flows Through Your App

Here is the exact step-by-step lifecycle when a user clicks **Proceed** on a sample card:

|Step|Component|Code Executed|What Happens Under the Hood|
|---|---|---|---|
|**1. Subscribe to Action**|`SampleDetailCard.tsx`|`const setSelectedSample = useSampleStore((state) => state.setSelectedSample);`|Extracts **only** the setter function. Because `setSelectedSample` never changes, `SampleDetailCard` will **never** re-render when `selectedSample` changes.|
|**2. Write Data**|`SampleDetailCard.tsx`|`setSelectedSample({ SSFNumber, BD, ... })`|User taps "Proceed". The card pushes its own props into the Zustand store.|
|**3. Switch View**|`SampleDetailCard.tsx`|`setActiveTab('assignDetail')`|Tells `MainScreen` to unmount `<Assigned/>` and mount `<AssignedDetail/>`.|
|**4. Read Data**|`AssignedDetail.tsx`|`const selectedSample = useSampleStore((state) => state.selectedSample);`|`<AssignedDetail/>` mounts, reaches into the Zustand store, grabs the exact object saved in Step 2, and renders it inside `<SampleDraftCard/>`.|

## 4. Essential Zustand Patterns for Your Study Vault

### A. Selecting State (The Golden Rule)

Always select the smallest piece of state your component needs.

- **Good (Atomic Selector):**
    
    ```tsx
    const selectedSample = useSampleStore((state) => state.selectedSample);
    ```
    
    _Why:_ Component only re-renders when `selectedSample` changes.
    
- **Bad (Destructuring the whole store):**
    
    ```tsx
    const { selectedSample } = useSampleStore();
    ```
    
    _Why:_ Subscribes the component to the **entire** store. If you later add `searchQuery` to `useSampleStore`, `AssignedDetail` will unnecessarily re-render on every keystroke in the search bar.
    

### B. Updating State Based on Previous State

Just like React's `useState`, if your new state depends on the old state (for example, adding a newly created draft to an array of drafts), pass a callback function to `set`:

```typescript
interface DraftStore {
  drafts: SampleData[];
  addDraft: (newDraft: SampleData) => void;
}

export const useDraftStore = create<DraftStore>((set) => ({
  drafts: [],
  addDraft: (newDraft) =>
    set((state) => ({
      drafts: [...state.drafts, newDraft],
    })),
}));
```

### C. Reading State Inside the Store (`get`)

Alongside `set`, `create` provides a second parameter called `get`. This lets an action read current state values without passing them in as arguments:

```typescript
export const useAuthStore = create<AuthStore>((set, get) => ({
  username: 'akshay',
  role: 'Executive',
  logCurrentUser: () => {
    const currentRole = get().role;
    console.log(`Active role is ${currentRole}`);
  },
}));
```

### D. Resetting State (e.g., on Logout)

Define an initial state object outside `create` so you can easily reset the store back to defaults:

```typescript
const initialState = {
  selectedSample: null,
};

export const useSampleStore = create<SampleStore>((set) => ({
  ...initialState,
  setSelectedSample: (sample) => set({ selectedSample: sample }),
  clearSample: () => set(initialState),
}));
```

### E. Persisting State Across App Restarts (`persist` middleware)

In React Native, if you want your Auth store ("Remember me") or saved Drafts to survive closing the app, wrap the store with Zustand's `persist` middleware and `@react-native-async-storage/async-storage`:

```typescript
import { create } from 'zustand';
import { persist, createJSONStorage } from 'zustand/middleware';
import AsyncStorage from '@react-native-async-storage/async-storage';

export const useAuthStore = create(
  persist(
    (set) => ({
      username: '',
      role: '',
      login: (username: string, role: string) => set({ username, role }),
    }),
    {
      name: 'farelabs-auth-storage', // unique key in AsyncStorage
      storage: createJSONStorage(() => AsyncStorage),
    }
  )
);
```

## 5. Summary Comparison: Local `useState` vs. Zustand

- **Use Local `useState`** when only one component cares about the value (e.g., `const [pop, setPop] = useState(false)` for opening a modal, or typing inside a local `Remarks` input before hitting Save).
    
- **Use Zustand** when data needs to survive screen/tab transitions (`selectedSample`), or when multiple distant components need access to the same truth (logged-in user role, saved drafts list, global search filters).