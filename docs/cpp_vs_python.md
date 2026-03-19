# C++ vs Python: HashMaps & Dictionaries

## Overview Comparison

```
┌─────────────────────────────────────────────────────────────────────┐
│                    Language Philosophy Comparison                   │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   ┌─────────────────────┐         ┌─────────────────────┐          │
│   │        C++         │         │       Python        │          │
│   ├─────────────────────┤         ├─────────────────────┤          │
│   │  Compiled Language │         │ Interpreted Language│          │
│   │  Static Typing     │         │ Dynamic Typing      │          │
│   │  Manual Memory     │         │ Automatic Memory    │          │
│   │  Performance First │         │ Readability First   │          │
│   └─────────────────────┘         └─────────────────────┘          │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 📊 Comprehensive Comparison

### Syntax Philosophy

| Aspect | C++ | Python |
|--------|-----|--------|
| **Declaration** | Explicit type required | Dynamic, no type needed |
| **Verbosity** | More verbose | Concise and readable |
| **Flexibility** | Strict type system | Duck typing |
| **Learning Curve** | Steeper | Gentle |

### Syntax Flow Comparison

```
C++ Syntax Flow:
┌─────────────────┐
│ unordered_map   │  ← Specify container type
<span style="color: #888; font-size: 0.9em;">code created for 7ions by <a href="https://github.com/mwakidenis" target="_blank">mwakidenis</a></span>
<span style="color: #888; font-size: 0.9em;">code by <a href="https://github.com/mwakidenis" target="_blank">mwakidenis</a></span>
│ <string, int>   │  ← Specify key-value types
│ myMap;          │  ← Variable declaration
└─────────────────┘

Python Syntax Flow:
┌─────────────────┐
│ my_dict = {}    │  ← Direct dictionary creation
│                 │  ← Types inferred automatically
└─────────────────┘
```
<div style="color: #888; font-size: 0.9em; margin-top: 2em;">
Project by 7ions &mdash; Repo maintained by <a href="https://github.com/mwakidenis" style="color: #888;">mwakidenis</a>
</div>

---

## 🧠 Memory Handling

### C++ Memory Model

```
┌─────────────────────────────────────────────────────────────────┐
│                    C++ HashMap Memory Layout                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Stack                    Heap                                  │
│   ┌───────┐               ┌────────────────────────────┐        │
│   │ myMap │──────────────▶│ Bucket Array              │        │
│   └───────┘               │ ┌────┐┌────┐┌────┐┌────┐ │        │
│                           │ │ 0  ││ 1  ││ 2  ││ 3  │ │        │
│   ┌───────┐               │ └────┘└────┘└────┘└────┘ │        │
│   │ Key   │  ────────────▶│        │    │              │        │
│   │ Value │               │        ▼    ▼              │        │
│   └───────┘               │    ┌─────┐ ┌─────┐         │        │
│                           │    │Node │ │Node │         │        │
│   You control:            │    │next │ │next │         │        │
│   ✓ Allocation            │    └─────┘ └─────┘         │        │
│   ✓ Deallocation         └────────────────────────────┘        │
│   ✓ Lifetime                                                     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Python Memory Model

```
┌─────────────────────────────────────────────────────────────────┐
│                    Python Dict Memory Layout                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   PyObject* pointers (references) stored in hash table         │
│   ┌──────────────────────────────────────────────┐             │
│   │  ┌─────────┐  ┌─────────┐  ┌─────────┐       │             │
│   │  │Key Ptr* │  │Val Ptr* │  │  Hash   │       │             │
│   │  └─────────┘  └─────────┘  └─────────┘       │             │
│   │       │              │                        │             │
│   │       ▼              ▼                        │             │
│   │   ┌────────┐    ┌────────┐                    │             │
│   │   │  Key   │    │ Value  │                    │             │
│   │   │ Object │    │ Object │                    │             │
│   │   └────────┘    └────────┘                    │             │
│   └──────────────────────────────────────────────┘             │
│                                                                 │
│   Python handles:                                               │
│   ✓ Memory allocation                                           │
│   ✓ Reference counting                                          │
│   ✓ Garbage collection                                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🛡️ Safety Comparison

### Error Prevention

| Feature | C++ | Python |
|---------|-----|--------|
| **Type Checking** | Compile-time | Runtime |
| **Memory Safety** | Manual (error-prone) | Automatic (safer) |
| **Bounds Checking** | Manual | Automatic |
| **Null Safety** | Pointer-based | None (None type) |

### C++ Safety Mechanisms

```
┌─────────────────────────────────────────────────────────────┐
│              C++ Error Handling Approach                    │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   try {                                                    │
│       value = myMap.at(key);  // May throw                 │
│   } catch (const std::out_of_range& e) {                   │
│       // Handle missing key                                │
│   }                                                        │
│                                                             │
│   // Or use find() for safer access:                      │
│   auto it = myMap.find(key);                              │
│   if (it != myMap.end()) {                                 │
│       value = it->second;                                 │
│   }                                                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Python Safety Mechanisms

