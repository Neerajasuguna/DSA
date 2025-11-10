

# 🧾 Problem Documentation — Find the Closest Palindrome (Leetcode 564)

---

## 🧩 Problem Statement

Given a string `n` representing an integer, return the **closest integer (in absolute difference)** that is a **palindrome** — *but not equal to `n` itself.*

If there are multiple closest values, return the **smallest** one.

---

### 🔢 Example 1:

```
Input:  n = "123"
Output: "121"
Explanation: The closest palindrome to 123 is 121 (diff = 2).
```

### 🔢 Example 2:

```
Input:  n = "1"
Output: "0"
Explanation: 0 is the closest palindrome smaller than 1.
```

### 🔢 Example 3:

```
Input:  n = "12321"
Output: "12221"
Explanation: Both 12221 and 12421 are 100 away, return smaller.
```

---

## 🎯 Objective

Find the **numerically nearest palindrome** (|candidate − n| minimal),
excluding `n` itself, and return it as a **string**.

---

## ⚙️ Constraints

* `n` consists of digits only.
* `1 ≤ len(n) ≤ 18`
* `n` does not contain leading zeros (except "0" itself).
* Output must fit within a 64-bit signed integer range.

---

## 🧠 Intuition

1. **Palindromes are symmetric**, so their entire value is determined by their **left half**.
   → To find nearby palindromes, you only need to adjust this left part slightly.

2. For a number `n` of length `L`, its *closest palindrome* will have:

   * The **same length**, or
   * **One less digit** (like 1000 → 999), or
   * **One more digit** (like 999 → 1001)

3. Therefore, you only need to test a *small set of candidates* derived from:

   * Mirroring the prefix (directly)
   * Mirroring (prefix ± 1)
   * Handling structural edge cases like 999… and 1000…

---

## 🪞 Approach — Prefix Mirroring Logic

1. **Extract prefix**: first `(len + 1) / 2` digits of `n`.

2. **Generate nearby prefixes**:

   ```
   prefix - 1, prefix, prefix + 1
   ```

3. **Form palindromes** by mirroring the prefix:

   * For odd lengths, mirror all except the middle digit.
   * For even lengths, mirror the entire prefix.

4. **Add two boundary cases**:

   * `10^len + 1` → handles inputs like `"999"` → `"1001"`
   * `10^(len-1) - 1` → handles inputs like `"1000"` → `"999"`

5. **Exclude `n` itself** and pick the palindrome with:

   * Minimum `|candidate - n|`
   * If tie, smaller numeric value.

---

## 🧪 Example Dry Run

Input: `"12321"`

```
prefix = "123"
Possible prefixes = [122, 123, 124]
Mirror:
122 → 12221
123 → 12321 (same, skip)
124 → 12421
Boundary: 9999, 100001

Candidates = [9999, 12221, 12421, 100001]
Differences:
9999  → 2322
12221 → 100
12421 → 100
100001 → 87680

Result = "12221" (tie → smaller)
```

✅ Output = `"12221"`

---

## 💻 Java Implementation

```java
class Solution {
    public String nearestPalindromic(String n) {
        long num = Long.parseLong(n);
        int len = n.length();
        HashSet<Long> candidates = new HashSet<>();

        // Edge case candidates
        candidates.add((long)Math.pow(10, len) + 1);     // e.g. 999 -> 1001
        candidates.add((long)Math.pow(10, len - 1) - 1); // e.g. 1000 -> 999

        // Extract prefix and build mirrored candidates
        long prefix = Long.parseLong(n.substring(0, (len + 1) / 2));
        for (long i = prefix - 1; i <= prefix + 1; i++) {
            StringBuilder sb = new StringBuilder();
            String p = String.valueOf(i);
            sb.append(p);
            String rev = new StringBuilder(p.substring(0, len / 2)).reverse().toString();
            sb.append(rev);
            try {
                candidates.add(Long.parseLong(sb.toString()));
            } catch (Exception e) {}
        }

        // Find closest palindrome
        long closest = -1;
        for (long cand : candidates) {
            if (cand == num) continue;
            if (closest == -1 ||
                Math.abs(cand - num) < Math.abs(closest - num) ||
                (Math.abs(cand - num) == Math.abs(closest - num) && cand < closest)) {
                closest = cand;
            }
        }

        return String.valueOf(closest);
    }
}
```

---

## 🧮 Complexity Analysis

| Complexity | Explanation                                           |
| ---------- | ----------------------------------------------------- |
| **Time**   | O(L) — only a few (≤5) candidates, each built in O(L) |
| **Space**  | O(1) — constant extra storage for candidates          |

---

## ⚠️ Edge Cases

| Type                   | Input   | Output  | Explanation                      |
| ---------------------- | ------- | ------- | -------------------------------- |
| **All 9’s**            | "999"   | "1001"  | next palindrome increases length |
| **Power of 10**        | "1000"  | "999"   | previous palindrome shortens     |
| **Already palindrome** | "12321" | "12221" | skip same number                 |
| **Single digit**       | "1"     | "0"     | return smaller number            |
| **Even digits**        | "1221"  | "1111"  | mirror behavior changes          |
| **Prefix overflow**    | "9999"  | "10001" | prefix+1 increases digits        |

---

## 🧭 Key Insights

* Only **3 local prefix variants** + **2 boundary cases** matter.
* **Mirroring** turns a complex number space into a simple pattern search.
* Palindromes cluster symmetrically — nearby palindromes differ only in middle digits.
* Think in terms of **prefix control → global symmetry**.

---

## ✅ Summary

| Concept          | Description                                       |
| ---------------- | ------------------------------------------------- |
| Core Idea        | Mirror-based generation around prefix             |
| Goal             | Find nearest palindrome ≠ n                       |
| Trick            | Only need local prefix ±1 changes                 |
| Handles          | All edge structural transitions (9’s, 10’s, etc.) |
| Time Complexity  | O(L)                                              |
| Space Complexity | O(1)                                              |

---

