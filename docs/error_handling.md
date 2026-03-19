# Error Handling: HashMaps & Dictionaries

## Overview

Handling missing keys and runtime errors is crucial when working with hash maps. Different languages provide different mechanisms for managing these scenarios.

---

## 🔄 Access Pattern Flow

```
┌─────────────────────────────────────────────────────────────────────┐
│                      Key Access Flow Diagram                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│    ┌──────────────┐                                                 │
│    │  Access Key  │                                                 │
│    └──────┬───────┘                                                 │
│           │                                                          │
│           ▼                                                          │
│    ┌──────────────┐                                                 │
│    │  Key Exists? │                                                 │
│    └──────┬───────┘                                                 │
│           │                                                          │
│      ┌────┴────┐                                                     │
│      │         │                                                     │
│     Yes        No                                                    │
│      │         │                                                     │
│      ▼         ▼                                                     │
│ ┌─────────┐  ┌──────────────┐                                        │
│ │ Return  │  │ Handle Error │                                        │
│ │ Value   │  │ Appropriately│                                        │
│ └─────────┘  └──────────────┘                                        │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🐞 C++ Error Handling
<span style="color: #888; font-size: 0.9em;">code by <a href="https://github.com/mwakidenis" target="_blank">mwakidenis</a></span>

### Approaches to Safe Access

#### 1. Using `at()` Method
```
┌─────────────────────────────────────────────────────────────┐
│              C++ at() Method Behavior                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   myMap.at(key)                                            │
│                                                             │
│   ┌─────────────┐                                          │
│   │  Key Exists │  ───▶  Returns value                     │
│   └─────────────┘                                          │
│                                                             │
│   ┌─────────────┐                                          │
│   │  Key Missing│  ───▶  Throws std::out_of_range         │
│   └─────────────┘                                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### 2. Using `find()` Method (Recommended)
```
┌─────────────────────────────────────────────────────────────┐
│              C++ find() Method Pattern                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   auto it = myMap.find(key);                               │
│                                                             │
│   if (it != myMap.end()) {                                 │
│       // Key exists - use it->second                      │
│   } else {                                                 │
│       // Key doesn't exist - handle gracefully             │
│   }                                                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### 3. Using `count()` Method
```
┌─────────────────────────────────────────────────────────────┐
│              C++ count() Method Pattern                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   if (myMap.count(key) > 0) {                              │
│       // Key exists                                         │
│   } else {                                                  │
│       // Key doesn't exist                                  │
│   }                                                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Defensive Programming Patterns

```
┌─────────────────────────────────────────────────────────────┐
│              C++ Defensive Access Pattern                    │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   // Pattern 1: Check before access                         │
│   if (myMap.contains(key)) {                                │
│       auto value = myMap[key];                             │
│   }                                                         │
│                                                             │
│   // Pattern 2: Use find() for performance                 │
│   auto it = myMap.find(key);                               │
│   if (it != myMap.end()) {                                 │
│       return it->second;                                   │
│   }                                                         │
│   return defaultValue;                                      │
│                                                             │
│   // Pattern 3: Exception handling                          │
│   try {                                                     │
│       value = myMap.at(key);                               │
│   } catch (const std::out_of_range&) {                      │
│       // Handle missing key                                │
│   }                                                         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🐍 Python Error Handling

### Approaches to Safe Access

#### 1. Using `get()` Method (Recommended)
```
┌─────────────────────────────────────────────────────────────┐
│              Python get() Method Pattern                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   value = my_dict.get(key)           # Returns None       │
│   value = my_dict.get(key, default)  # Returns default     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### 2. Using Exception Handling
```
┌─────────────────────────────────────────────────────────────┐
│              Python Exception Handling Pattern               │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   try:                                                     │
│       value = my_dict[key]  # Raises KeyError if missing   │
│   except KeyError:                                         │
│       # Handle missing key                                 │
│       value = default_value                                │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### 3. Using `in` Operator
```
┌─────────────────────────────────────────────────────────────┐
│              Python Membership Check Pattern                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   if key in my_dict:                                       │
<div style="color: #888; font-size: 0.9em; margin-top: 2em;">
Repo maintained by <a href="https://github.com/mwakidenis" style="color: #888;">mwakidenis</a>
</div>
│       value = my_dict[key]                                │
│   else:                                                    │
│       # Handle missing key                                 │
│       value = default_value                                │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Safe Access Patterns Flow

