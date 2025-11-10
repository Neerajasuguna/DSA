Here’s a clean, **copyable format** of the full solution (problem → intuition → explanation → code → complexity):

---

## 🧩 Problem Description

You are given two strings `s` and `t`, consisting only of lowercase English letters.
You can repeatedly append the string `s` to itself any number of times.

Your task is to determine the **minimum number of times** `s` must be appended to itself such that `t` becomes a **subsequence** of the resulting string.

If it is **impossible** to form `t` (because `t` contains a character not present in `s`), return `-1`.

---

### Example 1

```
Input:
s = "boy"
t = "oyb"

Output:
2

Explanation:
After appending s twice → "boyboy"
We can pick characters (o, y, b) from "boyboy" in order:
  o (from 1st copy)
  y (from 1st copy)
  b (from 2nd copy)
So t is a subsequence after 2 repetitions.
```

### Example 2

```
Input:
s = "abc"
t = "abcbc"

Output:
2

Explanation:
"abcabc" → we can form "abcbc" as a subsequence.
```

---

## 💡 Intuition

We’re trying to form `t` by scanning through **repeated copies** of `s`.
Each time we traverse `s`:

* We try to match as many characters of `t` as possible.
* When we reach the end of `s` but `t` isn’t finished, we start another repetition of `s` (incrementing our count).

If at any point a character in `t` doesn’t exist in `s`, it’s impossible.

---

## 🧠 Explanation

We maintain two pointers:

* `i` → current index in `s`
* `j` → current index in `t`

We scan both:

* If `s[i] == t[j]`, we move both pointers (`i++`, `j++`)
* Else, we only move `i++` (skip this character in `s`)

When `i` reaches the end of `s`:

* If `t` still has characters left, increment the count (we need one more repetition)
* Reset `i = 0`
* Before restarting, check if `t[j]` even exists in `s`. If not, return `-1`.

---

## ✅ Java Code (Greedy Approach)

```java
class Solution {
    public int shortestWay(String s, String t) {
        int count = 1;  // At least one repetition of s
        int i = 0;      // pointer in s
        int j = 0;      // pointer in t
        
        while (j < t.length()) {
            if (s.charAt(i) == t.charAt(j)) {
                // characters match → move both pointers
                i++;
                j++;
            } else {
                // skip s[i]
                i++;
            }
            
            // if end of s is reached but t not done
            if (i == s.length() && j < t.length()) {
                count++;
                i = 0; // restart scanning s
                // if t[j] not in s, impossible
                if (s.indexOf(t.charAt(j)) == -1) {
                    return -1;
                }
            }
        }
        
        return count;
    }
}
```

---

## 🕒 Time Complexity

* Worst-case: **O(|s| × |t|)**
  (for each repetition, we might scan through all of `s` while matching `t`).
* Average-case: typically **O(|s| + |t|)** for most inputs.

## 💾 Space Complexity

* **O(1)** — constant extra space used.

---

## ✅ Key Takeaways

* Use two pointers to greedily consume `t` from repeated copies of `s`.
* If any character in `t` doesn’t exist in `s`, return `-1`.
* This approach is clean, easy to reason about, and works efficiently for moderate input sizes.

---


**TODO *********explore optimal Solution **
