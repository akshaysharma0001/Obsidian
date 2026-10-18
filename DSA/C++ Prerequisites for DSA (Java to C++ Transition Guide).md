```yaml
---
title: "C++ Prerequisites for DSA (Java to C++ Transition Guide)"
tags:
  - dsa
  - cpp
  - stl
  - java-to-cpp
  - programming-notes
aliases:
  - "CPP Basics for DSA"
  - "C++ for Java Developers"
status: complete
created: 2026-10-16
---
```

# C++ Prerequisites for DSA (Java to C++ Transition Guide)

> [!abstract] Vault Navigation & Purpose This note bridges the gap between **Java** and **C++** specifically for **Data Structures and Algorithms (DSA)** and Competitive Programming. It skips legacy C fluff and focuses on modern C++ (C++17/C++20), memory semantics, common traps for Java developers, and the **Standard Template Library (STL)**.
> 
> - **Next Notes:** [[Arrays & Hashing]], [[Two Pointers]], [[Linked Lists]], [[Trees & Graphs]]
>     

## 1. The Paradigm Shift: Java vs. C++ Mental Model

Moving from Java to C++ changes how your code compiles, manages memory, and passes data between functions.

|Feature|Java|C++|DSA Impact|
|---|---|---|---|
|**Compilation**|Compiles to JVM Bytecode (`javac`)|Compiles directly to native machine code (`g++` / `clang++`)|Faster execution speeds; zero warmup or JIT overhead.|
|**Memory Management**|Automatic Garbage Collector (GC)|Deterministic stack allocation + manual heap (`new` / `delete`)|Predictable runtime without GC pauses; you control stack vs. heap.|
|**Program Structure**|Everything must live inside a `class`|Supports standalone functions (`int main()`) and global variables|Less boilerplate; helper functions don't need `public static`.|
|**Parameter Passing**|Primitives by value; Objects by reference copy|**Everything is passed by value (deep copy) by default**|**Critical Trap:** Forgetting `&` in C++ copies entire containers in O(N) time.|
|**Strings**|Immutable (`String`); requires `StringBuilder` to mutate|**Mutable** (`std::string`); modifies in-place in O(1)|Direct index manipulation and in-place string algorithms.|
|**Collections / Containers**|Java Collections Framework (only holds Objects/Wrappers)|Standard Template Library (STL) (holds primitives directly)|No autoboxing overhead (`int` vs `Integer`).|

## 2. Program Boilerplate, Fast I/O & Data Types

### Standard DSA Template

In Java, you import packages with `import java.util.*;`. In C++, you include header files using preprocessor directives.

> [!tip] Compiler Note (GCC vs. Clang on macOS/Linux) In competitive programming, `#include <bits/stdc++.h>` imports the entire standard library at once. It comes built-in with **GCC (`g++`)** on Linux, Windows (MinGW), and platforms like LeetCode/Codeforces. On **macOS**, default `clang++` does not ship with `bits/stdc++.h` unless you install GCC via Homebrew or manually place the header file in your include path. For technical interviews, prefer including explicit headers (`iostream`, `vector`, `string`, `algorithm`, `unordered_map`).

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
#include <unordered_map>
#include <unordered_set>
#include <queue>
#include <stack>
#include <climits>

// Brings std::cout, std::vector, etc. into the global scope
// Avoid in large production header files, but standard for DSA
using namespace std;

int main() {
    // Fast I/O: Decouples C and C++ standard streams (speeds up cin/cout to match BufferedReader)
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n; // Reads whitespace-separated input (like Scanner.nextInt())
    cout << "Input was: " << n << "\n"; // Prefer "\n" over endl (endl forces a slow buffer flush)

    return 0;
}
```

### Primitive Data Types & Overflow Rules

Java enforces fixed sizes across all platforms. C++ guarantees minimum sizes, though on modern 64-bit systems the sizes below are standard:

```cpp
// 1. Integers
int a = 10;                  // 32-bit signed: ~ -2 * 10^9 to 2 * 10^9 ( approx 2.14 * 10^9 )
long long b = 10000000000LL; // 64-bit signed: ~ -9 * 10^18 to 9 * 10^18 (Java's 'long')

// 2. Constants (Requires <climits>)
int max_int = INT_MAX;       // Equivalent to Integer.MAX_VALUE
int min_int = INT_MIN;       // Equivalent to Integer.MIN_VALUE
long long max_ll = LLONG_MAX;// Equivalent to Long.MAX_VALUE

