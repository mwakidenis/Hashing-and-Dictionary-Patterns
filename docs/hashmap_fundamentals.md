# HashMap Fundamentals

## What is a HashMap?

A HashMap (also known as hash table, dictionary, or associative array) is a data structure that implements a key-value pair mapping. It allows for efficient insertion, deletion, and lookup operations.

---

## 🔄 Hashing Flow Diagram

```
┌──────────┐     ┌───────────────┐     ┌─────────┐     ┌─────────┐     ┌──────────┐
│   Key    │ ──▶ │ Hash Function │ ──▶ │  Index  │ ──▶ │ Bucket  │ ──▶ │  Value   │
│ "apple"  │     │   h(key)      │     │   3     │     │   [ ]   │     │  "fruit" │
└──────────┘     └───────────────┘     └─────────┘     └─────────┘     └──────────┘
```

### Step-by-Step Explanation

| Step | Description |
|------|-------------|
| 1. Input Key | The key is provided (e.g., "apple") |
| 2. Hash Function | Converts key to a numeric hash value |
| 3. Index Computation | Modulo operation maps hash to array index |
| 4. Bucket Access | Direct access to the bucket at that index |
| 5. Value Retrieval | Returns the stored value |

---

## ⚡ Constant-Time Complexity (O(1))

### What Does O(1) Mean?

```
┌─────────────────────────────────────────────────────────────┐
│                    O(1) — Constant Time                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Operation: Get Value                                      │
│                                                             │
<span style="color: #888; font-size: 0.9em;">Code by <a href="https://github.com/mwakidenis" target="_blank">mwakidenis</a>, repo maintained by 7ions</span>
│   Input ──────────────▶ Result                               │
│     │                    │                                  │
│     │    Same time       │                                  │
│     │    regardless      │                                  │
│     │    of size!        │                                  │
│     ▼                    ▼                                  │
│                                                             │
│   [1 item] ───────────▶ Found ✓                             │
│   [100 items] ────────▶ Found ✓                             │
│   [1M items] ────────▶ Found ✓                              │
│                                                             │

<div style="color: #888; font-size: 0.9em; margin-top: 2em;">
Project by 7ions &mdash; Repo maintained by <a href="https://github.com/mwakidenis" style="color: #888;">mwakidenis</a>
</div>
└─────────────────────────────────────────────────────────────┘
```

### When O(1) Holds vs Breaks

| Scenario | Complexity | Reason |
|----------|------------|--------|
| Normal case | O(1) | Direct index access |
| Poor hash function | O(n) | Many collisions |
| High load factor | O(n) | Rehash required |
| Attack scenario | O(n) | Deliberate collision attack |

---

## 💥 Collision Handling

### What is a Collision?

A collision occurs when two different keys produce the same hash index.

```
┌────────────────────────────────────────────────────────────────┐
│                    Collision Scenario                         │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│   Key: "apple"  ──▶  Hash: 42  ──▶  Index: 2                   │
│   Key: "banana" ──▶  Hash: 87  ──▶  Index: 2  ⚠️ COLLISION!   │
│                                                                │
│   Bucket Array:                                               │
│   ┌───┬───┬───┬───┬───┬───┐                                   │
│   │ 0 │ 1 │ 2 │ 3 │ 4 │ 5 │                                   │
│   └───┴───┴─┬─┴───┴───┴───┘                                   │
│             │                                                  │
│             ▼                                                  │
│        ┌────┴────┐                                            │
│        │ "apple" │                                            │
│        │"banana" │  ← Collision chain                         │
│        └─────────┘                                            │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

### Collision Resolution Methods

#### 1. Chaining (Linked List in Bucket)
```
Bucket 2
   │
   ▼
┌─────────┐     ┌─────────┐     ┌─────────┐
│  Entry  │ ──▶ │  Entry  │ ──▶ │  Entry  │
│ Key: A  │     │ Key: B  │     │ Key: C  │
│ Val: 1  │     │ Val: 2  │     │ Val: 3  │
└─────────┘     └─────────┘     └─────────┘
```

#### 2. Open Addressing (Linear Probing)
```
Step 1: Hash("cat") = Index 3 → Occupied → Try 4 → Empty → Store
Step 2: Hash("dog") = Index 3 → Occupied → Try 4 → Occupied → Try 5 → Empty → Store
```

---

## ⚖️ C++ vs Python Comparison

| Aspect | C++ Perspective | Python Perspective |
|--------|------------------|---------------------|
| **Name** | `std::unordered_map` | `dict` (Dictionary) |
| **Implementation** | Template-based hash table | Hash table with open addressing |
| **Default Behavior** | Throws `std::out_of_range` | Raises `KeyError` |
| **Thread Safety** | Not thread-safe by default | Not thread-safe |
| **Ordering** | Unordered (C++20 has `std::unordered_map`) | Python 3.7+ maintains insertion order |

### Memory Layout Comparison

```
C++ unordered_map:          Python dict:
┌─────────────────┐         ┌─────────────────┐
│  Key Object     │         │  PyObject*      │ ──▶ Key
│  Value Object   │         │  PyObject*      │ ──▶ Value
│  Next Pointer   │         │  Hash Value     │
└─────────────────┘         │  Key+Ref Count  │
                            └─────────────────┘
```

---

## 🔑 Key Differences

- **C++** requires explicit type specification
- **Python** is dynamically typed
- **C++** offers more control over memory
- **Python** handles memory automatically (garbage collection)
- **C++** can use custom hash functions via specialization
- **Python** uses built-in `__hash__` method

---

## 🎯 Interview Insights

### What Interviewers Look For

1. **Understanding of Hashing**
   - Can you explain how hashing works?
   - Do you know what makes a good hash function?

2. **Complexity Analysis**
   - Can you analyze time and space complexity?
   - Do you understand when O(1) becomes O(n)?

3. **Collision Handling**
   - Do you know different resolution strategies?
   - Can you discuss trade-offs?

### Common Mistakes to Avoid

| Mistake | Why It's Wrong |
|---------|----------------|
| Assuming O(1) always | Collisions can degrade performance |
| Ignoring load factor | High load factor causes rehashing |
| Using mutable keys | Leads to unpredictable behavior |
| Forgetting thread safety | Race conditions in concurrent code |

### Optimization Thinking

- **Pre-sizing**: Reserve capacity to avoid rehashing
- **Custom hash**: For custom types, implement efficient hashing
- **Load factor tuning**: Balance memory vs. performance
- **Bloom filters**: Use for membership testing before lookup
