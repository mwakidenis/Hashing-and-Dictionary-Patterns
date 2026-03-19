# Interview Patterns: HashMap Problems

## Common HashMap Interview Patterns

```
┌─────────────────────────────────────────────────────────────────────┐
│                  HashMap Interview Patterns                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐              │
│   │  Frequency   │  │  Two Pointers│  │   Sliding   │              │
│   │   Counting   │  │   Problems   │  │   Window    │              │
│   └──────┬───────┘  └──────┬───────┘  └──────┬───────┘              │
│          │                 │                 │                      │
│          ▼                 ▼                 ▼                      │
│   "Most Common"      "Two Sum"         "Longest Substring"          │
│   "Anagrams"         "3 Sum"           "Min Window"                │
│                                                                     │
│   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐              │
│   │   Mapping    │  │   Caching    │  │  Substring   │              │
│   │   Problems   │  │   (LRU)      │  │   Matching   │              │
│   └──────┬───────┘  └──────┬───────┘  └──────┬───────┘              │
│          │                 │                 │                      │
│          ▼                 ▼                 ▼                      │
│   "Isomorphic"       "LRU Cache"       "Word Pattern"             │
│   "Word Dictionary"  "Memoization"     "Anagram Substring"         │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 📊 Pattern 1: Frequency Counting

### When to Use

- Counting occurrences of elements
- Finding most common/frequent elements
- Grouping by frequency

### Problem Flow

```
Input Array → Iterate Once → Store in HashMap → Analyze Frequency → Return Result

Example:
Input: [1, 2, 1, 3, 2, 1]

Step 1: Iterate
┌───┬───┬───┬───┬───┬───┐
│ 1 │ 2 │ 1 │ 3 │ 2 │ 1 │
└───┴───┴───┴───┴───┴───┘
 │   │   │   │   │   │
 ▼   ▼   ▼   ▼   ▼   ▼
 Update HashMap

Step 2: HashMap State
┌───────────┬────────────┐
│ Key       │ Count      │
├───────────┼────────────┤
│     1     │     3      │
│     2     │     2      │
│     3     │     1      │
└───────────┴────────────┘
```

### Time/Space Complexity

| Metric | Complexity | Explanation |
|--------|------------|-------------|
| **Time** | O(n) | Single pass through input |
| **Space** | O(k) | k = number of unique elements |

---

## 🔍 Pattern 2: Two Sum / Complement

### When to Use

- Finding pairs that sum to target
- Finding numbers that complement each other
- When you need to find "what pairs with current element"

### Problem Flow

```
Input Array + Target → For Each Element → Check Complement Exists → Return Pair

Example:
Input: [2, 7, 11, 15], Target: 9

Step 1: Start with first element
┌───┬───┬───┬───┐
│ 2 │ 7 │11 │15 │
└───┴───┴───┴───┘
│
▼
Current: 2, Complement: 9 - 2 = 7
HashMap: {} → Not found → Store {2: index}

Step 2: Move to second element
┌───┬───┬───┬───┐
│ 2 │ 7 │11 │15 │
└───┴───┴───┴───┘
   │
   ▼
Current: 7, Complement: 9 - 7 = 2
HashMap: {2: 0} → Found! → Return [0, 1]
```

### Time/Space Complexity

| Metric | Complexity | Explanation |
|--------|------------|-------------|
| **Time** | O(n) | Single pass, O(1) lookup per element |
| **Space** | O(n) | Store up to n elements |

---

## 🔄 Pattern 3: Anagram Grouping

### When to Use

- Grouping strings that are anagrams
- Finding words with same character composition
- Identifying permutations

### Problem Flow

```
Input Strings → Create Signature (Sorted) → Group by Signature → Return Groups

Example:
Input: ["eat", "tea", "tan", "ate", "nat", "bat"]

Step 1: Create signatures
┌────────┬────────────┐
│ String │ Sorted     │
├────────┼────────────┤
│  eat   │    aet     │
│  tea   │    aet     │
│  tan   │    ant     │
│  ate   │    aet     │
│  nat   │    ant     │
│  bat   │    abt     │
└────────┴────────────┘

Step 2: Group by signature
┌─────────────┬────────────────────┐
│  Signature  │      Group         │
├─────────────┼────────────────────┤
│    aet      │ [eat, tea, ate]    │
│    ant      │ [tan, nat]         │
│    abt      │ [bat]               │
└─────────────┴────────────────────┘
```

### Time/Space Complexity

| Metric | Complexity | Explanation |
|--------|------------|-------------|
| **Time** | O(n × k log k) | n strings, k chars each, sorting |
| **Space** | O(n × k) | Store all strings |

---

## 🪟 Pattern 4: Sliding Window

### When to Use

- Finding substring with property
- Window size varies
- Contiguous subarrays/substrings

### Problem Flow

```
Input + Window Rules → Expand Window → Shrink When Needed → Track Best Answer

