# Two Sum Problem

## Problem Description

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

---

## 📝 Plain English Explanation

Think of it as finding two people in a room whose combined ages equal a target age. You could ask everyone their age and remember it, then look for someone whose age complements to reach the target.

---

## 🔄 Problem Flow Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                        Two Sum Solution Flow                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   Input: Array [2, 7, 11, 15], Target: 9                           │
│                                                                     │
│   ┌──────────────┐                                                 │
│   │ Start at     │                                                 │
│   │ first element│                                                 │
│   └──────┬───────┘                                                 │
│          │                                                          │
│          ▼                                                          │
│   ┌──────────────┐     ┌──────────────┐                            │
│   │ Calculate    │     │ Check if     │                            │
│   │ Complement  │     │ Complement    │                            │
│   │ Target-Curr │     │ Exists?       │                            │
│   └──────┬───────┘     └──────┬───────┘                            │
│          │                     │                                     │
│          │                ┌───┴───┐                                 │
│          │                │       │                                 │
│          │               Yes      No                                │
│          │                │       │                                 │
│          ▼                ▼       ▼                                 │
│   ┌──────────────┐  ┌──────────────┐                               │
│   │ Store in     │  │ Move to Next │                               │
│   │ HashMap +    │  │ Element      │                               │
│   │ Continue     │  │ (repeat)     │                               │
│   └──────────────┘  └──────────────┘                               │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🚫 Naive Approach

### Step-by-Step Logic

1. For each element in the array
2. For each other element in the array
3. Check if they sum to target
4. Return indices if found

### Why It's Slow

```
Naive Approach Visualization:

Array: [2, 7, 11, 15], Target: 9

Iteration 1:
┌───┬───┬───┬───┐
│ 2 │ 7 │11 │15 │
└───┴───┴─┬─┴───┘
  │   │   │
  └─► │   │  Check: 2+7=9 ✓ Found!
      │   │
      └─► │  Check: 2+11=13 ✗
          │  Check: 2+15=17 ✗
          │
          └─► Check: 7+2 ✗ (duplicate)

Time: O(n²) - Nested loops
```

---

## ✅ Optimized HashMap Approach

### Conceptual Steps

1. Create an empty hash map to store seen numbers
2. Iterate through each element in the array
3. For each element, calculate the complement (target - current)
4. Check if the complement exists in the hash map
   - If yes: Found the pair! Return indices
   - If no: Store current element and index in hash map
5. Continue to next element if not found

### HashMap State Evolution

```
Array: [2, 7, 11, 15], Target: 9

Step 1: Element = 2
┌───────────────┐     ┌───────────────┐
│ Complement    │     │ HashMap       │
│ 9 - 2 = 7     │     │ {}            │
│ In Map? No    │     │               │
└───────────────┘     └───────────────┘
Store: {2: 0}

Step 2: Element = 7
┌───────────────┐     ┌───────────────┐
│ Complement    │     │ HashMap       │
│ 9 - 7 = 2     │     │ {2: 0}        │
│ In Map? Yes! │     │  Found pair!  │
└───────────────┘     └───────────────┘

Result: [0, 1]
```

---

## ⏱️ Complexity Analysis

### Time Complexity

| Approach | Complexity | Explanation |
|----------|------------|-------------|
| **Naive** | O(n²) | Nested loops, check every pair |
| **Optimized** | O(n) | Single pass, O(1) lookup per element |

### Space Complexity

| Approach | Complexity | Explanation |
|----------|------------|-------------|
| **Naive** | O(1) | No extra space |
| **Optimized** | O(n) | Store up to n elements in hash map |

---

## ⚖️ C++ vs Python Thinking

| Aspect | C++ Thinking | Python Thinking |
|--------|--------------|------------------|
| **Map Type** | `unordered_map<int, int>` | Built-in `dict` |
| **Lookup** | `find()` method | `in` operator or `.get()` |
| **Storage** | Explicit type declaration | Dynamic typing |
| **Return** | `vector<int>` | List `[int, int]` |

### C++ Approach

```
Mental Model:
1. Use unordered_map<int, int> to store value → index
2. For each element, find complement using map.find()
3. Return vector of indices

Key insight: Only need to store one index per value
```

### Python Approach

```
Mental Model:
1. Use dict to store value → index
2. For each element, check if complement in dict
3. Return list of indices

Key insight: Python's dict provides clean, readable code
```

---

## 🎯 Edge Cases

| Edge Case | How to Handle |
|-----------|---------------|
| **No solution exists** | Problem states exactly one solution |
| **Same element twice** | Can't use same index twice (handled by logic) |
| **Negative numbers** | Works automatically (hash map handles any integer) |
| **Large numbers** | Works automatically |
| **Empty array** | Return empty (not possible per problem) |
| **Single element** | Return empty (need two elements) |

### Test Your Understanding

```
Test Case 1: [2, 7, 11, 15], Target: 9
Expected: [0, 1]
Why: 2 + 7 = 9

Test Case 2: [3, 2, 4], Target: 6
Expected: [1, 2]
Why: 2 + 4 = 6

Test Case 3: [3, 3], Target: 6
Expected: [0, 1]
Why: 3 + 3 = 6
```

---

## 🎓 Interview Insights

### Why It's Commonly Asked

1. **Tests fundamental hash map understanding**
2. **Shows optimization from brute force**
3. **Simple enough to explain clearly**
4. **Has clear follow-up potential**

### Google Variations

- "Find all pairs summing to target"
- "Return count instead of indices"
- "Handle duplicate values differently"

### Microsoft Variations

- "What if multiple solutions exist?"
- "How would you handle very large arrays?"
- "Can you solve without extra space?"

### Follow-up Questions

| Question | Answer Approach |
|----------|------------------|
| Can you do it in O(1) space? | Sort + Two pointers (O(n log n) time) |
| What if values can be negative? | Works the same |
| How to handle duplicates? | Store all indices in list |

---

## 💡 Key Takeaways

- **Hash maps enable O(1) lookup** - Store what you need to find later
- **Single pass solution** - Trade space for time
- **Complement thinking** - "What value would complete my solution?"
- **Edge cases matter** - Consider empty input, duplicates, negatives