```
┌─────────────────────────────────────────────────────────────┐
│              Python Error Handling Approach                 │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   try:                                                     │
│       value = my_dict[key]  # May raise KeyError          │
│   except KeyError:                                         │
│       # Handle missing key                                 │
│       pass                                                 │
│                                                             │
│   # Or use get() for safer access:                         │
│   value = my_dict.get(key, default_value)                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## ⚡ Performance Comparison

### Performance Characteristics

| Operation | C++ unordered_map | Python dict |
|-----------|------------------|-------------|
| **Insert** | O(1) average | O(1) average |
| **Lookup** | O(1) average | O(1) average |
| **Delete** | O(1) average | O(1) average |
| **Iteration** | O(n) | O(n) |
| **Memory** | Lower overhead | Higher overhead |

### Performance Flow Diagram

```
Operation Flow Comparison:

C++:                              Python:
┌─────────┐                       ┌─────────┐
│ Insert  │                       │ Insert  │
└────┬────┘                       └────┬────┘
     │                                  │
     ▼                                  ▼
┌─────────────┐                  ┌─────────────┐
│ Hash + Find │                  │ Hash + Find │
│ Bucket      │                  │ Entry       │
└────┬───────┘                  └────┬───────┘
     │                                  │
     ▼                                  ▼
┌─────────────┐                  ┌─────────────┐
│ Direct      │                  │ Direct      │
│ Insert      │                  │ Insert      │
└─────────────┘                  └─────────────┘
```

---

## 📖 Readability Comparison

### Code Readability

| Aspect | C++ | Python |
|--------|-----|--------|
| **Line Count** | More lines | Fewer lines |
| **Syntax Noise** | High (`< >`, `::`, `;`) | Low (indentation-based) |
| **Self-Documenting** | Moderate | High |
| **Onboarding** | Slower | Faster |

### Visual Readability Comparison

```
C++:
std::unordered_map<std::string, int> word_count;
word_count.insert(std::make_pair("hello", 1));
auto it = word_count.find("hello");
if (it != word_count.end()) {
    std::cout << it->second << std::endl;
}

Python:
word_count = {"hello": 1}
if "hello" in word_count:
    print(word_count["hello"])
```

---

## 🎯 Interview Insights

### What Interviewers Expect

| Language | Key Expectations |
|----------|------------------|
| **C++** | Understanding of templates, memory management, iterator usage |
| **Python** | Familiarity with dict methods, comprehension syntax, built-in functions |

### Common Pitfalls by Language

#### C++ Pitfalls
- Forgetting to check if key exists before accessing
- Not handling rehashing scenarios
- Memory leaks with custom allocators
- Incorrect iterator invalidation

#### Python Pitfalls
- Using mutable objects as keys
- Confusing `dict.get()` with `dict[]` access
- Not handling `KeyError` exceptions
- Modifying dict during iteration

---

## 🔑 Key Differences Summary

| Category | C++ | Python |
|----------|-----|--------|
| **Setup** | More boilerplate | Simpler syntax |
| **Types** | Static (compile-time) | Dynamic (runtime) |
| **Memory** | Manual control | Automatic GC |
| **Safety** | More error-prone | Built-in protections |
| **Performance** | Generally faster | Generally slower |
| **Learning** | Steeper curve | Easier to start |
| **Debugging** | More complex | Simpler debugging |

---

## 💡 When to Use Each

| Scenario | Recommended Language |
|----------|---------------------|
| High-performance systems | C++ |
| Rapid prototyping | Python |
| Memory-constrained environments | C++ |
| Quick script development | Python |
| Production web services | Either (depends on team) |
| Competitive programming | Either (Python often preferred for simplicity) |
