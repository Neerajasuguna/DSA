# Check If Points Lie on a Straight Line

## 📌 Problem Statement
You are given an array `coordinates`, where each element  
`coordinates[i] = [x, y]` represents a point on a **2D plane**.

Determine whether **all the given points lie on a single straight line**.

---

## ✅ Constraints
- `2 ≤ coordinates.length ≤ 1000`
- `coordinates[i].length == 2`
- `-10⁴ ≤ x, y ≤ 10⁴`
- No duplicate points

---

## 💡 Key Insight
All points lie on a straight line **if the slope between them remains constant**.

Slope between two points:
slope = (y₂ - y₁) / (x₂ - x₁)


However, division fails for **vertical lines** (`x₂ - x₁ = 0`).

👉 To avoid division, we use **cross multiplication**.

---

## 🔁 Cross Multiplication Rule
Instead of:
(y₂ - y₁) / (x₂ - x₁) = (y₃ - y₁) / (x₃ - x₁)

We check:
(y₂ - y₁) * (x₃ - x₁) = (y₃ - y₁) * (x₂ - x₁)


✔ Handles vertical lines  
✔ No floating-point precision issues

---

## 🪜 Step-by-Step Approach

1. Take the **first point** as reference `(x₁, y₁)`
2. Compute reference differences using the second point:


dx1 = x₂ - x₁
dy1 = y₂ - y₁

3. For every remaining point `(xi, yi)`:
- Compute:
  ```
  dx2 = xi - x₁
  dy2 = yi - y₁
  ```
- Check:
  ```
  dy1 * dx2 == dy2 * dx1
  ```
4. If any comparison fails → `false`
5. If all pass → `true`

---
    class Solution {
    public boolean checkStraightLine(int[][] coordinates) {
        int n= coordinates.length;
        int deltaX1= coordinates[1][0]-coordinates[0][0];
        int deltaY1 =coordinates[1][1]-coordinates[0][1];

        for(int i=2;i<n;i++){
            int deltaX2= coordinates[i][0]-coordinates[0][0];
            int deltaY2=coordinates[i][1]-coordinates[0][1];
            if(!((deltaX2*deltaY1)==(deltaY2*deltaX1)))
            return false;
        }
        return true;
    }
    }




  Time: O(n) — one pass through points

Space: O(1) — no extra data structures
