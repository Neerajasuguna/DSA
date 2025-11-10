https://leetcode.com/problems/2-keys-keyboard/description/

There is only one character 'A' on the screen of a notepad. You can perform one of two operations on this notepad for each step:

Copy All: You can copy all the characters present on the screen (a partial copy is not allowed).
Paste: You can paste the characters which are copied last time.
Given an integer n, return the minimum number of operations to get the character 'A' exactly n times on the screen.

 

Example 1:

Input: n = 3
Output: 3
Explanation: Initially, we have one character 'A'.
In step 1, we use Copy All operation.
In step 2, we use Paste operation to get 'AA'.
In step 3, we use Paste operation to get 'AAA'.
Example 2:

Input: n = 1
Output: 0




**Observation**

**Example 1 — n = 3**

Start with 1 ‘A’.

Step	Operation	Result	Explanation
1	Copy All	Copy "A"	clipboard = "A"
2	Paste	"AA"	2 A’s on screen
3	Paste	"AAA"	3 A’s on screen ✅


**Example 5 — n = 9**

Let’s factorize:
9 = 3 × 3

Steps:

Copy (A)

Paste (2A)

Paste (3A)

Copy (3A)

Paste (6A)

Paste (9A)

✅ Total = 6 operations




Pattern / Formula

The minimum operations to reach n is the sum of its prime factors.

Explanation

Each time you factorize n,
you’re saying:
“I can reach n by repeatedly copying and pasting smaller chunks.”

So, for any n,
break it down into its factors a * b →
it takes a operations to make a characters,
then b operations to repeat that chunk.





class Solution {
    public int minSteps(int n) {
        if (n == 1) return 0;

        int steps = 0;
        int factor = 2;

        while (n > 1) {
            while (n % factor == 0) {
                steps += factor;
                n /= factor;
            }
            factor++;
        }

        return steps;
    }
}



How it Works

We’re factorizing n,
and for each factor f, we add f to our operation count.

Because each factor corresponds to:

1 Copy All

(f−1) Pastes
→ total f operations.


Time Complexity

Factorization up to √n → O(√n)

Constant space → O(1)

