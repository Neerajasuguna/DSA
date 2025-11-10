You need to design a data structure that maintains a queue of integers and can efficiently identify the first unique (non-duplicate) integer in the queue.

The FirstUnique class should support three operations:

Constructor FirstUnique(int[] nums): Initialize the data structure with an initial array of integers. These integers form the starting queue.

Method showFirstUnique(): Return the value of the first unique integer in the queue. A unique integer is one that appears exactly once in the entire queue. If no unique integer exists, return -1. The "first" unique integer refers to the one that appears earliest in the queue order.

Method add(int value): Add a new integer to the end of the queue. This may affect which integers are considered unique.





Code 

class FirstUnique {
    // Map to store the frequency count of each number
    private Map<Integer, Integer> frequencyMap = new HashMap<>();
    // LinkedHashSet to maintain unique numbers in insertion order
    private Set<Integer> uniqueNumbers = new LinkedHashSet<>();

    /**
     * Constructor initializes the data structure with an array of numbers.
     * First pass: Count frequency of each number
     * Second pass: Add numbers with frequency 1 to the unique set
     *
     * @param nums Initial array of integers
     */
    public FirstUnique(int[] nums) {
        // Count frequency of each number in the array
        for (int num : nums) {
            frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
        }

        // Add all unique numbers (frequency = 1) to the unique set
        for (int num : nums) {
            if (frequencyMap.get(num) == 1) {
                uniqueNumbers.add(num);
            }
        }
    }

    /**
     * Returns the first unique number from the data structure.
     * If no unique number exists, returns -1.
     *
     * @return The first unique number or -1 if none exists
     */
    public int showFirstUnique() {
        // Return -1 if no unique numbers exist, otherwise return the first element
        return uniqueNumbers.isEmpty() ? -1 : uniqueNumbers.iterator().next();
    }

    /**
     * Adds a new number to the data structure and updates uniqueness.
     * If the number becomes unique (frequency = 1), add it to unique set.
     * If the number is no longer unique (frequency > 1), remove it from unique set.
     *
     * @param value The integer value to add
     */
    public void add(int value) {
        // Update frequency count for the value
        frequencyMap.put(value, frequencyMap.getOrDefault(value, 0) + 1);

        // Check if this is the first occurrence (now unique)
        if (frequencyMap.get(value) == 1) {
            uniqueNumbers.add(value);
        } else {
            // Value is no longer unique, remove from unique set
            uniqueNumbers.remove(value);
        }
    }
}

/**
 * Your FirstUnique object will be instantiated and called as such:
 * FirstUnique obj = new FirstUnique(nums);
 * int param_1 = obj.showFirstUnique();
 * obj.add(value);
 */




Data Structures Used

HashMap<Integer, Integer> frequencyMap
→ Stores count of each number → O(1) average access/update.

LinkedHashSet<Integer> uniqueNumbers
→ Maintains insertion order + allows O(1) add, remove, contains, and iteration (amortized).

⚙️ 1️⃣ Constructor → public FirstUnique(int[] nums)
Steps inside constructor:

First loop:

Traverse nums and count frequencies.

Each put or getOrDefault in HashMap is O(1) average.

So for n elements → O(n).

Second loop:

Traverse again and add numbers with frequency 1 to the LinkedHashSet.

Each add() is O(1).

So → O(n).

✅ Overall Constructor Complexity:

Time: O(n)

Space: O(n)

⚙️ 2️⃣ Method → public int showFirstUnique()
Operations:

uniqueNumbers.isEmpty() → O(1)

uniqueNumbers.iterator().next() → O(1)
(LinkedHashSet internally maintains a linked list of entries, so first element access is constant time.)

✅ Time Complexity: O(1)
✅ Space Complexity: O(1)

⚙️ 3️⃣ Method → public void add(int value)
Steps:

frequencyMap.put(value, frequencyMap.getOrDefault(value, 0) + 1) → O(1)

If new frequency == 1 → uniqueNumbers.add(value) → O(1)

Else → uniqueNumbers.remove(value) → O(1)

✅ Time Complexity: O(1)
✅ Space Complexity: O(1) extra (for one new entry in map/set)

⚙️ Summary Table
Method	Operation	Time Complexity	Space Complexity	Notes
Constructor	Initialize with nums	O(n)	O(n)	Two passes over nums
showFirstUnique()	Get first unique number	O(1)	O(1)	Uses iterator’s first element
add(value)	Add or update number	O(1)	O(1)	Constant time hash ops
🧠 Intuition Summary

LinkedHashSet ensures insertion order (so first element = first unique).

HashMap gives O(1) frequency tracking.

Combining both → fully O(1) operations for add and query.
