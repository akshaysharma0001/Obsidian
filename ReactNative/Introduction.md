# React Native Fundamentals (Cheat Sheet & Notes)

## 1. What is React Native?

- A framework to build native iOS and Android apps using JavaScript / TypeScript and React.
    
- It does not render a web page inside a webview; it compiles to real native UI primitives on each operating system.
    

## 2. Core Primitives vs. Web Equivalents

|Web HTML|React Native Component|Purpose|
|---|---|---|
|`<div>`|`<View>`|The basic layout container.|
|`<p>`, `<span>`, `<h1>`|`<Text>`|All text **must** be inside `<Text>`.|
|`<img>`|`<Image>`|Displays local or remote images.|
|`<button>`|`<Pressable>` or `<TouchableOpacity>`|Detects clicks, taps, and interactions.|
|`<div>` (scrollable)|`<ScrollView>`|Simple scrolling container for small content.|
|`<ul>` / `<li>`|`<FlatList>`|High-performance virtualized list for arrays.|
|`<input type="text">`|`<TextInput>`|Accepts keyboard input from the user.|

> **Rule of Thumb:** In React Native, plain text cannot sit directly inside a `<View>`. Putting raw text outside a `<Text>` component causes a crash.

## 3. Core Component Template (TypeScript + Props)

```tsx
import React from 'react';
import { View, Text, StyleSheet } from 'react-native';

// 1. Interface for props
interface UserCardProps {
  name: string;
  age: number;
  role?: string;       // Optional prop
  isEmployed: boolean; // Required boolean
}

// 2. Component with typed props
export const UserCard = ({ name, age, role, isEmployed }: UserCardProps) => {
  return (
    <View style={styles.card}>
      <Text style={styles.name}>{name}</Text>
      <Text style={styles.info}>Age: {age}</Text>
      <Text style={styles.info}>Role: {role ?? 'Unassigned'}</Text>
      <Text style={styles.status}>
        {isEmployed ? 'Employed' : 'Open to work'}
      </Text>
    </View>
  );
};

const styles = StyleSheet.create({
  card: {
    padding: 16,
    borderRadius: 8,
    backgroundColor: '#ffffff',
  },
  name: {
    fontSize: 18,
    fontWeight: 'bold',
  },
  info: {
    fontSize: 14,
    color: '#666666',
  },
  status: {
    fontSize: 12,
    marginTop: 4,
  },
});
```

## 4. State & User Interaction (`useState` + `Pressable`)

```tsx
import React, { useState } from 'react';
import { View, Text, Pressable, StyleSheet } from 'react-native';

export default function Counter() {
  const [count, setCount] = useState<number>(0);

  return (
    <View style={styles.container}>
      <Text style={styles.countText}>Count: {count}</Text>

      <Pressable 
        style={styles.button} 
        onPress={() => setCount(count + 1)}
      >
        <Text style={styles.buttonText}>Increment</Text>
      </Pressable>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  countText: {
    fontSize: 24,
    marginBottom: 12,
  },
  button: {
    backgroundColor: '#0284c7',
    paddingHorizontal: 20,
    paddingVertical: 10,
    borderRadius: 6,
  },
  buttonText: {
    color: '#ffffff',
    fontWeight: '600',
  },
});
```

## 5. Handling User Input (`TextInput`)

```tsx
import React, { useState } from 'react';
import { View, TextInput, Text, StyleSheet } from 'react-native';

export function InputExample() {
  const [text, setText] = useState<string>('');

  return (
    <View style={styles.wrapper}>
      <TextInput
        value={text}
        onChangeText={(val) => setText(val)} // Receives string directly, not an event object
        placeholder="Type your name..."
        style={styles.input}
      />
      <Text>You typed: {text}</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  wrapper: {
    padding: 20,
  },
  input: {
    borderWidth: 1,
    borderColor: '#cbd5e1',
    padding: 10,
    borderRadius: 6,
    marginBottom: 8,
  },
});
```

## 6. Rendering Lists with `FlatList`

Never use `.map()` inside a `<ScrollView>` for long lists because it renders every item in memory simultaneously. Use `<FlatList>` instead:

```tsx
import React from 'react';
import { FlatList, Text, View, StyleSheet } from 'react-native';

interface Item {
  id: string;
  title: string;
}

const DATA: Item[] = [
  { id: '1', title: 'Task 1' },
  { id: '2', title: 'Task 2' },
  { id: '3', title: 'Task 3' },
];

export function ListExample() {
  return (
    <FlatList
      data={DATA}
      keyExtractor={(item) => item.id}
      renderItem={({ item }) => (
        <View style={styles.row}>
          <Text style={styles.text}>{item.title}</Text>
        </View>
      )}
    />
  );
}

const styles = StyleSheet.create({
  row: {
    padding: 16,
    borderBottomWidth: 1,
    borderBottomColor: '#e2e8f0',
  },
  text: {
    fontSize: 16,
  },
});
```

## 7. Flexbox Layout Quick Rules

- **Default Direction:** `flexDirection: 'column'` (stacks top-to-bottom, opposite of CSS web).
    
- **Main Axis Alignment:** `justifyContent` (`center`, `flex-start`, `flex-end`, `space-between`, `space-around`).
    
- **Cross Axis Alignment:** `alignItems` (`center`, `flex-start`, `flex-end`, `stretch`).
    
- **Fill Screen:** `flex: 1` tells a component to expand and fill all remaining space in its parent.