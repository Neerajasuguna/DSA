Problem Description
You need to design a max stack data structure that combines the functionality of a regular stack with the ability to efficiently retrieve and remove the maximum element.

The MaxStack class should support the following operations:

MaxStack(): Initialize an empty stack object.

push(int x): Add element x to the top of the stack.

pop(): Remove and return the element from the top of the stack (most recently added element).

top(): Return the element at the top of the stack without removing it.

peekMax(): Return the maximum element currently in the stack without removing it.

popMax(): Remove and return the maximum element from the stack. If multiple elements have the same maximum value, remove the one that is closest to the top of the stack (the most recently added among the maximum elements).

Performance Requirements:

The top() operation must run in O(1) time
All other operations (push, pop, peekMax, popMax) must run in O(log n) time


✅ Key Requirements
Operation	Time Complexity	Description
push(x)	O(log n)	Push element to stack
pop()	O(log n)	Remove from top
top()	O(1)	Return top
peekMax()	O(log n)	Get maximum
popMax()	O(log n)	Remove most recent maximum
💡 Core Idea

We need both:

Stack order → to implement normal push/pop/top

Max tracking → to get and remove maximum efficiently

Hence we’ll use:

Doubly Linked List (DLL) to represent stack order.

Balanced TreeMap (or multiset / TreeMap<Integer, List<Node>>) to keep track of values for O(log n) max lookup/removal.

⚙️ Data Structures
class Node {
    int val;
    Node prev, next;
    Node(int val) { this.val = val; }
}


DLL (stack):

Keeps track of order for push, pop, top in O(1)

Supports deleting arbitrary node in O(1)

TreeMap<Integer, List<Node>> map:

Key: element value

Value: list of nodes in DLL having that value (for handling duplicates)

Enables:

peekMax() → map.lastKey() in O(log n)

popMax() → remove last node in list for lastKey()

🧠 Algorithm Design
Operation	Implementation Details
push(x)	Create node, insert at tail of DLL, add node reference to TreeMap in O(log n)
pop()	Remove tail node from DLL and TreeMap in O(log n)
top()	Return tail node’s value (O(1))
peekMax()	Return map.lastKey() (O(log n))
popMax()	Get map.lastKey(), remove latest node in its list, unlink from DLL (O(log n))
💻 Java Implementation
import java.util.*;

class MaxStack {
    private static class Node {
        int val;
        Node prev, next;
        Node(int val) { this.val = val; }
    }

    private Node head, tail; // Doubly linked list
    private TreeMap<Integer, List<Node>> map;

    public MaxStack() {
        head = new Node(0);
        tail = new Node(0);
        head.next = tail;
        tail.prev = head;
        map = new TreeMap<>();
    }

    public void push(int x) {
        Node node = new Node(x);
        addNode(node);
        map.computeIfAbsent(x, k -> new ArrayList<>()).add(node);
    }

    public int pop() {
        Node node = tail.prev;
        removeNode(node);
        removeFromMap(node);
        return node.val;
    }

    public int top() {
        return tail.prev.val;
    }

    public int peekMax() {
        return map.lastKey();
    }

    public int popMax() {
        int maxVal = map.lastKey();
        List<Node> nodes = map.get(maxVal);
        Node node = nodes.remove(nodes.size() - 1);
        if (nodes.isEmpty()) map.remove(maxVal);
        removeNode(node);
        return maxVal;
    }

    // helper to add node to DLL tail
    private void addNode(Node node) {
        Node prev = tail.prev;
        prev.next = node;
        node.prev = prev;
        node.next = tail;
        tail.prev = node;
    }

    // helper to remove node from DLL
    private void removeNode(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    // helper to remove node reference from TreeMap
    private void removeFromMap(Node node) {
        List<Node> list = map.get(node.val);
        list.remove(list.size() - 1); // tail node always most recent
        if (list.isEmpty()) map.remove(node.val);
    }
}

🧩 Example Usage
MaxStack st = new MaxStack();
st.push(5);
st.push(1);
st.push(5);
System.out.println(st.top());      // 5
System.out.println(st.popMax());   // 5 (most recent max)
System.out.println(st.top());      // 1
System.out.println(st.peekMax());  // 5
System.out.println(st.pop());      // 1
System.out.println(st.top());      // 5

✅ Complexity Analysis
Operation	Time	Space
push	O(log n)	O(n)
pop	O(log n)	O(n)
top	O(1)	O(1)
peekMax	O(log n)	O(1)
popMax	O(log n)	O(n)


