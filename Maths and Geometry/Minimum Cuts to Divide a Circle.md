
Given an integer n, determine the minimum number of cuts required to divide the circle into n equal slices. A valid cut in a circle is defined as one of the following:

A cut is represented by a straight line that passes through the circle’s center and touches two points on its edge.

A cut is represented by a straight line touching one point on the circle’s edge and center.



Algorithm 

To divide a circle into **n slices**, the number of cuts required depends on the value of **n**. If **n = 1**, no cuts are needed because the circle already consists of a single slice. When **n > 1**, the approach changes based on whether **n** is even or odd.  

If **n is even**, the circle can be divided into **n slices** by making **n / 2 cuts**, where each cut passes through the center of the circle. Since each such cut splits the circle into two equal parts, every cut increases the number of slices by two, resulting in **n / 2 cuts** for **n** slices.  

If **n is odd**, the circle cannot be evenly divided using center cuts alone. In this case, each cut produces exactly one new slice, so to obtain **n slices**, we must make **n cuts**.  

Mathematically, let **Cuts(n)** represent the minimum number of cuts required to divide the circle into **n slices**:

Cuts(n) =  
0, if n = 1  
n, if n > 1 and n is odd  
n / 2, if n > 1 and n is even  

This formula ensures the minimum number of cuts needed for any value of **n**.


    public class Solution {

    public static int numberOfCuts(int n) {
        // If there is only one slice, no cuts are needed
        if (n == 1)
            return 0;

        // If n is odd, the number of cuts equals n
        if (n % 2 == 1)
            return n;

        // If n is even, the number of cuts is half of n
        return n / 2;
    }


Time Complexity - O(1) 
Sapce Complexity = O(1)