// 3. Booleans & Characters
bool flag = true;            // Note: 'bool' in C++, NOT 'boolean'
char ch = 'A';               // 1 byte ASCII (Java char is 2-byte UTF-16)

// 4. Floating Point
double pi = 3.141592653589793; // 64-bit float (prefer double over float for precision)

// 5. Type Inference (auto)
auto count = 42;             // Deduced as int (similar to Java's 'var', but works everywhere)
```

> [!warning] Integer Multiplication Overflow Trap When multiplying two `int` variables whose product exceeds 2 \times 10^9, C++ computes the multiplication in 32-bit arithmetic **before** assigning it to a `long long`. Cast one operand or multiply by `1LL`:
> 
> ```cpp
> int x = 100000, y = 100000;
> long long bad = x * y;         // OVERFLOW! Computed as 32-bit int first
> long long good = 1LL * x * y;  // SAFE: 1LL promotes the expression to 64-bit long long
> ```

## 3. Memory Model: References, Pointers & Value Semantics

This is the single most important section when switching from Java to C++.

### Pass-by-Value vs. Pass-by-Reference (`&`)

In Java, passing an `ArrayList` or array to a function copies the _reference_ in O(1) time. In C++, **containers are passed by value (deep-copied) by default**.

> [!danger] The #1 Cause of Time Limit Exceeded (TLE) for Java Switchers If you write a recursive binary search or DFS in C++ and pass `vector<int> arr` without an ampersand (`&`), C++ allocates and copies the entire vector on **every single function call**, turning an O(N) or O(\log N) algorithm into O(N^2)!

```cpp
#include <vector>
#include <iostream>
using namespace std;

// 1. BAD FOR CONTAINERS: Pass-by-Value (Creates a full deep copy in O(N) time and space)
void modifyCopy(vector<int> nums) {
    nums[0] = 999; // Only modifies the local copy; caller's vector is unchanged
}

// 2. GOOD WHEN MUTATING: Pass-by-Reference (Zero copy, O(1) time, mutates original)
void modifyOriginal(vector<int>& nums) {
    nums[0] = 999; // Modifies caller's vector directly (just like Java)
}

// 3. BEST FOR READ-ONLY: Pass-by-Const-Reference (Zero copy, O(1) time, compiler prevents mutation)
int sumElements(const vector<int>& nums) {
    // nums[0] = 999; // Compile Error! Safe read-only access
    int sum = 0;
    for (int x : nums) sum += x;
    return sum;
}
```

### Pointers (`*`) vs. References (`&`)

- **Reference (`&`)**: An _alias_ for an existing variable. Cannot be `nullptr`, and cannot be reassigned to refer to a different variable after initialization. Used heavily in function parameters and range-based `for` loops.
    
- **Pointer (`*`)**: A variable that stores a **memory address**. Can be `nullptr` (Java's `null`) and can be reassigned to point to different memory locations. Used for Linked Lists, Trees, Tries, and Graph nodes.
    

```cpp
int x = 42;

// REFERENCE: 'ref' is just another name for 'x'
int& ref = x;
ref = 100; // x is now 100

// POINTER: 'ptr' holds the memory address of 'x'
int* ptr = &x;       // '&' here is the Address-Of operator
cout << ptr << "\n"; // Prints hex memory address (e.g., 0x7ffee4b...)
cout << *ptr << "\n";// '*' here is the Dereference operator: prints 100

*ptr = 200;          // Changes the value at that memory address; x is now 200
ptr = nullptr;       // Points to nothing (equivalent to Java's null)
```

### Stack vs. Heap Allocation (Dot `.` vs. Arrow `->`)

In Java, all objects live on the heap (`Node n = new Node()`), and you access members with `.`. In C++, you can create objects on the **Stack** (automatic cleanup when out of scope) or on the **Heap** using `new` (returns a pointer):

```cpp
struct Node {
    int val;
    Node* next;
    Node(int v) : val(v), next(nullptr) {} // Constructor with initializer list
};

void memoryDemo() {
    // 1. Stack Allocation (Automatically destroyed when memoryDemo() finishes)
    Node stackNode(10);
    cout << stackNode.val << "\n"; // Use DOT (.) operator for stack objects/references

    // 2. Heap Allocation (Returns a pointer; used for Linked Lists & Trees)
    Node* heapNode = new Node(20);
    cout << heapNode->val << "\n"; // Use ARROW (->) operator for pointers!
    // Note: heapNode->val is syntactic sugar for (*heapNode).val

    delete heapNode; // Manual deallocation (frees heap memory)
}
```

## 4. Arrays and Strings in C++

### Static Arrays vs. Dynamic Vectors

Raw C-style arrays (`int arr[100]`) decay into pointers when passed to functions and do not know their own `.length` like Java's `arr.length`. **Always prefer `std::vector` for DSA** unless working with fixed-size DP tables.

```cpp
#include <cstring> // Required for memset

