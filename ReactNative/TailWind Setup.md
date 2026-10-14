Your app is now bundling cleanly without errors (`Android Bundled 2576ms index.ts`)!

Here is the complete Markdown note to copy and paste into Obsidian, covering both the modern **NativeWind v4 setup** and the **most important Tailwind utilities** for mobile development.

# NativeWind v4 (Tailwind CSS) in React Native

## Part 1: Complete Setup Guide (Expo + NativeWind v4)

### 1. Install Dependencies

```bash
npm install nativewind@latest tailwindcss@^3.4.0 react-native-reanimated react-native-safe-area-context
```

### 2. Configure `metro.config.js`

Create this in the project root:

```javascript
const { getDefaultConfig } = require("expo/metro-config");
const { withNativeWind } = require("nativewind/metro");

const config = getDefaultConfig(__dirname);

module.exports = withNativeWind(config, { input: "./global.css" });
```

### 3. Create `global.css`

Create this in the project root:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### 4. Configure `tailwind.config.js`

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./App.{js,jsx,ts,tsx}", "./components/**/*.{js,jsx,ts,tsx}"],
  presets: [require("nativewind/preset")],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

### 5. Configure `babel.config.js`

```javascript
module.exports = function (api) {
  api.cache(true);
  return {
    presets: [
      ["babel-preset-expo", { jsxImportSource: "nativewind" }],
      "nativewind/babel",
    ],
  };
};
```

### 6. TypeScript Support (`nativewind-env.d.ts`)

Create this in the project root to enable autocomplete and remove TypeScript squiggly warnings under `className`:

```typescript
/// <reference types="nativewind/types" />
```

### 7. Import CSS in Entry File (`App.tsx`)

At the very top of `App.tsx`:

```tsx
import "./global.css";
```

### 8. Run with Cleared Cache

```bash
npx expo start -c
```

## Part 2: Tailwind Basics for React Native

### 1. Spacing (Margin & Padding)

In Tailwind, spacing numbers map to multiples of `4px` (`p-4` = `16px`).

|Class|Equivalent RN Style|What it does|
|---|---|---|
|`p-4`|`padding: 16`|Padding on all 4 sides|
|`px-4`|`paddingHorizontal: 16`|Left and right padding|
|`py-2`|`paddingVertical: 8`|Top and bottom padding|
|`m-3`|`margin: 12`|Margin on all 4 sides|
|`mb-4`|`marginBottom: 16`|Space below an element|
|`gap-3`|`gap: 12`|Space between child elements|

### 2. Layout & Flexbox

> **Golden Rule:** On web, `flex` defaults to row. In React Native, `flex` defaults to column!

| Class             | React Native Effect                                  |
| ----------------- | ---------------------------------------------------- |
| `flex-1`          | Expands component to fill all remaining screen space |
| `flex-row`        | Puts child elements side-by-side horizontally        |
| `flex-col`        | Stacks child elements vertically (default)           |
| `items-center`    | Centers children horizontally (in column mode)       |
| `justify-center`  | Centers children vertically (in column mode)         |
| `justify-between` | Pushes items to opposite edges                       |

### 3. Backgrounds & Colors

Tailwind shades range from `50` (lightest) to `950` (darkest).

- `bg-white` \rightarrow White background
    
- `bg-slate-100` \rightarrow Very light gray/slate background (great for screen bases)
    
- `bg-blue-600` \rightarrow Primary brand blue
    
- `text-slate-900` \rightarrow Dark, high-contrast text color
    
- `text-slate-500` \rightarrow Subdued/secondary text color
    

### 4. Text & Typography

Apply these directly to `<Text>` components, not `<View>`.

- **Font Size:** `text-xs` (12px), `text-sm` (14px), `text-base` (16px), `text-lg` (18px), `text-xl` (20px), `text-2xl` (24px)
    
- **Font Weight:** `font-normal`, `font-medium`, `font-semibold`, `font-bold`
    
- **Alignment:** `text-center`, `text-left`, `text-right`
    

### 5. Borders & Corners

- `rounded-md` \rightarrow Slight rounding (`borderRadius: 6`)
    
- `rounded-xl` \rightarrow Modern card rounding (`borderRadius: 12`)
    
- `rounded-full` \rightarrow Circles/pill buttons (`borderRadius: 9999`)
    
- `border border-slate-200` \rightarrow 1px subtle light border
    

## Part 3: Cheat Sheet — Building a Modern Card Component

```tsx
import React from 'react';
import { View, Text, Pressable } from 'react-native';

interface CardProps {
  title: string;
  subtitle: string;
  badgeText: string;
}

export function ActionCard({ title, subtitle, badgeText }: CardProps) {
  return (
    // Outer Card Container
    <View className="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm w-full max-w-sm">
      
      {/* Top Header Row */}
      <View className="flex-row items-center justify-between mb-2">
        <Text className="text-lg font-bold text-slate-900">{title}</Text>
        <View className="bg-emerald-50 px-2.5 py-1 rounded-full">
          <Text className="text-xs font-semibold text-emerald-700">{badgeText}</Text>
        </View>
      </View>

      {/* Body Text */}
      <Text className="text-sm text-slate-500 mb-4">{subtitle}</Text>

      {/* Button */}
      <Pressable className="bg-blue-600 active:bg-blue-700 py-2.5 rounded-xl items-center">
        <Text className="text-white font-medium text-sm">View Details</Text>
      </Pressable>
    </View>
  );
}
```