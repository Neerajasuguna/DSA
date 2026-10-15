# Reverse Integer

## Problem

Given a signed 32-bit integer `num`, reverse its digits.

If reversing the integer causes the result to go outside the **32-bit signed integer range**:

```text
[-2³¹, 2³¹ - 1]
```

return `0`.

### Examples

| Input | Output | Explanation |
|---:|---:|---|
| `123` | `321` | Reverse the digits |
| `-123` | `-321` | Preserve the negative sign |
| `12000` | `21` | Leading zeros disappear |
| `0` | `0` | No digits to reverse |
| `1534236469` | `0` | Reversed value overflows `int` |

---

## Approach

We reverse the number one digit at a time.

For every iteration:

1. Extract the last digit using `% 10`.
2. Check whether adding this digit to `result` will cause an integer overflow.
3. Add the digit to `result`.
4. Remove the last digit from `num` using `/ 10`.
5. Continue until `num` becomes `0`.

For negative numbers, we first remember the sign and work with the absolute value.

---

## Step-by-Step Example

Consider:

```text
num = 123
```

### Iteration 1

```text
digit = 123 % 10 = 3
num = 123 / 10 = 12

result = 0 * 10 + 3
       = 3
```

### Iteration 2

```text
digit = 12 % 10 = 2
num = 12 / 10 = 1

result = 3 * 10 + 2
       = 32
```

### Iteration 3

```text
digit = 1 % 10 = 1
num = 1 / 10 = 0

result = 32 * 10 + 1
       = 321
```

Final result:

```text
321
```

---

## Handling Negative Numbers

For:

```text
num = -123
```

we first identify the sign:

```java
boolean isNegative = num < 0;
```

Then convert it to its absolute value:

```java
num = -num;
```

The reversal produces:

```text
321
```

Finally, restore the sign:

```java
return isNegative ? -result : result;
```

So:

```text
-123 → -321
```

---

## Overflow Handling

A Java `int` can store values from:

```text
Integer.MIN_VALUE = -2147483648
Integer.MAX_VALUE =  2147483647
```

The important part is that we **must check for overflow before** doing:

```java
result = result * 10 + digit;
```

because the multiplication itself can overflow.

### Overflow condition

We use:

```java
if (result > (INT_MAX - digit) / 10) {
    return 0;
}
```

Why?

We want to perform:

```text
result * 10 + digit
```

without exceeding:

```text
INT_MAX
```

So:

```text
result * 10 + digit <= INT_MAX
```

Rearranging:

```text
result <= (INT_MAX - digit) / 10
```

Therefore, if:

```text
result > (INT_MAX - digit) / 10
```

the next operation would overflow.

---

## Why We Check Before Updating `result`

Consider a value close to the maximum integer:

```text
result = 214748364
digit = 8
```

The next value would be:

```text
214748364 * 10 + 8
= 2147483648
```

But:

```text
Integer.MAX_VALUE = 2147483647
```

Therefore, the result cannot fit inside a Java `int`.

If we perform the calculation first:

```java
result = result * 10 + digit;
```

the `int` may already have overflowed.

That's why the overflow check must happen **before** updating `result`.

---

## Algorithm

```text
1. Store whether num is negative.
2. Convert num to its absolute value.
3. Initialize result = 0.
4. While num != 0:
   a. Extract last digit using num % 10.
   b. Check if result * 10 + digit will overflow.
   c. If overflow occurs, return 0.
   d. Update result = result * 10 + digit.
   e. Remove the last digit using num / 10.
5. Restore the original sign.
6. Return result.
```

---

## Java Solution

```java
public class Solution {

    public static int reverse(int num) {
        // Define the maximum value for a 32-bit signed integer
        int INT_MAX = Integer.MAX_VALUE;  // 2^31 - 1

        // Initialize result to 0, which will store the reversed number
        int result = 0;

        // Check if the number is negative
        boolean isNegative = num < 0;

        // Work with the absolute value of num
        if (isNegative) {
            num = -num;
        }

        // Loop through all digits of num
        while (num != 0) {

            // Extract the last digit
            int digit = num % 10;

            // Check for overflow before updating result
            if (result > (INT_MAX - digit) / 10) {
                return 0;
            }

            // Add digit to the reversed number
            result = result * 10 + digit;

            // Remove the last digit from num
            num /= 10;
        }

        // Restore the original sign
        return isNegative ? -result : result;
    }

    
```

---

## Test Cases

```text
Input: 123
Output: 321
```

```text
Input: -1230
Output: -321
```

```text
Input: 12000
Output: 21
```

```text
Input: 0
Output: 0
```

```text
Input: 1534236469
Output: 0
```

The last case produces `0` because:

```text
9646324351
```

is greater than:

```text
2147483647
```

and therefore cannot be represented by a 32-bit signed integer.

---

## Complexity

Let `n` be the number of digits in the input number.

### Time Complexity

```text
O(log₁₀(n))
```

More precisely, if the number has `d` digits:

```text
O(d)
```

We process every digit exactly once.

### Space Complexity

```text
O(1)
```

Only a few integer variables are used.

---

## Key Interview Takeaways

- Use `% 10` to extract the last digit.
- Use `/ 10` to remove the last digit.
- Use `while (num != 0)` so both positive and negative numbers can be handled.
- **Check overflow before** performing `result * 10 + digit`.
- `Integer.MAX_VALUE` is preferable to memorizing `2147483647`.
- Restore the negative sign after reversing.
- The solution uses constant extra space.