```
Python Dictionary Access Methods:

Method 1: get()
┌─────────────┐
│ my_dict.get │──▶ Key exists? ──▶ Yes → Return value
│   (key)     │               │
│             │               No  → Return None
└─────────────┘

Method 2: [ ] operator
┌─────────────┐
│ my_dict[key]│──▶ Key exists? ──▶ Yes → Return value
│             │               │
│             │               No  → Raise KeyError
└─────────────┘

Method 3: in operator
┌─────────────┐
│ key in     │──▶ Key exists? ──▶ Yes → True
│   my_dict   │               │
│             │               No  → False
└─────────────┘
```

---

## ⚖️ C++ vs Python Comparison

| Aspect | C++ Approach | Python Approach |
|--------|--------------|------------------|
| **Default Access** | `operator[]` creates if missing | `[]` raises KeyError |
| **Safe Access** | `at()` throws exception | `get()` returns None/default |
| **Check Existence** | `find()`, `count()` | `in` operator |
| **Exception Handling** | try-catch blocks | try-except blocks |
| **Default Value** | Manual check required | Built-in to `get()` |

### Side-by-Side Comparison

```
C++ Style:                              Python Style:
┌─────────────────────┐                 ┌─────────────────────┐
│ // Check first     │                 │ # Check first       │
│ if (map.count(k))  │                 │ if k in my_dict:    │
│   use map[k];      │                 │   use my_dict[k]    │
└─────────────────────┘                 └─────────────────────┘

┌─────────────────────┐                 ┌─────────────────────┐
│ // Use find()      │                 │ # Use get()         │
│ auto it = map.find │                 │ value = my_dict.get│
│   (k);             │                 │   (k, default)      │
│ if (it != end())   │                 └─────────────────────┘
│   use it->second;  │
└─────────────────────┘

┌─────────────────────┐                 ┌─────────────────────┐
│ // Exception handle│                 │ # Exception handle  │
│ try {              │                 │ try:                │
│   auto v = map.at  │                 │   value = my_dict[k]│
│     (k);           │                 │ except KeyError:    │
│ } catch (...) {    │                 │   value = default   │
│ }                  │                 │                     │
└─────────────────────┘                 └─────────────────────┘
```

---

## 🎯 Interview Insights

### What Interviewers Look For

1. **Understanding of Access Patterns**
   - Do you know when to use `get()` vs `[]`?
   - Can you explain the difference between methods?

2. **Error Awareness**
   - Do you know what exceptions can be raised?
   - Can you handle missing keys gracefully?

3. **Performance Considerations**
   - Do you check existence before access (vs. exception handling)?
   - Do you understand the cost of each approach?

### Common Mistakes to Avoid

| Mistake | Impact | Solution |
|---------|--------|----------|
| Using `[]` without checking | KeyError/exception | Use `get()` |
| Not handling missing keys | Runtime crash | Always provide defaults |
| Double lookup (check + access) | Wasted operations | Use `get()` or `find()` |
| Assuming key exists | Logic errors | Defensive programming |

### Performance Impact

```
Access Method Comparison:

┌──────────────────┬─────────────┬─────────────────┐
│ Method           │ Best Case   │ Worst Case      │
├──────────────────┼─────────────┼─────────────────┤
│ Check + Access   │ O(1)        │ O(n) (collision)│
│ find() / get()   │ O(1)        │ O(n) (collision)│
│ Exception Handle │ O(1)*       │ O(n)            │
└──────────────────┴─────────────┴─────────────────┘

* Exception handling has overhead when exception occurs
```

---

## 🔑 Key Differences

- **C++** `operator[]` inserts with default if key missing (can cause bugs)
- **Python** `[]` raises KeyError (fail-fast behavior)
- **C++** `find()` returns iterator (efficient)
- **Python** `get()` returns value or default (convenient)
- **C++** requires explicit type for defaults
- **Python** handles types dynamically

---

## 💡 Best Practices

### C++ Best Practices
1. Use `find()` instead of `at()` for better performance
2. Always check iterator validity
3. Prefer `count()` > 0 for simple existence checks
4. Use exceptions sparingly for control flow

### Python Best Practices
1. Use `get()` with default for simple cases
2. Use `in` operator for boolean checks
3. Use try-except for "ask forgiveness" pattern
4. Avoid double lookups (check then access)
