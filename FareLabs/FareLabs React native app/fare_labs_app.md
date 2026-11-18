# Fare Labs Mobile Application - Complete Codebase Vault
#react-native #fare-labs #architecture #tanstack-query #zustand #axios #nativewind

---

## 🗺️ 1. Project Directory Map (`src/`)

Based on your `src` folder structure, the application is modularized into **6 distinct architectural layers**:

```text
src/
├── api/                  # Network clients & backend endpoint definitions
│   ├── client.ts         # Axios instance & OS-specific localhost resolution
│   └── auth.ts           # Authentication API calls (/auth/userLogin, /role/getAllRoles)
├── hooks/                # TanStack Query custom hooks (Server State)
│   └── useAuth.ts        # useLogin() mutation & error extraction
├── store/                # Zustand global stores (Client State)
│   ├── useUserStore.ts   # Logged-in username, selected role, roles list
│   └── useSampleStore.ts # Selected sample details & workflow state
├── navigation/           # App-wide layout and routing bars
│   ├── TopBar.tsx        # Header navigation bar with user/role context
│   └── BottomBar.tsx     # Bottom tab navigation (Home, Assigned, Drafts)
├── screens/              # Full-page application views
│   ├── Login.tsx         # Authentication screen
│   ├── MainScreen.tsx    # Authenticated shell wrapping TopBar, Screens, & BottomBar
│   ├── HomeScreen.tsx    # Dashboard overview for Sampling Executive
│   ├── Assigned.tsx      # List of assigned samples
│   ├── AssignedDetail.tsx# Detailed view & actions for a specific assigned sample
│   ├── Draft.tsx         # List of saved draft samples
│   └── DraftDetail.tsx   # Detailed view for editing/submitting a draft sample
└── components/           # Reusable UI building blocks
    ├── Button.tsx        # Primary action button
    ├── Checkbox.tsx      # Custom toggle checkbox ("Remember me", etc.)
    ├── DropDownBox.tsx   # Native Picker with invisible overlay & snake_case formatter
    ├── PopUp.tsx         # Modal dialog for confirmations/alerts
    ├── SampleAssignCard.tsx # Summary card used in Assigned/Draft lists
    ├── SampleDetailCard.tsx # Comprehensive data card used in Detail screens
    ├── SearchBar.tsx     # Search input for filtering sample lists
    ├── StatusBadge.tsx   # Color-coded status pill (Assigned, Draft, Completed)
    └── TextInputBox.tsx  # Styled text input for forms and login
```

---

## 🧠 2. Core Architectural Mental Model

> [!info] Client State vs. Server State
> The app strictly separates **data living inside the phone** from **data coming from the backend server**:
> 1. **Local UI State (`useState`)**: Form inputs (`user`, `password`), checkboxes (`rememberMe`), search queries, and popup visibility.
> 2. **Global Client State (`Zustand`)**: Cross-screen app memory (`useUserStore` for who is logged in, `useSampleStore` for which sample card the user tapped).
> 3. **Server State (`TanStack Query` + `Axios`)**: Asynchronous API communication, loading spinners (`isPending`, `isLoading`), caching (`queryKey`), and error parsing (`isAxiosError`).

```
[Login.tsx (UI)] 
       │
       ▼ calls mutate({ userName, password })
[useAuth.ts (TanStack Hook)] 
       │
       ├──► calls ──► [auth.ts] ──► [client.ts (Axios)] ──► 🌐 Backend (:3002)
       │
       ▼ onSuccess()
[useUserStore.ts (Zustand)] ──► Navigates to ──► [MainScreen.tsx]
```

---

## 🌐 3. Network & API Layer (`src/api/`)

### 3.1 `src/api/client.ts` — The Axios Blueprint
Handles the **React Native Localhost Trap**. Because an Android Emulator runs inside its own virtual router, `localhost` points to the virtual phone itself rather than the host Mac. Android emulators reserve `10.0.2.2` to bridge back to the host machine's `127.0.0.1`.

```typescript
import axios from 'axios';
import { Platform } from 'react-native';

const BASE_URL = Platform.OS === 'android'
  ? 'http://10.0.2.2:3002/api/v1'
  : 'http://localhost:3002/api/v1';

export const devFarelabsBackendApi = axios.create({
  baseURL: BASE_URL,
  headers: {
    'Content-Type': 'application/json',
  },
});
```

> [!tip] Physical Device Testing
> When testing on a physical phone over Wi-Fi, replace `localhost` / `10.0.2.2` with your computer's local LAN IPv4 address (e.g., `http://192.168.1.X:3002/api/v1`).

