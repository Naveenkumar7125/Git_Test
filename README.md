# 5. Longest Palindromic Substring

![LeetCode](https://img.shields.io/badge/LeetCode-Medium-orange?style=flat-square&logo=leetcode)
![Language](https://img.shields.io/badge/Language-Java-blue?style=flat-square&logo=java)
![Approach](https://img.shields.io/badge/Approach-Expand%20Around%20Center-green?style=flat-square)

## 📋 Problem Statement

Given a string `s`, return the **longest palindromic substring** in `s`.

**Example 1:**
```
Input:  s = "babad"
Output: "bab"
```

**Example 2:**
```
Input:  s = "cbbd"
Output: "bb"
```

---

## 💡 Approach — Expand Around Center

The key insight is that every palindrome has a **center** — either a single character (odd-length) or a gap between two characters (even-length).

For each index `i`, we:
1. **Expand outward** treating `i` as the center of an **odd-length** palindrome (`l = i`, `r = i`)
2. **Expand outward** treating the gap between `i` and `i+1` as the center of an **even-length** palindrome (`l = i`, `r = i+1`)
3. Keep expanding as long as characters on both sides match (`s[l] == s[r]`)
4. Track the **maximum length** found and the corresponding substring

### Visual Example
```
s = "r a c e c a r"
         ^
     center = 'e' (index 3)

Expand: a == a ✓
Expand: c == c ✓
Expand: r == r ✓
→ "racecar" is the longest palindrome
```

---

## ✅ Solution
```java
class Solution {
    public String longestPalindrome(String s) {

        int n = s.length();
        String maxString = "";
        int max = 0;

        for (int i = 0; i < n; i++) {

            // Case 1: Odd-length palindrome (single center)
            int l = i, r = i;
            while (l >= 0 && r < n && s.charAt(l) == s.charAt(r)) {
                if (max < (r - l + 1)) {
                    max = r - l + 1;
                    maxString = s.substring(l, r + 1);
                }
                l--;
                r++;
            }

            // Case 2: Even-length palindrome (gap between i and i+1)
            l = i;
            r = i + 1;
            while (l >= 0 && r < n && s.charAt(l) == s.charAt(r)) {
                if (max < (r - l + 1)) {
                    max = r - l + 1;
                    maxString = s.substring(l, r + 1);
                }
                l--;
                r++;
            }
        }

        return maxString;
    }
}
```

---

## 📊 Complexity Analysis

| | Complexity | Reason |
|---|---|---|
| **Time** | `O(n²)` | For each of `n` centers, we expand up to `O(n)` in the worst case |
| **Space** | `O(1)` | Only a few variables used; no extra array needed |

---

## 🔍 Dry Run

**Input:** `s = "cbbd"`

| Center `i` | Type | Expansion | Max Found |
|---|---|---|---|
| 0 (`c`) | Odd | `c` only | `"c"` (len 1) |
| 1 (`b`) | Odd | `b` only | `"b"` (len 1) |
| 1–2 (`bb`) | Even | `b == b` ✓ → `c ≠ d` ✗ | `"bb"` (len 2) ✅ |
| 2 (`b`) | Odd | `b` only | — |
| 3 (`d`) | Odd | `d` only | — |

**Output:** `"bb"`

---

## 🔗 Related Problems

| Problem | Difficulty | Similarity |
|---|---|---|
| [516. Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/) | Medium | Palindrome DP |
| [647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/) | Medium | Same expand technique |
| [132. Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/) | Hard | Palindrome + DP |

---

## 📌 Key Takeaways

- Expanding around center is more **space-efficient** than the `O(n²)` DP table approach
- Always handle **both odd and even** centers separately
- This pattern reappears in [LC 647](https://leetcode.com/problems/palindromic-substrings/) — worth mastering
- A linear `O(n)` solution exists via **Manacher's Algorithm** for further optimization
