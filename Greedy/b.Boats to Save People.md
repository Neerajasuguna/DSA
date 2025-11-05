A big ship with numerous passengers is sinking, and there is a need to evacuate these people with the minimum number of life-saving boats. Each boat can carry, at most, two persons however, 
the weight of the people cannot exceed the carrying weight limit of the boat.

We are given an array, people, where people[i] is the weight of the ith

person, and an infinite number of boats, where each boat can carry a maximum weight, limit. Each boat carries, at most, two people at the same time.
This is provided that the sum of the weight of these people is under or equal to the weight limit.

You need to return the minimum number of boats to carry all persons in the array.



Approach 

To solve the problem, we can use the greedy pattern and pair people with the lightest and heaviest people available, as long as their combined weight does not exceed the weight limit. If the combined weight exceeds the limit, we can only send one person on that boat. This approach ensures that we use the minimum number of boats to rescue the people.

The steps to implement the approach above are given below:

**Sort the people array.
Initialize two pointers—left at the start and right at the end of the array.
Iterate over the people array while the left pointer is less than or equal to the right pointer.
Check if both the lightest and heaviest persons can fit in one boat. If so, increment the left pointer and decrement the right pointer.
Otherwise, rescue the heaviest person alone and decrement the right pointer.
Increment the boats after each rescue operation.**


Time complexity
The time complexity for the solution is
O(nlog⁡ n)
, since sorting the people array takes 
O(nlog⁡ n)
 time.

Space complexity
In Java, the sorting algorithm takes 

O(n)
 space to sort the people array. Therefore, the space complexity of the solution above is 

O(n)
.


**
Solution **

 public static int rescueBoats(int[] people, int limit) {
        
        Arrays.sort(people);
        
        int left = 0; 
        int right = people.length - 1;

        int boats = 0; 

        while (left <= right) {
            if (people[left] + people[right] <= limit) {
                left++;
            }
            right--;

            boats++;
        }
        return boats;
    }