Example:
Find longest substring without repeating chars
Input: "abcabcbb"

Step-by-step:
a b c a b c b b
│       │
└───────┘
Window: "abc" (length 3)

Slide and track maximum:
"abca" → shrink → "bca" → "abc" → etc.
Result: 3 (for "abc")
```

### Time/Space Complexity

| Metric | Complexity | Explanation |
|--------|------------|-------------|
| **Time** | O(n) | Each element visited at most twice |
| **Space** | O(min(m, n)) | m = charset size, n = string length |

---

## 📦 Pattern 5: Key-Value Mapping

### When to Use

- One-to-one mapping validation
- Pattern matching (isomorphic strings)
- Dependency tracking

### Problem Flow

```
String/Pattern → Map Characters → Validate No Conflicts → Return Result

Example:
Pattern: "abba", String: "dog cat cat dog"

Step-by-step:
┌─────────┬─────────────┬─────────────────┐
│ Pattern │   String    │   Mapping       │
├─────────┼─────────────┼─────────────────┤
│    a    │    dog      │ dog → a         │
│    b    │    cat      │ cat → b         │
│    b    │    cat      │ cat → b ✓       │
│    a    │    dog      │ dog → a ✓       │
└─────────┴─────────────┴─────────────────┘

Result: True (valid mapping)
```

---

## ⚖️ C++ vs Python Thinking

| Pattern | C++ Thinking | Python Thinking |
|---------|--------------|------------------|
| **Frequency** | Pre-size map with `reserve()` | Use `collections.Counter` |
| **Two Sum** | Use `unordered_map.find()` | Use dict `.get()` method |
| **Anagrams** | Sort string to key | Sort string to key |
| **Sliding Window** | Manual index management | Slice and set operations |

### Mental Model Differences

```
C++ Approach:
- Explicit type declarations
- Iterator-based traversal
- Need to handle edge cases manually
- Performance-conscious design

Python Approach:
- Dynamic typing
- Built-in utilities (Counter, defaultdict)
- Cleaner, more readable logic
- Trade-off: slightly slower
```

---

## 🎯 Interview Insights

### Company-Specific Expectations

#### Google
- Expects optimal time/space complexity
- Wants clean, maintainable solutions
- Tests follow-up questions deeply
- Values multiple approaches

#### Microsoft
- Emphasizes problem-solving process
- Tests understanding of trade-offs
- Likes optimized solutions
- Asks about edge cases

### Follow-up Questions to Expect

| Question | What It Tests |
|----------|----------------|
| "Can you improve the space complexity?" | Optimization thinking |
| "What if the data is sorted?" | Algorithm adaptation |
| "How would you handle duplicates?" | Edge case handling |
| "What if input is huge?" | Scalability awareness |
| "Thread-safety concerns?" | Concurrency knowledge |

### Common Pitfalls

| Pitfall | Why It Fails | Fix |
|---------|--------------|-----|
| Double lookup | Wastes time | Use `get()` or `find()` |
| Not handling duplicates | Incorrect results | Use set or check existence |
| Wrong complexity | Shows misunderstanding | Practice analysis |
| Not considering edge cases | Incomplete solution | Test with empty, single, repeated |


## 💡 Problem-Solving Strategy

```
When You See a HashMap Problem:

1. Identify the Pattern
   ┌─────────────────────────────────────┐
   │  Frequency? → Pattern 1             │
   │  Find pair? → Pattern 2             │
   │  Grouping? → Pattern 3              │
   │  Window?   → Pattern 4              │
   │  Mapping?  → Pattern 5              │
   └─────────────────────────────────────┘

2. Design the Algorithm
   ┌─────────────────────────────────────┐
   │  Single pass vs Multiple passes     │
   │  What to store in map?              │
   │  What becomes the key?              │
   └─────────────────────────────────────┘

3. Analyze Complexity
   ┌─────────────────────────────────────┐
   │  Time: O(?)                         │
   │  Space: O(?)                        │
   └─────────────────────────────────────┘

4. Handle Edge Cases
   ┌─────────────────────────────────────┐
   │  Empty input?                      │
   │  Single element?                    │
   │  All duplicates?                    │
   └─────────────────────────────────────┘
```
<div style="color: #888; font-size: 0.9em; margin-top: 2em;">Repo maintained by <a href="https://github.com/mwakidenis" style="color: #888;">mwakidenis</a></div>
