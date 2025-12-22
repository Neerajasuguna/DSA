**A monotonic stack keeps elements in increasing or decreasing order so that each element is pushed and popped only once → O(n).
**


When to use Monotonic Stack Technique ?
In problems where we need to process

          Next greater Element
          Next smaller Element
          Previous greater Element
          Previous smaller Element
          Lexicographically Smallest/Greatest
          Histogram Related Problems left and right boundaries for each bar



🔑 ONE GOLDEN RULE (Memorize This) to find something ----> foloww this 
👉 Next → traverse from RIGHT
👉 Previous → traverse from LEFT
👉 Greater → pop smaller
👉 Smaller → pop greater




Next Greater Element

      public int[] nextGreater(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    Stack<Integer> st = new Stack<>();

    for (int i = n - 1; i >= 0; i--) {
        while (!st.isEmpty() && st.peek() <= nums[i]) {
            st.pop();
        }
        res[i] = st.isEmpty() ? -1 : st.peek();
        st.push(nums[i]);
    }
    return res;
    }



Next Smaller Element

        public int[] nextSmaller(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    Stack<Integer> st = new Stack<>();

    for (int i = n - 1; i >= 0; i--) {
        while (!st.isEmpty() && st.peek() >= nums[i]) {
            st.pop();
        }
        res[i] = st.isEmpty() ? -1 : st.peek();
        st.push(nums[i]);
    }
    return res;
    }

Previous Greater Element (PGE)




    public int[] prevGreater(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    Stack<Integer> st = new Stack<>();

    for (int i = 0; i < n; i++) {
        while (!st.isEmpty() && st.peek() <= nums[i]) {
            st.pop();
        }
        res[i] = st.isEmpty() ? -1 : st.peek();
        st.push(nums[i]);
    }
    return res;
    }




Previous Smaller Element (PSE)


      public int[] prevSmaller(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    Stack<Integer> st = new Stack<>();

    for (int i = 0; i < n; i++) {
        while (!st.isEmpty() && st.peek() >= nums[i]) {
            st.pop();
        }
        res[i] = st.isEmpty() ? -1 : st.peek();
        st.push(nums[i]);
    }
    return res;
    }


**Notes: **

1️⃣ Value stack vs Index stack

Use value stack → NGE, NSE

Use index stack → histogram, width, range

2️⃣ Duplicates handling

Use <= or >= carefully









