---
title: React Native Master Guide — From Basics to Enterprise Architecture
tags:
  - react-native
  - expo
  - typescript
  - zustand
  - tanstack-query
  - axios
  - nativewind
  - architecture
  - fare-labs
created: 2026-11-16
---

# 📱 React Native Master Guide: From Basics to Enterprise Architecture

> [!abstract] Vault Overview
> This master note documents **React Native from foundational primitives to production-grade enterprise patterns**, grounded directly in the code and architecture of the **Fare Labs Sampling Executive App**.
> 
> ### 📑 Table of Contents
> 1. [[#1. React Native Fundamentals & Core Primitives]]
> 2. [[#2. Styling with NativeWind & Mobile Flexbox]]
> 3. [[#3. Component Architecture & Advanced UI Patterns]]
> 4. [[#4. Navigation, Stack History & Hardware Back Button]]
> 5. [[#5. The 3 Pillars of State Management (Local, Zustand, TanStack Query)]]
> 6. [[#6. Networking, Axios Interceptors & The Localhost Trap]]
> 7. [[#7. JWT Authentication, SecureStore & "Remember Me" Hydration]]
> 8. [[#8. Native Device Hardware APIs (GPS Location & Camera/Gallery)]]
> 9. [[#9. Enterprise Folder Structure & Developer Cheat Sheet]]

---

## 1. React Native Fundamentals & Core Primitives
#fundamentals #components #jsx

### 1.1 How React Native Differs from React Web
In React for the Web, your JSX compiles into HTML DOM elements (`<div>`, `<p>`, `<input>`, `<button>`). A mobile operating system (iOS or Android) does not have a browser DOM. Instead, React Native translates your components into **real native UI views** (`android.view.View` on Android, `UIView` on iOS).

| Web HTML Element | React Native Equivalent | Key Rule / Behavior |
| :--- | :--- | :--- |
| `<div>` | `<View>` | A layout container. Cannot contain raw text directly. |
| `<p>`, `<span>`, `<h1>` | `<Text>` | **All text must be wrapped inside `<Text>`**. Raw strings inside a `<View>` will crash the app. |
| `<input type="text">` | `<TextInput>` | Uses `onChangeText={(text) => ...}` instead of `onChange={(e) => e.target.value}`. |
| `<button>` | `<Pressable>` | Modern touch wrapper supporting `onPress`, `active:opacity-80`, and custom styling. |
| `<img>` | `<Image>` | Uses `source={require('./path.png')}` for local files or `source={{ uri: 'https://...' }}` for remote/picked images. |
| `<div>` with `overflow: scroll` | `<ScrollView>` | Screens do **not** scroll by default on mobile; you must wrap overflowing content in `<ScrollView>` or `<FlatList>`. |

---

### 1.2 Essential Core Components Used in Fare Labs

#### A. `<View>` and `<Text>`
```tsx
<View className="px-4 py-2 bg-white">
  <Text className="text-base font-semibold text-gray-900">
    Upload Photos
  </Text>
</View>
```

#### B. `<TextInput>` (Controlled Inputs & Multiline Textareas)
In React Native, `<TextInput>` handles both single-line inputs (like `Username` and `Password`) and multi-line textareas (like `Remarks` in `AssignedDetail.tsx`):

```tsx
// Single-line Password Input (from TextInputBox.tsx)
<TextInput
  value={value}
  onChangeText={onChangeText}
  placeholder="Password"
  secureTextEntry={true} // Masks characters as dots (••••••)
  className="border border-gray-200 rounded-md h-11 px-3"
/>

// Multi-line Remarks Box (from AssignedDetail.tsx)
<TextInput
  className="border border-gray-200 rounded-md p-3 my-3 h-24 text-sm text-gray-700"
  placeholder="Remarks......"
  placeholderTextColor="#9ca3af"
  multiline={true}
  textAlignVertical="top" // Critical on Android: forces cursor to start at top-left instead of vertical center
/>
```

#### C. `<KeyboardAvoidingView>` & `Platform.OS`
When a user taps a `<TextInput>`, the mobile software keyboard slides up from the bottom and covers half the screen. `<KeyboardAvoidingView>` automatically shifts your inputs upward so they stay visible. Because iOS and Android calculate screen height differently when the keyboard opens, we check `Platform.OS`:

```tsx
import { KeyboardAvoidingView, Platform } from "react-native";

<KeyboardAvoidingView
  behavior={Platform.OS === "ios" ? "padding" : "height"}
  className="flex-1 bg-white"
>
  {/* Login Form Content */}
</KeyboardAvoidingView>
```

#### D. `<Modal>` & `statusBarTranslucent`
A `<Modal>` renders content above the entire screen (used in `PopUp.tsx` for alerts and `TopBar.tsx` for the profile logout menu).

> [!warning] The Android Modal Coordinate Gotcha
> On Android, a standard `<Modal>` starts measuring `top: 0` **below** the system status bar, whereas your main screen layout starts at the very top pixel of the physical display. Adding `statusBarTranslucent` forces the `<Modal>` to use the exact same coordinate system as the rest of your screen.

```tsx
<Modal
  transparent
  statusBarTranslucent // Aligns top/right coordinates with the physical screen top
  animationType="fade"
  visible={menuVisible}
  onRequestClose={() => setMenuVisible(false)} // Required for Android hardware back button
>
  {/* Modal Content */}
</Modal>
```

---

## 2. Styling with NativeWind & Mobile Flexbox
#nativewind #tailwind #flexbox

### 2.1 Mobile Flexbox vs. Web Flexbox
In React Native, **every `<View>` uses Flexbox by default** (`display: flex` is automatic). However, the default direction is **vertical**, not horizontal:

* **Web Default**: `flex-direction: row` (items sit side-by-side $\rightarrow$).
* **React Native Default**: `flexDirection: 'column'` (items stack top-to-bottom $\downarrow$).

#### Key Tailwind Classes Used Across the App:
* `flex-1`: Tells a `<View>` to expand and fill all available remaining space in its parent container.
* `flex-row`: Switches layout direction from vertical ($\downarrow$) to horizontal ($\rightarrow$).
* `justify-between`: Pushes child elements to opposite ends along the main axis.
* `items-center`: Centers child elements along the cross axis.
* `mt-auto`: Pushes a bottom button bar (like "Save Details" in `AssignedDetail.tsx`) all the way to the bottom of the screen.

### 2.2 Absolute Positioning & Anchoring (`TopBar.tsx` & `Login.tsx`)
When placing decorative background images (`login_bg.png`) or floating dropdown menus:
* `absolute`: Removes the element from normal layout flow and positions it relative to its nearest parent `<View>`.
* `top-[105px] right-6 z-50`: Pins the logout menu exactly $105\text{px}$ from the top and $24\text{px}$ (`right-6`) from the right edge, with `z-50` ensuring it floats above cards underneath.

---

## 3. Component Architecture & Advanced UI Patterns
#ui-patterns #typescript #reusable-components

### 3.1 TypeScript Props Pattern
Every reusable component in `src/components/` defines an explicit TypeScript `interface` so VS Code catches missing or mistyped props immediately.

```tsx
// src/components/Button.tsx
import React from "react";
import { Pressable, Text } from "react-native";

interface ButtonProps {
  text: string;
  onPress: () => void;
}

export default function Button({ text, onPress }: ButtonProps) {
  return (
    <Pressable
      onPress={onPress}
      className="bg-[#016db7] py-3 rounded-md items-center active:opacity-80"
    >
      <Text className="text-white font-semibold text-base">{text}</Text>
    </Pressable>
  );
}
```

### 3.2 The "Invisible Overlay" Pattern (`DropDownBox.tsx`)
Native mobile dropdowns (`@react-native-picker/picker`) have hardcoded OS internal padding that clips text when placed inside a sleek $44\text{px}$ (`h-11`) box.

> [!tip] Architectural Solution
> Render a custom `<View>` + `<Text>` for 100% pixel-perfect visual control, and stretch an invisible native `<Picker style={{ position: 'absolute', opacity: 0 }} />` directly over top of it. When the user taps the box, they are physically tapping the invisible native `<Picker>`, which triggers the native OS selection wheel/dialog!

```tsx
import React from "react";
import { View, Text } from "react-native";
import { Picker } from "@react-native-picker/picker";

interface DropdownBoxProps {
  text: string;
  selectedValue: string;
  onValueChange: (itemValue: string) => void;
  options: string[];
}

// Utility: Converts "sampling_executive" -> "Sampling Executive" for UI display
const formatLabel = (str: string) => {
  if (!str) return "";
  return str
    .split("_")
    .map((word) => word.charAt(0).toUpperCase() + word.slice(1))
    .join(" ");
};

export default function DropdownBox({
  text,
  selectedValue,
  onValueChange,
  options,
}: DropdownBoxProps) {
  return (
    <View className="my-2">
      <Text className="font-medium text-base mb-1">{text}</Text>
      <View className="border border-gray-200 rounded-md h-11 px-3 bg-white flex-row items-center justify-between relative overflow-hidden">
        {/* 1. Clean Visual Layer */}
        <Text className="text-sm text-gray-800" numberOfLines={1}>
          {formatLabel(selectedValue)}
        </Text>
        <Text className="text-gray-500 text-xs">▼</Text>

        {/* 2. Invisible Interactive Native Picker Layer */}
        <Picker
          selectedValue={selectedValue}
          onValueChange={(itemValue) => onValueChange(itemValue)}
          mode="dropdown"
          style={{
            position: "absolute",
            top: 0,
            left: 0,
            right: 0,
            bottom: 0,
            opacity: 0, // Invisible but captures touches!
          }}
        >
          {options.map((option, index) => (
            <Picker.Item
              key={index}
              label={formatLabel(option)} // Human-readable in dropdown menu
              value={option}              // Raw snake_case kept in state for API
            />
          ))}
        </Picker>
      </View>
    </View>
  );
}
```

---

## 4. Navigation, Stack History & Hardware Back Button
#navigation #backhandler #ux

### 4.1 Stack Navigation: `.navigate()` vs `.replace()`
React Navigation's Native Stack (`createNativeStackNavigator`) works like a deck of cards:

1. **`navigation.navigate("MainScreen")`**: Places `MainScreen` **on top** of `Login`. `Login` remains underneath in memory. If the user presses the Android back button, `MainScreen` is popped off and they land back on `Login`.
2. **`navigation.replace("MainScreen")`**: **Destroys** the `Login` card and replaces it with `MainScreen`. The user can never accidentally navigate back to the Login screen unless they explicitly log out.

```
Using .navigate():   [ Login ] ──► [ Login, MainScreen ] (Back goes to Login ❌)
Using .replace():    [ Login ] ──► [ MainScreen ]        (Login is destroyed ✅)
```

### 4.2 Custom Tab State + Hardware `BackHandler` (`MainScreen.tsx`)
Inside `MainScreen.tsx`, sub-screens (`home`, `assign`, `draft`, `assignDetail`, `draftDetail`) are switched using a React `useState` variable called `activeTab`. 

Because Android's physical back button only knows about React Navigation's Stack (and does not automatically know about your `activeTab` state), we register a `BackHandler` listener in `MainScreen.tsx`:

```tsx
useEffect(() => {
  const backAction = () => {
    if (activeTab === "assignDetail") {
      setActiveTab("assign");
      return true; // Returning true tells Android: "We handled the back press, do not exit the app!"
    }
    if (activeTab === "draftDetail") {
      setActiveTab("draft");
      return true;
    }
    if (activeTab === "assign" || activeTab === "draft") {
      setActiveTab("home");
      return true;
    }
    // When on 'home', return false so Android minimizes the app naturally
    return false;
  };

  const backHandler = BackHandler.addEventListener(
    "hardwareBackPress",
    backAction
  );

  // Cleanup listener when component unmounts or activeTab changes
  return () => backHandler.remove();
}, [activeTab]);
```

---

## 5. The 3 Pillars of State Management (Local, Zustand, TanStack Query)
#state-management #zustand #tanstack-query

> [!important] The Golden Rule of State Separation
> Never mix UI state, Global Client state, and Server state in the same tool. Each has a dedicated job in our architecture:

| State Type | Tool Used | What Belongs Here? | Example in Fare Labs |
| :--- | :--- | :--- | :--- |
| **1. Local UI State** | `useState` | Data that only matters to a single screen/component right now. | `password` input text, `menuVisible` modal toggle, `activeTab`. |
| **2. Global Client State** | **Zustand** (`src/store/`) | App-wide memory shared across multiple screens on the device. | `useUserStore` (`user`, `role`, `token`), `useSampleStore` (`selectedSample`). |
| **3. Server State** | **TanStack Query** (`src/hooks/`) | Data coming from or going to the backend API over HTTP. | `useLogin()` mutation, loading spinners (`isPending`), API caching. |

---

### 5.1 Pillar 2: Global Client State with Zustand (`src/store/`)
Zustand is a fast, unopinionated global state store that requires zero `<Provider>` wrappers.

#### Creating a Store (`src/store/useUserStore.ts`):
```typescript
import { create } from "zustand";

interface UserStore {
  user: string;
  role: string;
  token: string | null;
  setUser: (user: string) => void;
  setRole: (role: string) => void;
  setToken: (token: string | null) => void;
  logout: () => void;
}

export const useUser = create<UserStore>((set) => ({
  user: "",
  role: "",
  token: null,
  setUser: (user) => set({ user }),
  setRole: (role) => set({ role }),
  setToken: (token) => set({ token }),
  logout: () => set({ user: "", role: "", token: null }),
}));
```

#### Two Ways to Read Zustand State:
1. **Inside a React Component (Reactive Selector)**:
   ```tsx
   const userName = useUser((state) => state.user);
   ```
   *Why use a selector `(state) => state.user`?* It ensures the component **only** re-renders when `user` changes, ignoring changes to `token` or `role`.
2. **Outside a React Component (Static Snapshot via `.getState()`)**:
   ```typescript
   const token = useUser.getState().token;
   ```
   *Why is this superpower useful?* Plain TypeScript files like `src/api/client.ts` are not React components, so they cannot call hooks. `useUser.getState().token` lets our Axios interceptor grab the JWT token directly from memory!

---

### 5.2 Pillar 3: Server State with TanStack Query (`@tanstack/react-query`)

When communicating with a server, you perform two types of operations:

#### A. `useQuery` — For Reading Data (`GET` Requests)
* Runs **automatically** when the component mounts.
* Caches the response in memory using a unique `queryKey` array (e.g., `['samples', 'assigned']`).
* Provides `data`, `isLoading`, `isError`, and `refetch`.

#### B. `useMutation` — For Writing/Changing Data (`POST`, `PATCH`, `PUT`, `DELETE`)
* **Never runs automatically.** It waits until you pull the trigger (`mutate()`).
* Eliminates manual `try/catch` and `const [loading, setLoading] = useState(false)` boilerplate.
* Provides `mutate` (the trigger function) and `isPending` (automatic boolean spinner state).

```
User Taps "Sign In"
       │
       ▼
Calls login({ userName, password, rememberMe })
       │
       ├──► TanStack flips isPending = true (UI shows ActivityIndicator)
       │
       ├──► Executes mutationFn (authApi.login)
       │
       ├──► If HTTP 200 OK ────► Runs Hook's onSuccess() ──► Runs Screen's onSuccess()
       │
       └──► If HTTP 4xx/5xx ───► Runs Hook's onError()   ──► Shows Alert.alert()
       │
       ▼
TanStack flips isPending = false
```

---

## 6. Networking, Axios Interceptors & The Localhost Trap
#axios #networking #interceptors #error-handling

### 6.1 The Android Emulator Localhost Trap
On a Mac, your Node.js backend runs on `http://localhost:3001` (`127.0.0.1`).
* **iOS Simulator**: Shares the Mac's network stack, so `http://localhost:3001` works natively.
* **Android Emulator**: Runs inside its own virtual router/firewall. Inside Android, `localhost` (`127.0.0.1`) points to the **virtual phone itself**, not your Mac! Android emulators reserve the special IP **`10.0.2.2`** as a tunnel back to your host computer's `localhost`.
* **Physical Phone over Wi-Fi**: Must use your Mac's local Wi-Fi LAN IP address (e.g., `http://192.168.1.45:3001`).

### 6.2 Axios Instance & Request Interceptor (`src/api/client.ts`)
Instead of repeating URLs and headers in every file, we create a single Axios client and attach a **Request Interceptor**. An interceptor acts like a security checkpoint that inspects every outgoing request before it leaves the phone and injects the JWT Bearer token automatically:

```typescript
import axios from "axios";
import { Platform } from "react-native";
import { useUser } from "../store/useUserStore";

const BASE_URL =
  Platform.OS === "android"
    ? "http://10.0.2.2:3001/api/v1"
    : "http://localhost:3001/api/v1";

export const devFarelabsBackendApi = axios.create({
  baseURL: BASE_URL,
  headers: {
    "Content-Type": "application/json",
  },
});

// Request Interceptor: Automatically attaches JWT token to EVERY request
devFarelabsBackendApi.interceptors.request.use(
  (config) => {
    const token = useUser.getState().token;
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);
```

### 6.3 Deep Error Extraction with `isAxiosError`
When a backend returns an HTTP `401 Unauthorized` or `400 Bad Request` (such as `"Incorrect password"`), Axios immediately throws an exception. 
* The standard JavaScript `error.message` only contains `"Request failed with status code 401"`.
* The actual JSON message sent by your Node.js backend is buried inside **`error.response?.data?.message`**.

```typescript
import { isAxiosError } from "axios";

onError: (error: any) => {
  let errorMessage = "Unable to connect to server";

  // 1. Check if Axios caught an HTTP 4xx/5xx response from the backend
  if (isAxiosError(error)) {
    errorMessage = error.response?.data?.message || error.message;
  } 
  // 2. Fallback if our own code threw a standard Error
  else if (error instanceof Error) {
    errorMessage = error.message;
  }

  Alert.alert("Login Failed", errorMessage);
}
```

---

## 7. JWT Authentication, SecureStore & "Remember Me" Hydration
#authentication #jwt #secure-store #security

### 7.1 Why `expo-secure-store` Instead of `localStorage`?
React Native does not have browser `localStorage` or `cookies`. For sensitive data like JWT tokens, plain `AsyncStorage` is unencrypted. **`expo-secure-store`** encrypts key-value pairs using the phone's hardware security module (**Keychain** on iOS, **Keystore** on Android).

### 7.2 Storage Utility (`src/utils/authStorage.ts`)
```typescript
import * as SecureStore from "expo-secure-store";

const KEYS = {
  TOKEN: "auth_token",
  USER: "auth_user",
  ROLE: "auth_role",
};

export const saveRememberedSession = async (
  token: string,
  user: string,
  role: string
) => {
  try {
    await SecureStore.setItemAsync(KEYS.TOKEN, token);
    await SecureStore.setItemAsync(KEYS.USER, user);
    await SecureStore.setItemAsync(KEYS.ROLE, role);
  } catch (error) {
    console.error("Failed to save session:", error);
  }
};

export const getRememberedSession = async () => {
  try {
    const token = await SecureStore.getItemAsync(KEYS.TOKEN);
    const user = await SecureStore.getItemAsync(KEYS.USER);
    const role = await SecureStore.getItemAsync(KEYS.ROLE);

    if (token && user) {
      return { token, user, role: role || "sampling_executive" };
    }
    return null;
  } catch (error) {
    console.error("Failed to load session:", error);
    return null;
  }
};

export const clearRememberedSession = async () => {
  try {
    await SecureStore.deleteItemAsync(KEYS.TOKEN);
    await SecureStore.deleteItemAsync(KEYS.USER);
    await SecureStore.deleteItemAsync(KEYS.ROLE);
  } catch (error) {
    console.error("Failed to clear session:", error);
  }
};
```

### 7.3 Complete Authentication Lifecycle
1. **Sign In (`useAuth.ts`)**:
   * Sends `{ userName, password }` to `POST /executives/loginExecutive`.
   * Extracts the JWT `token` and saves it in Zustand (`passToken(token)`).
   * If `rememberMe === true`, calls `saveRememberedSession(token, userName, role)`. Otherwise, calls `clearRememberedSession()`.
2. **Session Hydration on App Launch (`Login.tsx`)**:
   * When `Login.tsx` mounts, `checkingSession` starts as `true` (showing a clean loading spinner so the login form doesn't flash).
   * `useEffect` calls `getRememberedSession()`.
   * If a saved token is found in `SecureStore`, it populates Zustand (`passToken`, `passUser`, `passRole`) and immediately calls `navigation.replace("MainScreen")`.
3. **Logout (`TopBar.tsx`)**:
   * User taps their profile initial $\rightarrow$ taps **Logout**.
   * Calls `await clearRememberedSession()` (wipes disk storage).
   * Calls `logoutFromStore()` (wipes Zustand RAM so the Axios interceptor stops sending the token).
   * Calls `navigation.replace("Login")` (locks them out of `MainScreen`).

---

## 8. Native Device Hardware APIs (GPS Location & Camera/Gallery)
#expo-location #expo-image-picker #permissions #hardware

### 8.1 Geolocation Tracking (`expo-location`)
Mobile operating systems strictly protect user privacy. You must always **request runtime permission** before accessing GPS coordinates.

```typescript
import * as Location from "expo-location";

// Inside useLogin's onSuccess callback:
const { status } = await Location.requestForegroundPermissionsAsync();
if (status === "granted") {
  const location = await Location.getCurrentPositionAsync({
    accuracy: Location.Accuracy.High,
  });

  // Sends PATCH /api/v1/executives/updateExecutiveLocation
  // Token is attached automatically by client.ts interceptor!
  await authApi.updateExecutiveLocation({
    latitude: location.coords.latitude,
    longitude: location.coords.longitude,
  });
}
```

### 8.2 Camera & Gallery Uploads (`expo-image-picker` in `PhotoUploader.tsx`)
Our `PhotoUploader` component supports both taking a live photo with the camera and selecting an existing image from the device gallery:

1. **Camera Permission & Launch**:
   ```typescript
   const { status } = await ImagePicker.requestCameraPermissionsAsync();
   const result = await ImagePicker.launchCameraAsync({
     allowsEditing: true,
     quality: 0.8, // Compresses image to 80% quality to save bandwidth
   });
   ```
2. **Gallery Permission & Launch**:
   ```typescript
   const { status } = await ImagePicker.requestMediaLibraryPermissionsAsync();
   const result = await ImagePicker.launchImageLibraryAsync({
     mediaTypes: ["images"],
     allowsEditing: true,
     quality: 0.8,
   });
   ```
3. **Reading the Picked Image URI**:
   If `!result.canceled`, the local file path on the phone is stored at `result.assets[0].uri` (e.g., `file:///data/user/0/.../ImagePicker/xyz.jpg`). We append this URI to our `photos` array state:
   ```typescript
   onChangePhotos([...photos, result.assets[0].uri]);
   ```

> [!info] Bonus: How to Upload `photos` to a Backend API Later (`FormData`)
> When your backend is ready to receive image files from `AssignedDetail` or `DraftDetail`, JSON cannot carry raw binary files. Instead, you wrap the local file URIs inside `FormData`:
> ```typescript
> const formData = new FormData();
> formData.append("ssfNumber", selectedSample.SSFNumber);
> formData.append("remarks", remarks);
> 
> photos.forEach((uri, index) => {
>   const filename = uri.split("/").pop() || `photo_${index}.jpg`;
>   formData.append("photos", {
>     uri,
>     name: filename,
>     type: "image/jpeg",
>   } as any);
> });
> 
> await devFarelabsBackendApi.post("/samples/submit", formData, {
>   headers: { "Content-Type": "multipart/form-data" },
> });
> ```

---

## 9. Enterprise Folder Structure & Developer Cheat Sheet
#folder-structure #cheat-sheet

### 9.1 Final `src/` Architecture Map
```
src/
├── api/
│   ├── client.ts             # Axios instance, Android 10.0.2.2 check, JWT Request Interceptor
│   └── auth.ts               # loginExecutive (POST) & updateExecutiveLocation (PATCH)
├── hooks/
│   └── useAuth.ts            # useLogin() mutation, Zustand sync, SecureStore sync, GPS trigger
├── store/
│   ├── useUserStore.ts       # Global user, role, token, and logout() action
│   └── useSampleStore.ts     # Global selectedSample for Master-Detail screen transitions
├── utils/
│   └── authStorage.ts        # Encrypted SecureStore read/write/delete helpers for Remember Me
├── navigation/
│   ├── TopBar.tsx            # Brand header, profile avatar initial, anchored Logout Modal
│   └── BottomBar.tsx         # Tab switcher (Home, Assigned, Draft)
├── screens/
│   ├── Login.tsx             # Session check on mount, login form, Remember Me checkbox
│   ├── MainScreen.tsx        # Authenticated shell + Android hardware BackHandler
│   ├── HomeScreen.tsx        # Executive dashboard metrics & sample overview
│   ├── Assigned.tsx          # Searchable list of assigned samples
│   ├── AssignedDetail.tsx    # Remarks input, PhotoUploader, Save to Draft popup
│   ├── Draft.tsx             # Searchable list of saved drafts
│   └── DraftDetail.tsx       # Remarks input, PhotoUploader, Update/Submit popup
└── components/
    ├── Button.tsx            # Branded primary button
    ├── Checkbox.tsx          # Controlled boolean checkbox
    ├── DropDownBox.tsx       # Invisible native Picker overlay + snake_case formatter
    ├── PhotoUploader.tsx     # Camera & Gallery image picker with horizontal preview & remove
    ├── PopUp.tsx             # Reusable confirmation modal
    ├── SampleAssignCard.tsx  # Summary card for sample lists
    ├── SampleDetailCard.tsx  # Detailed sample metadata card
    ├── SearchBar.tsx         # Search input box
    ├── StatusBadge.tsx       # Dynamic Pending/Completed/Draft pill
    └── TextInputBox.tsx      # Standardized labeled text/password input
```

### 9.2 Quick Debugging Checklist for React Native
* **Getting `Network Error` on Android Emulator?**
  Check `client.ts` port (`3001` vs `3002`) and ensure Android uses `http://10.0.2.2:<PORT>` while your backend server is actively running.
* **Backend returning `500 Internal Server Error` on Login?**
  Check the backend `.env` file (e.g., `JWT_EXPIRES_IN=7d`) and restart the Node server.
* **Backend returning `401 Unauthorized` on protected routes?**
  Log `useUser.getState().token` inside `client.ts` to verify the token was extracted from the login response (`data?.token || data?.data?.token`).
* **Back button going to the wrong screen?**
  Ensure auth transitions use `navigation.replace()` instead of `navigation.navigate()`, and check the `BackHandler` inside `MainScreen.tsx`.