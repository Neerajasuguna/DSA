✅ Digit character to integer value
char c = '7';
int digit = c - '0';   // 7

✅ Convert digit (0–9) to character
int digit = 5;
char c = (char) (digit + '0');   // '5'



**StringBuilder()**

StringBuilder sb = new StringBuilder("abcde");
Initial value → "abcde"


| Method                      | Example                 | Result         | How / When to Use                                                  |
| --------------------------- | ----------------------- | -------------- | ------------------------------------------------------------------ |
| **`append()`**              | `sb.append("12");`      | `"abcde12"`    | Add characters/numbers while building output (loops, stacks, DSA). |
| **`delete(start, end)`**    | `sb.delete(1, 3);`      | `"ade"`        | Remove characters from index `1` to `2` (end index excluded).      |
| **`deleteCharAt(index)`**   | `sb.deleteCharAt(0);`   | `"bcde"`       | Remove one character (commonly used to remove leading zeros).      |
| **`insert(index, str)`**    | `sb.insert(2, "XY");`   | `"abXYcde"`    | Insert characters at a specific index.                             |
| **`reverse()`**             | `sb.reverse();`         | `"edcba"`      | Reverse string in-place (palindrome, reverse problems).            |
| **`setCharAt(index, ch)`**  | `sb.setCharAt(0, '9');` | `"9bcde"`      | Replace a character efficiently.                                   |
| **`substring(start, end)`** | `sb.substring(1, 4);`   | `"bcd"`        | Extract part as **new `String`** (does NOT modify `sb`).           |
| **`toString()`**            | `sb.toString();`        | `"abcde"`      | Convert to immutable `String` for returning result.                |
| **`length()`**              | `sb.length();`          | `5`            | Get current size (conditions, loops).                              |
| **`capacity()`**            | `sb.capacity();`        | `16` (default) | Internal buffer size (optimization topic).                         |
