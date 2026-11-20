Check if Two Strings are Anagrams – Java Solution
Problem: Given two strings s and t, determine whether they are anagrams of each other.

Two strings are anagrams if they contain the same characters with the same frequency, irrespective of order.

Example:
Input: s = "listen", t = "silent"
Output: true

Input: s = "hello", t = "world"
Output: false


Approach 1: Using Character Frequency Array (Optimal)
If the input contains only lowercase English letters, use an array of size 26.

Java Code:

    class Solution {
      public boolean isAnagram(String s, String t) {

        if (s.length() != t.length()) {
            return false;
        }

        int[] freq = new int[26];

        for (int i = 0; i < s.length(); i++) {
            freq[s.charAt(i) - 'a']++;
            freq[t.charAt(i) - 'a']--;
        }

        for (int count : freq) {
            if (count != 0) {
                return false;
            }
        }

        return true;
    }
    }



Dry Run
Input:
s = "anagram"
t = "nagaram"


We maintain a frequency array.
Character	Increment from s	Decrement from t
a	+1	-1
n	+1	-1
a	+1	-1
g	+1	-1
r	+1	-1
a	+1	-1
m	+1	-1

All character frequencies become zero.
Output: true
Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(1), because the array always has 26 elements.
Approach 2: Using HashMap
Useful when the input contains a broader range of characters.

import java.util.*;

    class Solution {
    public boolean isAnagram(String s, String t) {

        if (s.length() != t.length()) {
            return false;
        }

        Map<Character, Integer> map = new HashMap<>();

        for (char ch : s.toCharArray()) {
            map.put(ch, map.getOrDefault(ch, 0) + 1);
        }

        for (char ch : t.toCharArray()) {
            if (!map.containsKey(ch)) {
                return false;
            }

            map.put(ch, map.get(ch) - 1);

            if (map.get(ch) == 0) {
                map.remove(ch);
            }
        }

        return map.isEmpty();
    }
    }



- Time Complexity: O(n) on average.
- Space Complexity: O(k), where k is the number of distinct characters.
Approach 3: Using Sorting

import java.util.Arrays;

    class Solution {
    public boolean isAnagram(String s, String t) {

        if (s.length() != t.length()) {
            return false;
        }

        char[] a = s.toCharArray();
        char[] b = t.toCharArray();

        Arrays.sort(a);
        Arrays.sort(b);

        return Arrays.equals(a, b);
    }
    }



- Time Complexity: O(n log n).
- Space Complexity: Depends on sorting implementation and input representation.

Which approach should you use in interviews?
Approach	When to use
Frequency Array	Lowercase English letters – best choice
HashMap	General character frequency
Sorting	Simple implementation
