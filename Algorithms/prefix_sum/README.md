# Prefix Sum (Arrays)

## 🤔 What Problem Does Prefix Sum Solve?

When you're dealing with arrays, you will often be asked to **find the sum of numbers between two indexes** (like from index `l` to `r`).

### Example:

```python
nums = [2, 5, 1, 3, 7]
# What is the sum from index 1 to 3?
# 5 + 1 + 3 = 9
```

If you do this repeatedly (especially inside loops), it becomes slow — because every time you're re-adding values.

This leads to **O(n)** work each time.

---

## 💡 Prefix Sum: The Idea

Instead of adding repeatedly, we **precompute** a running total of the array.

Example:

```
nums:   [2, 5, 1, 3, 7]
prefix: [2, 7, 8, 11, 18]
```

Meaning:

* prefix[i] stores the **sum from nums[0] → nums[i]**

Now sum of any range `l` to `r` can be done in **O(1)** time:

```
sum(l, r) = prefix[r] - prefix[l - 1]
```

For example: sum(1,3) = prefix[3] - prefix[0] = 11 - 2 = 9 ✅

---

## 🧠 Why Is This Important?

Because prefix sums:

* **Avoid repeated work**
* Reduce runtime from **O(n²)** to **O(n)** or even **O(1)** per query
* Help with many problems involving **subarrays** (continuous chunks of the array)

Prefix sums also help detect patterns like:

* Subarrays that sum to a target
* Balanced counts (like equal number of 0s and 1s)
* Cumulative frequency changes

---

## 🎯 When Should I Think of Prefix Sum?

Look for key phrases in problems:

| If the problem asks for…                  | Prefix Sum may help because…              |
| ----------------------------------------- | ----------------------------------------- |
| Sum of part of an array (repeated)        | Prefix sum prevents re-adding numbers     |
| Number of subarrays that sum to something | Prefix sums + HashMap track past sums     |
| Balanced or equal count of two values     | Convert one value and use prefix tracking |

**If you see "subarray" and "sum" in the prompt → Think Prefix Sum.**

---

# 📝 Practice Problems

## 1. Range Sum Query - Immutable (LeetCode 303)

**Goal:** Quickly get sum of any subarray multiple times.

### Approach:

* Build prefix array once in the constructor
* Use difference between prefix values to answer queries fast

```python
class NumArray:
    def __init__(self, nums):
        self.prefix = [0]
        for n in nums:
            self.prefix.append(self.prefix[-1] + n)

    def sumRange(self, left, right):
        return self.prefix[right + 1] - self.prefix[left]
```

### Key Concept:

📌 Once prefix is built → every query = O(1) time.

---

## 2. Contiguous Array (LeetCode 525)

**Goal:** Find the **longest subarray** where number of 0s = number of 1s.

### Trick:

Convert:

```
0 → -1
1 → +1
```

If the prefix sum repeats, it means the subarray between them has equal +1 and -1 (so equal 1s and 0s).

```python
def findMaxLength(nums):
    prefix = 0
    seen = {0: -1}
    longest = 0

    for i, n in enumerate(nums):
        prefix += 1 if n == 1 else -1
        
        if prefix in seen:
            longest = max(longest, i - seen[prefix])
        else:
            seen[prefix] = i

    return longest
```

### Key Concept:

✅ Repeated prefix sum means the values between those indexes balance out.

---

## 3. Subarray Sum Equals K (LeetCode 560)

**Goal:** Count how many subarrays sum to `k`.

### Logic:

* Use hashmap to store how many times a prefix sum has appeared.
* If `prefix - k` has appeared before, we found a valid subarray.

```python
def subarraySum(nums, k):
    prefix = 0
    freq = {0: 1}
    count = 0

    for n in nums:
        prefix += n
        
        if prefix - k in freq:
            count += freq[prefix - k]
        
        freq[prefix] = freq.get(prefix, 0) + 1
    
    return count
```

### Key Concept:

🎯 We’re counting ways past prefix sums help us form `k`.

---

# ✅ Summary (Keep This in Mind!)

| Concept                     | Explanation                                                |
| --------------------------- | ---------------------------------------------------------- |
| Prefix Sum Array            | Stores cumulative sums; speeds up repeated sum queries     |
| Identify by keywords        | “Subarray” + “sum”, “equal counts”, “range queries”        |
| Often paired with Hashmaps  | Helps count subarrays efficiently                          |
| Time Complexity Improvement | Can reduce from **O(n²)** → **O(n)** or **O(1) per query** |

Prefix sums are one of the **most common** array techniques — you'll use them in DP, sliding window, and even graph problems later.