// Fixed-size array initialization tricks
int arr[5] = {0}; // Initializes all 5 elements to 0

// Fast initialization of raw arrays for Memoization / DP tables
int dp[1005][1005];
memset(dp, -1, sizeof(dp)); // Fills all bytes with 0xFF (which equals -1 for two's complement int)
// WARNING: memset works byte-by-byte! Only use 0 or -1 with memset on int arrays.
```

### Strings (`std::string`)

Unlike Java where `String` is immutable and requires `.charAt(i)` and `StringBuilder`, C++ `std::string` behaves like a `vector<char>`—it is **mutable** and supports `[]` indexing in O(1) time!

```cpp
#include <string>
#include <algorithm>
using namespace std;

void stringOperations() {
    string s = "hello";

    // 1. Indexing & In-place Mutation (No charAt() or StringBuilder needed!)
    char c = s[0];   // 'h'
    s[0] = 'H';      // s becomes "Hello"

    // 2. Appending (Amortized O(1) at the back)
    s.push_back('!');// "Hello!"
    s += " World";   // "Hello! World"

    // 3. Length
    int n = s.size(); // or s.length() — both are O(1)

    // 4. Substring TRAP!
    // Java: s.substring(startIndex, endIndex)
    // C++ : s.substr(startIndex, LENGTH)
    string sub = s.substr(0, 5); // Starts at index 0, takes 5 characters -> "Hello"
    string tail = s.substr(7);   // From index 7 to the end -> "World"

    // 5. Searching
    size_t pos = s.find("World");
    if (pos != string::npos) {   // string::npos means "not found" (Java returns -1)
        // Found at index 'pos'
    }

    // 6. Reversing in-place (Requires <algorithm>)
    reverse(s.begin(), s.end());

    // 7. String <-> Number Conversions
    int num = stoi("12345");          // Integer.parseInt()
    long long big = stoll("9876543210"); // Long.parseLong()
    string strNum = to_string(42);    // String.valueOf()

    // 8. Reading a full line with spaces (like Scanner.nextLine())
    string fullLine;
    getline(cin, fullLine);
}
```

## 5. Structs, Classes & OOP for DSA

In C++, `struct` and `class` are identical except for their **default access modifier**:

- `struct`: Members are `**public**` by default (used for `ListNode`, `TreeNode`, `TrieNode`, `DSU`).
    
- `class`: Members are `**private**` by default (used for encapsulating full solutions).
    

```cpp
// Standard LeetCode Binary Tree Node in C++
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;

    // Member Initializer List: cleaner and faster than assigning inside {}
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};
```

## 6. The Standard Template Library (STL) Masterclass

The STL is your biggest superpower in C++ DSA. It consists of **Containers**, **Iterators**, and **Algorithms**.

### Java Collections to C++ STL Cheat Sheet

|Data Structure|Java Collection|C++ STL Container|Header|Time Complexity (Insert / Lookup)|
|---|---|---|---|---|
|**Dynamic Array**|`ArrayList`|`std::vector`|`vector`|O(1) amortized push / O(1) index|
|**Pair / 2-Tuple**|Custom Class / `Map.Entry`|`std::pair`|`utility`|O(1) access via `.first` and `.second`|
|**Stack (LIFO)**|`Stack` / `ArrayDeque`|`std::stack`|`stack`|O(1) push, pop, top|
|**Queue (FIFO)**|`Queue` (`LinkedList` / `ArrayDeque`)|`std::queue`|`queue`|O(1) push, pop, front|
|**Double-Ended Queue**|`ArrayDeque`|`std::deque`|`deque`|O(1) front/back push & pop, O(1) random access|
|**Priority Queue (Heap)**|`PriorityQueue` (**Min-Heap** default)|`std::priority_queue` (**Max-Heap** default)|`queue`|O(\log N) push/pop, O(1) top|
|**Hash Set**|`HashSet`|`std::unordered_set`|`unordered_set`|O(1) average, O(N) worst-case|
|**Ordered Set (BST)**|`TreeSet`|`std::set`|`set`|O(\log N) (Red-Black Tree)|
|**Multi-Set (Duplicates)**|_No direct equivalent_|`std::multiset`|`set`|O(\log N) (keeps duplicates sorted)|
|**Hash Map**|`HashMap`|`std::unordered_map`|`unordered_map`|O(1) average, O(N) worst-case|
|**Ordered Map (BST)**|`TreeMap`|`std::map`|`map`|O(\log N) (Keys sorted automatically)|

### 6.1 `std::vector` (Replacing Java's `ArrayList` and `int[]`)

```cpp
#include <vector>
using namespace std;

