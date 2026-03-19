# Anagrams Problem

## Problem Description

Given an array of strings, group the anagrams together. You can return the answer in any order.

An anagram is a word formed by rearranging the letters of another word (e.g., "eat" and "tea" are anagrams).

---

## 📝 Plain English Explanation

Think of it like organizing books on a shelf by their content, not their title. Books with the same letters (regardless of order) should be grouped together.

---

## 🔄 Problem Flow Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                     Anagrams Solution Flow                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   Input: ["eat", "tea", "tan", "ate", "nat", "bat"]               │
│                                                                     │
│   ┌──────────────┐                                                 │
│   │ For each     │                                                 │
│   │ string       │                                                 │
│   └──────┬───────┘                                                 │
│          │                                                          │
│          ▼                                                          │
│   ┌──────────────┐                                                 │
│   │ Create       │                                                 │
│   │ Signature    │                                                 │
│   │ (sorted      │                                                 │
│   │  chars)      │                                                 │
│   └──────┬───────┘                                                 │
│          │                                                          │
│          ▼                                                          │
│   ┌──────────────┐     ┌──────────────┐                            │
│   │ Group by    │     │ Signature:    │                            │
│   │ Signature   │     │ ["eat","tea"]│                            │
│   │ in HashMap  │     │ ["tan","nat"] │                            │
│   └──────┬───────┘     │ ["bat"]      │                            │
│          │              └──────────────┘                            │
│          ▼                                                         │
│   ┌──────────────┐                                                 │
│   │ Return all  │                                                 │
│   │ groups      │                                                 │
│   └──────────────┘                                                 │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🚫 Naive Approach

### Step-by-Step Logic

1. Compare every string with every other string
2. Check if they have the same character counts
3. Group similar strings together

### Why It's Slow

```
Naive Approach Visualization:

Input: ["eat", "tea", "tan", "ate", "nat", "bat"]

String 1: "eat" - Compare with 5 other strings
String 2: "tea" - Compare with remaining 4 strings
String 3: "tan" - Compare with remaining 3 strings
...

Time: O(n² × k) where k = average string length
```

### Complexity Issues

| Metric | Complexity | Problem |
|--------|------------|---------|
| **Time** | O(n² × k) | Nested comparison |
| **Space** | O(n²) | Storing all pairs |

---

## ✅ Optimized HashMap Approach

### Conceptual Steps

1. For each string, create a "signature"
2. A signature is a normalized form that identifies anagrams
3. Use sorted characters as signature (e.g., "eat" → "aet")
4. Group strings by their signature in a hash map
5. Return all groups from the hash map

### Signature Creation Process

```
Input String: "tea"

Step 1: Split into characters
t-e-a

Step 2: Sort alphabetically
a-e-t

Step 3: Join back to string
"aet"

This is the signature!
```

### HashMap State Evolution

```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"]

Step 1: "eat" → sorted "aet"
┌─────────┬────────────┐
│ Signature│ Group    │
├─────────┼────────────┤
│   aet   │ ["eat"]   │
└─────────┴────────────┘

Step 2: "tea" → sorted "aet"
┌─────────┬────────────┐
│ Signature│ Group    │
├─────────┼────────────┤
│   aet   │ ["eat","tea"]
└─────────┴────────────┘

Step 3: "tan" → sorted "ant"
┌─────────┬────────────┐
│ Signature│ Group    │
├─────────┼────────────┤
│   aet   │ ["eat","tea"]
│   ant   │ ["tan"]
└─────────┴────────────┘

... (continue for all strings)

Final Result:
┌─────────┬────────────────────┐
│ Signature│ Group             │
├─────────┼────────────────────┤
│   aet   │ ["eat","tea","ate"]│
│   ant   │ ["tan","nat"]      │
│   abt   │ ["bat"]            │
└─────────┴────────────────────┘
```

---

## ⏱️ Complexity Analysis

### Time Complexity

| Approach | Complexity | Explanation |
|----------|------------|-------------|
| **Naive** | O(n² × k) | Compare every pair |
| **Optimized** | O(n × k log k) | n strings, sort each in O(k log k) |

### Space Complexity

| Approach | Complexity | Explanation |
|----------|------------|-------------|
| **Naive** | O(n²) | Store all pairs |
| **Optimized** | O(n × k) | Store all strings |

### Optimization Note

```
If using character count (26 letters) instead of sorting:
- Time: O(n × k) instead of O(n × k log k)
- Trade-off: More complex implementation

This is often asked in follow-up questions!
```

---

## ⚖️ C++ vs Python Thinking

| Aspect | C++ Thinking | Python Thinking |
|--------|--------------|------------------|
| **Signature** | Sort string with `std::sort()` | Use `sorted()` function |
| **Map Type** | `unordered_map<string, vector<string>>` | `dict` with list values |
| **Key** | String (sorted characters) | String (sorted characters) |
| **Character counting** | Array of 26 ints | List or dict |

### Alternative: Character Count Signature

```
Instead of sorting, count characters:

"eat" → [1, 0, 1, 0, 1, ...] (a=1, e=1, t=1)
"tea" → [1, 0, 1, 0, 1, ...] (same!)

This is O(k) instead of O(k log k)
```

### C++ Character Array Approach

```
Signature as array:
int count[26] = {0};
for (char c : word) count[c - 'a']++;
// Convert to string key for map
```

### Python Counter Approach

```
Signature using Counter:
from collections import Counter
signature = tuple(sorted(Counter(word).items()))
```

---

## 🎯 Edge Cases

| Edge Case | How to Handle |
|-----------|---------------|
| **Empty strings** | Empty string sorts to empty string → group together |
| **Single character** | Each unique char forms its own group |
| **All same letters** | "aaa" and "aaa" → same group |
| **Different lengths** | Cannot be anagrams → different groups |
| **Unicode characters** | Works if consistent encoding |

### Test Your Understanding

```
Test Case 1: ["eat", "tea", "tan", "ate", "nat", "bat"]
Expected: [["eat","tea","ate"], ["tan","nat"], ["bat"]]
(Any order is acceptable)

Test Case 2: [""]
Expected: [[""]]

Test Case 3: ["a"]
Expected: [["a"]]
```

---

## 🎓 Interview Insights

### Why It's Commonly Asked

1. **Tests hash map grouping ability**
2. **Shows understanding of string manipulation**
3. **Has clear optimization path** (character counting)
4. **Real-world application** (search engines, spell checkers)

### Google Variations

- "Return groups in sorted order"
- "What if you have thousands of strings?"
- "How would you handle non-English characters?"

### Microsoft Variations

- "Can you do it without sorting?"
- "What's the minimum memory needed?"
- "How would you handle streaming input?"

### Follow-up Questions

| Question | Approach |
|----------|----------|
| Without sorting? | Character frequency array (26 letters) |
| Optimize further? | Pre-compute all possible anagram groups |
| Very long strings? | Consider hash of signature instead of full string |

---

## 💡 Alternative: Character Count Signature

### Why This Matters

```
Sorting: O(k log k) per string
Counting: O(k) per string

For very long strings, this matters!

Example: k = 1000 characters
Sort: 1000 × log2(1000) ≈ 10,000 operations
Count: 1000 operations
10x faster!
```

### Character Count Implementation

```
Signature format: "a1b0c1d0..." (26 letters)

For "abc": "a1b1c1d0e0..."
For "cba": "a1b1c1d0e0..." (same signature!)

This uniquely identifies character composition.
```