---

### 3.2 `src/api/auth.ts` — Endpoint Definitions
Encapsulates raw HTTP calls and strips away Axios wrappers so the rest of the app only deals with clean data or thrown errors.

```typescript
import { devFarelabsBackendApi } from './client';

export const authApi = {
  // POST /api/v1/auth/userLogin
  login: async (credentials: { userName: string; password: string }) => {
    const response = await devFarelabsBackendApi.post('/auth/userLogin', credentials);
    
    if (response.data.status !== false) {
      return response.data;
    }
    
    throw new Error(response.data.message || 'Invalid credentials');
  },

  // GET /api/v1/role/getAllRoles (Optional/Dynamic Role Fetching)
  getAllRoles: async (): Promise<string[]> => {
    const response = await devFarelabsBackendApi.get('/role/getAllRoles');
    
    if (response.data.status && Array.isArray(response.data.data)) {
      return response.data.data
        .filter((item: any) => item.isActive && !item.safeDelete)
        .map((item: any) => item.roleName);
    }
    throw new Error('Failed to fetch roles');
  },
};
```

---

## 🪝 4. Custom Hooks Layer (`src/hooks/`)

### 4.1 `src/hooks/useAuth.ts` — TanStack Query Wrapper
Wraps `authApi.login` inside `useMutation`. It bridges **Server State** (API response) with **Client State** (`useUserStore`) and digs into `error.response.data.message` when Axios throws HTTP 400/401/404 status errors.

```typescript
import { useMutation } from "@tanstack/react-query";
import { Alert } from "react-native";
import { isAxiosError } from "axios";
import { authApi } from "../api/auth";
import { useUser } from "../store/useUserStore";

export function useLogin() {
  const passUser = useUser((state) => state.setUser);
  const passRole = useUser((state) => state.setRole);

  return useMutation({
    mutationFn: authApi.login,
    onSuccess: (data, variables) => {
      // Save authenticated session info into Zustand
      passUser(variables.userName);
      passRole("sampling_executive");
    },
    onError: (error: any) => {
      let errorMessage = "Unable to connect to server";

      // 1. Extract backend JSON message on 4xx/5xx HTTP errors
      if (isAxiosError(error)) {
        errorMessage = error.response?.data?.message || error.message;
      } 
      // 2. Extract message if 200 OK returned { status: false }
      else if (error instanceof Error) {
        errorMessage = error.message;
      }

      Alert.alert("Login Failed", errorMessage);
    },
  });
}
```

#### Why `isAxiosError` is Critical:
* When a backend returns status `401 Unauthorized` with `{ "status": false, "message": "Incorrect password" }`, Axios immediately rejects the promise before `auth.ts` can read `response.data`.
* Default `error.message` only says `"Request failed with status code 401"`.
* `error.response?.data?.message` extracts the exact human-readable reason sent by your backend.

---

## 🗄️ 5. Global Client State (`src/store/`)

### 5.1 `src/store/useUserStore.ts`
Stores the active user's session details across all screens (`TopBar`, `HomeScreen`, etc.).

```typescript
import { create } from "zustand";

interface UserStore {
  user: string;
  role: string;
  rolesList: string[];
  setUser: (user: string) => void;
  setRole: (role: string) => void;
  setRolesList: (roles: string[]) => void;
}

export const useUser = create<UserStore>((set) => ({
  user: "",
  role: "",
  rolesList: [],
  setUser: (user) => set({ user }),
  setRole: (role) => set({ role }),
  setRolesList: (rolesList) => set({ rolesList }),
}));
```

### 5.2 `src/store/useSampleStore.ts`
Manages the currently selected sample object when navigating from a list screen (`Assigned.tsx` or `Draft.tsx`) to a detail screen (`AssignedDetail.tsx` or `DraftDetail.tsx`), avoiding messy route params and keeping sample updates synchronized.

---

## 📱 6. Screens Layer (`src/screens/`)

### 6.1 `src/screens/Login.tsx`
The entry point of the application.
* **Responsibilities**:
  * Collects `user`, `password`, `role` (`"sampling_executive"`), and `rememberMe`.
  * Calls `useLogin()` hook on "Sign In" press.
  * Swaps the `Button` for an `ActivityIndicator` whenever `isPending` is `true` to prevent duplicate network requests.
  * Navigates to `"MainScreen"` inside the `onSuccess` callback.

```tsx
const { mutate: login, isPending } = useLogin();

const handelSignin = () => {
  if (!user.trim() || !password.trim()) {
    Alert.alert("Validation", "Please enter username and password");
    return;
  }

  login(
    { userName: user, password },
    {
      onSuccess: () => navigation.navigate("MainScreen"),
    }
  );
};
```