void vectorDemo() {
    // Declarations
    vector<int> v;                       // Empty vector: {}
    vector<int> v_sized(5);              // Size 5, all initialized to 0: {0, 0, 0, 0, 0}
    vector<int> v_filled(5, -1);         // Size 5, all initialized to -1: {-1, -1, -1, -1, -1}
    vector<int> v_init = {10, 20, 30};   // Initializer list

    // 2D Vector (Grid / Matrix of size R x C initialized to 0)
    int R = 3, C = 4;
    vector<vector<int>> grid(R, vector<int>(C, 0));

    // Core Operations
    v.push_back(42);       // Appends to end in O(1) amortized (like list.add(42))
    v.pop_back();          // Removes last element in O(1) (returns void!)
    int first = v_init[0]; // Direct O(1) indexing (no .get(0) needed!)
    int last = v_init.back();   // Returns last element (equivalent to v[v.size() - 1])
    int len = v_init.size();    // Number of elements
    bool is_empty = v.empty();  // true if size == 0
    v.clear();                  // Removes all elements
}
```

### 6.2 `std::pair` and `std::tuple`

In Java, returning two values or storing `(row, col)` coordinates in a BFS queue often requires creating a custom `Pair` class or using a messy `int[]{r, c}`. C++ has `std::pair` built right in, and pairs automatically compare lexicographically (first by `.first`, then by `.second`)!

```cpp
#include <utility>
#include <vector>
using namespace std;

void pairDemo() {
    pair<int, int> coords = {2, 5}; // Or make_pair(2, 5)
    int r = coords.first;           // 2
    int c = coords.second;          // 5

    // C++17 Structured Binding (Unpacking! Super clean for BFS / Graphs)
    auto [row, col] = coords;

    // Vector of pairs (e.g., Weighted Graph Adjacency List: node -> {neighbor, weight})
    vector<pair<int, int>> adj[100];
    adj[0].push_back({1, 15});
    adj[0].emplace_back(2, 30); // emplace_back constructs the pair in-place without braces
}
```

### 6.3 `std::stack`, `std::queue`, and `std::deque`

> [!warning] Crucial C++ STL Trap: `.pop()` Returns `void`! In Java, `int x = stack.pop()` or `queue.poll()` removes **and** returns the element. In C++, `.pop()` **only removes** the element and returns `void` (because of exception-safety guarantees). You must inspect the element using `.top()` or `.front()` **before** calling `.pop()`.

```cpp
#include <stack>
#include <queue>
#include <deque>
using namespace std;

void adaptersDemo() {
    // 1. STACK (LIFO)
    stack<int> st;
    st.push(10);
    st.push(20);
    int topEl = st.top(); // 20 (Java's stack.peek())
    st.pop();             // Removes 20 (returns void!)

    // 2. QUEUE (FIFO)
    queue<int> q;
    q.push(1);            // Java's q.offer(1)
    q.push(2);
    int frontEl = q.front(); // 1 (Java's q.peek())
    int backEl = q.back();   // 2
    q.pop();              // Removes 1 from the front (Java's q.poll())

    // 3. DEQUE (Double-Ended Queue - Great for Sliding Window Maximum)
    deque<int> dq;
    dq.push_back(10);     // Add to back
    dq.push_front(5);     // Add to front
    dq.pop_back();        // Remove from back
    dq.pop_front();       // Remove from front
    // Bonus: Unlike stack/queue, std::deque supports O(1) random access via dq[i]!
}
```

### 6.4 `std::priority_queue` (Heaps)

> [!danger] Max-Heap vs. Min-Heap Default Reversal!
> 
> - **Java's** `PriorityQueue` is a **Min-Heap** by default (smallest element at the top).
>     
> - **C++'s** `std::priority_queue` is a **Max-Heap** by default (largest element at the top).
>     

```cpp
#include <queue>
#include <vector>
using namespace std;

