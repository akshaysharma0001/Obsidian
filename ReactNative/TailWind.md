Here is a clean Obsidian markdown note you can copy and paste directly:

# Setting Up Tailwind CSS (NativeWind) in React Native (Expo)

### 1. Installation

Install the required packages in your Expo project terminal:

```bash
npx expo install nativewind react-native-reanimated
npm install --save-dev tailwindcss
```

### 2. File Setup & Configurations

#### A. `tailwind.config.js`

Create or update this file in the project root:

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./App.{js,jsx,ts,tsx}",
    "./components/**/*.{js,jsx,ts,tsx}",
    "./app/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

#### B. `babel.config.js`

> **Important:** Ensure the filename is spelled correctly as `babel.config.js` (not `bable.config.js`).

```javascript
module.exports = function (api) {
  api.cache(true);
  return {
    presets: ["babel-preset-expo"],
    plugins: ["nativewind/babel"],
  };
};
```

#### C. `nativewind-env.d.ts`

Create this file in the project root to satisfy TypeScript for `className` props:

```typescript
/// <reference types="nativewind/types" />
```

### 3. Clear Cache & Start Dev Server

Always restart Metro with a clear cache whenever editing Babel or Tailwind configuration files:

```bash
npx expo start -c
```

### 4. Common Troubleshooting Notes

- **Typo Alert:** Watch out for misspellings like `iteams-center`; the valid Tailwind class is `items-center`.
    
- **Babel Filename:** If styles still do not apply, verify that the configuration file is named `babel.config.js` rather than `bable.config.js`.