### 6.2 `src/screens/MainScreen.tsx` & Navigation (`TopBar.tsx`, `BottomBar.tsx`)
* **`MainScreen.tsx`**: Acts as the authenticated layout container, rendering `TopBar` at the top, the active tab screen in the center, and `BottomBar` at the bottom.
* **`TopBar.tsx`**: Reads `user` and `role` from `useUserStore` to display the logged-in executive's identity and profile actions.
* **`BottomBar.tsx`**: Controls tab switching between `HomeScreen`, `Assigned`, and `Draft`.

### 6.3 Sample Workflow Screens
* **`HomeScreen.tsx`**: Displays summary metrics and quick navigation cards for the Sampling Executive.
* **`Assigned.tsx` & `AssignedDetail.tsx`**:
  * `Assigned.tsx` renders a searchable list of assigned samples using `SearchBar` and `SampleAssignCard`.
  * Tapping a card stores the sample in `useSampleStore` and opens `AssignedDetail.tsx`, which renders `SampleDetailCard` for sample collection/gate-in actions.
* **`Draft.tsx` & `DraftDetail.tsx`**:
  * Displays partially completed sample records (`Draft.tsx`) and allows the executive to resume editing or submit final details (`DraftDetail.tsx`) with confirmation via `PopUp.tsx`.

---

## 🧩 7. Reusable Components (`src/components/`)

### 7.1 `src/components/DropDownBox.tsx` — The Invisible Overlay Pattern
Solves two common React Native challenges:
1. **UI Clipping Fix**: Native `<Picker>` has rigid OS padding that clips text inside compact `h-11` (`44px`) containers. We render a clean Tailwind `<View>` + `<Text>` for visuals and stretch an invisible `<Picker style={{ position: 'absolute', opacity: 0 }} />` over top to capture native OS touches.
2. **Label Formatting (`formatLabel`)**: Converts database `snake_case` strings (`"sampling_executive"`) into clean UI titles (`"Sampling Executive"`) while preserving the raw database value in state.

```tsx
const formatLabel = (str: string) => {
  if (!str) return "";
  return str
    .split("_")
    .map((word) => word.charAt(0).toUpperCase() + word.slice(1))
    .join(" ");
};
```

### 7.2 Form & Feedback Components
* **`TextInputBox.tsx`**: Standardized labeled input supporting `secureTextEntry` for passwords and consistent Tailwind borders (`border-gray-200 rounded-md h-11 px-3`).
* **`Checkbox.tsx`**: Controlled boolean toggle box used for "Remember me" and form checklists.
* **`Button.tsx`**: Primary branded button (`bg-[#016db7]`) taking `text` and `onPress` props.
* **`SearchBar.tsx`**: Search filter bar used at the top of `Assigned` and `Draft` lists.
* **`StatusBadge.tsx`**: Dynamic badge component that changes background/text color based on sample status.
* **`SampleAssignCard.tsx` & `SampleDetailCard.tsx`**: Modular card views that display sample metadata, client info, and status badges.
* **`PopUp.tsx`**: Reusable modal overlay for confirming sample submissions or displaying alerts.

---

## 🚀 8. Cheat Sheet: How to Add a New Backend Endpoint

Whenever you need to connect a new screen (like `Assigned.tsx` or `DraftDetail.tsx`) to the backend, follow this exact 3-step pattern:

1. **Add Endpoint in `src/api/`**:
   ```typescript
   // src/api/samples.ts
   import { devFarelabsBackendApi } from './client';

   export const sampleApi = {
     getAssignedSamples: async () => {
       const res = await devFarelabsBackendApi.get('/samples/assigned');
       if (res.data.status) return res.data.data;
       throw new Error(res.data.message || 'Failed to load samples');
     }
   };
   ```
2. **Create Hook in `src/hooks/`**:
   * Use `useQuery` for **GET** requests (automatic fetching & caching).
   * Use `useMutation` for **POST / PUT / DELETE** requests (button-triggered actions).
   ```typescript
   // src/hooks/useSamples.ts
   import { useQuery } from '@tanstack/react-query';
   import { sampleApi } from '../api/samples';

   export function useAssignedSamples() {
     return useQuery({
       queryKey: ['samples', 'assigned'],
       queryFn: sampleApi.getAssignedSamples,
     });
   }
   ```
3. **Consume in Screen (`src/screens/Assigned.tsx`)**:
   ```tsx
   const { data: samples = [], isLoading } = useAssignedSamples();
   ```