void heapDemo() {
    // 1. MAX-HEAP (Default in C++)
    priority_queue<int> maxHeap;
    maxHeap.push(10);
    maxHeap.push(30);
    maxHeap.push(20);
    int largest = maxHeap.top(); // 30
    maxHeap.pop();

    // 2. MIN-HEAP (Memorize this 3-argument template syntax!)
    // Template args: <Type, UnderlyingContainer, Comparator>
    priority_queue<int, vector<int>, greater<int>> minHeap;
    minHeap.push(10);
    minHeap.push(30);
    minHeap.push(20);
    int smallest = minHeap.top(); // 10
    minHeap.pop();

    // 3. Min-Heap of Pairs (Used constantly in Dijkstra's Algorithm: {distance, node})
    using PII = pair<int, int>;
    priority_queue<PII, vector<PII>, greater<PII>> dijkstraPq;
    dijkstraPq.push({0, 1}); // Sorts automatically by pair.first (distance), then pair.second
}
```

### 6.5 Maps & Sets (`unordered_` vs. Ordered)

Choose between Hash-based (O(1)) and Tree-based (O(\log N)) containers depending on whether you need sorted order:

```cpp
#include <unordered_map>
#include <map>
#include <unordered_set>
#include <set>
using namespace std;

void mapAndSetDemo() {
    unordered_map<string, int> freq;

    // 1. Inserting & Updating Frequency (Much cleaner than Java's getOrDefault!)
    // If key doesn't exist, operator[] automatically initializes int value to 0!
    freq["apple"]++; // freq["apple"] is created as 0, then incremented to 1

    // 2. TRAP: Checking if a key exists!
    // DO NOT do: if (freq["banana"] == 0) -> This silently inserts "banana" with value 0 into the map!
    // Instead, use .count() or .find():
    if (freq.count("banana")) { // or freq.find("banana") != freq.end()
        // Key exists without mutating the map
    }

    // 3. Iterating over a Map (using C++17 Structured Bindings)
    for (const auto& [key, val] : freq) {
        // key is string, val is int
    }

    // 4. Erasing
    freq.erase("apple"); // Removes key in O(1) average

    // 5. Ordered Set & Multiset (Red-Black Tree, O(log N))
    set<int> s = {50, 10, 30, 10}; // Stores unique sorted elements: {10, 30, 50}
    multiset<int> ms = {50, 10, 10}; // Keeps duplicates sorted: {10, 10, 50}

    // Multiset Erase Trap:
    ms.erase(10);          // WARNING: Erases ALL instances of 10!
    // To erase only ONE instance of 10 in a multiset, pass an iterator:
    auto it = ms.find(10);
    if (it != ms.end()) ms.erase(it);
}
```

> [!info] Can't Hash a `std::pair` in `unordered_set` or `unordered_map`? Unlike `std::set` and `std::map` (which work out-of-the-box with `pair` because `pair` defines `<`), `unordered_set` and `unordered_map` do **not** have a default hash function for `std::pair`. Either use `std::set` (O(\log N)), encode coordinates into a single `long long` key, or provide a custom hash struct.

## 7. Iterators, Essential STL Algorithms & Custom Comparators

### Understanding Iterators

An **iterator** is an object that points to an element inside an STL container (acting like a generalized pointer).

- `v.begin()`: Points to the **first** element.
    
- `v.end()`: Points to the **theoretical position one past the last element** (half-open range `[begin, end)`).
    
- `*it`: Dereferences the iterator to get the actual value.
    
- `it - v.begin()`: Gives the integer **index** of a random-access iterator (`vector`, `string`, `deque`).
    

### Essential STL Algorithms (`#include <algorithm>` & `<numeric>`)

```cpp
#include <algorithm>
#include <numeric>
#include <vector>
using namespace std;

void algorithmsDemo() {
    vector<int> nums = {4, 1, 8, 3, 8, 2};

    // 1. Sorting: O(N log N) Introsort
    sort(nums.begin(), nums.end());                 // Ascending: {1, 2, 3, 4, 8, 8}
    sort(nums.begin(), nums.end(), greater<int>()); // Descending: {8, 8, 4, 3, 2, 1}

    // 2. Reversing: O(N)
    reverse(nums.begin(), nums.end());              // Back to ascending: {1, 2, 3, 4, 8, 8}

    // 3. Min / Max Element: O(N) — Returns an ITERATOR, so dereference with *
    int maxVal = *max_element(nums.begin(), nums.end()); // 8
    int minVal = *min_element(nums.begin(), nums.end()); // 1

    // 4. Summing a container: O(N) (Requires <numeric>)
    // Use 0LL as the initial value so the sum is accumulated as a 64-bit long long!
    long long total = accumulate(nums.begin(), nums.end(), 0LL);

    // 5. Binary Search Powerhouses (Container MUST be sorted first!): O(log N)
    // lower_bound: First element >= target
    auto it1 = lower_bound(nums.begin(), nums.end(), 4);
    int idx1 = it1 - nums.begin(); // Index 3 (points to 4)

    // upper_bound: First element STRICTLY > target
    auto it2 = upper_bound(nums.begin(), nums.end(), 4);
    int idx2 = it2 - nums.begin(); // Index 4 (points to first 8)

    //Note: For std::set and std::map, ALWAYS call s.lower_bound(x) as a member method (O(log N)),
    //NOT std::lower_bound(s.begin(), s.end(), x) which takes O(N) on non-random-access iterators!

    // 6. Bit Manipulation Built-ins (GCC / Clang intrinsics & C++20 <bit>)
    int x = 29; // Binary: 11101
    int setBits = __builtin_popcount(x);     // 4 (Use __builtin_popcountll(x) for long long)
    int leadingZeros = __builtin_clz(x);     // Counts leading zeros (32-bit)
    int trailingZeros = __builtin_ctz(x);    // Counts trailing zeros
}
```

### Custom Comparators & Lambda Functions

In Java, you sort custom objects with `(a, b) -> Integer.compare(a[1], b[1])`. In C++, you pass a **Lambda Function** `[capture](parameters) { body }` returning a `bool` that answers: **"Should `a` come strictly before `b`?"**

> [!danger] Strict Weak Ordering Rule Never use `<=` or `>=` in a C++ sort comparator! Your comparator **must** return `false` when `a` and `b` are equal (use `<` or `>`). Returning `true` on equal elements violates _strict weak ordering_ and causes `std::sort` to crash with a Segmentation Fault (`SIGSEGV`) at runtime.

```cpp
#include <vector>
#include <algorithm>
using namespace std;

void customSortDemo() {
    vector<vector<int>> intervals = {{1, 4}, {2, 3}, {1, 2}};

    // Sort intervals by:
    // 1. End time ascending (a[1] < b[1])
    // 2. If end times are equal, start time descending (a[0] > b[0])
    sort(intervals.begin(), intervals.end(), [](const vector<int>& a, const vector<int>& b) {
        if (a[1] != b[1]) {
            return a[1] < b[1]; // Smaller end time comes first
        }
        return a[0] > b[0];     // Larger start time comes first on ties
    });
}
```

### Recursive Lambdas Inside `Solution` Classes

When writing DFS or Backtracking inside a LeetCode `Solution` class method, you can avoid passing 6 state variables to a separate helper method by using a lambda that captures local variables by reference (`[&]`):

```cpp
class Solution {
public:
    int countComponents(int n, vector<vector<int>>& adj) {
        vector<bool> visited(n, false);

        // 'auto&& self' allows the lambda to call itself recursively without std::function overhead
        auto dfs = [&](auto&& self, int u) -> void {
            visited[u] = true;
            for (int v : adj[u]) {
                if (!visited[v]) {
                    self(self, v);
                }
            }
        };

        int components = 0;
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                components++;
                dfs(dfs, i);
            }
        }
        return components;
    }
};
```

## 8. Pre-DSA Readiness Checklist


- [ ] Understand why passing `vector<int>&` by reference prevents $O(N)$ copying and TLE.
- [ ] Know the difference between stack objects (`.`) and heap pointers (`->`).
- [ ] Remember that `s.substr(pos, len)` takes **length** as the second argument, not the end index.
- [ ] Remember that `stack.pop()`, `queue.pop()`, and `priority_queue.pop()` return `void`.
- [ ] Know how to declare a Min-Heap using `priority_queue<int, vector<int>, greater<int>>`.
- [ ] Avoid mutating maps accidentally via `mp[key]` when only checking key existence (use `mp.count(key)`).
- [ ] Write custom sort lambdas using strict inequality (`<` or `>`) and `const auto&` parameters.


For a complete visual walkthrough of every STL container and algorithm alongside its time complexity before diving into problem sets, check out [Striver's Complete C++ STL in 1 Video](https://www.youtube.com/watch?v=RRVYpIET_RU).
