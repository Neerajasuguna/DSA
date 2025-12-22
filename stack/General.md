                 (Interface) Collection
                          |
                          | extends
                          v
                    (Interface) Queue
                 offer() | poll() | peek()
                   O(1)  |  O(1)  |  O(1)
                          |
                          | extends
                          v
                    (Interface) Deque
       addFirst() | addLast() | removeFirst() | removeLast()
           O(1)   |    O(1)   |      O(1)     |      O(1)
                    |                     |
          ---------implements-------------implements---------
                    |                     |
                    v                     v
              (Class) LinkedList      (Class) ArrayDeque
             Queue + Deque             Deque only
        -----------------------------------------------------
        offer()        O(1)            addFirst()     O(1)
        poll()         O(1)            addLast()      O(1)
        peek()         O(1)            removeFirst()  O(1)
        addFirst()     O(1)            removeLast()   O(1)
        addLast()      O(1)            peekFirst()    O(1)
        search()       O(n)            resize()       O(n)*

                          (Interface) Queue
                                  |
                                  | implements
                                  v
                          (Class) PriorityQueue
                    offer()/add()     O(log n)
                    poll()/remove()   O(log n)
                    peek()            O(1)
                    contains()        O(n)




| Method       | Purpose        | Time |
| ------------ | -------------- | ---- |
| `size()`     | Get size       | O(1) |
| `isEmpty()`  | Check empty    | O(1) |
| `contains()` | Search element | O(n) |
| `clear()`    | Remove all     | O(n) |
| `iterator()` | Traverse queue | O(n) |





| Requirement               | Recommended Structure | Reason              |
| ------------------------- | --------------------- | ------------------- |
| Simple FIFO               | `LinkedList`          | O(1) insert/remove  |
| High-performance FIFO     | `ArrayDeque`          | Fastest, no sync    |
| Add/remove at both ends   | `ArrayDeque`          | Deque support       |
| Priority-based processing | `PriorityQueue`       | Heap ordering       |
| Sliding window problems   | `ArrayDeque`          | Front + back ops    |
| BFS / Tree traversal      | `ArrayDeque`          | Cache friendly      |
| Top-K problems            | `PriorityQueue`       | O(log n) ops        |
| Stack replacement         | `ArrayDeque`          | Faster than `Stack` |





Best Way to implement Stack  is using Array Deque 
        **Deque<Integer> stack = new ArrayDeque<>();**
        Queue<Intger>stack =new ArrayDeque<<>() ////// this is wrong as Queue doesnt support peekLast(), pushLast()....
         
        You can implement a stack using either end of Deque:
        
        Top = front → use push() / pop() / peek()
        
        Top = back → use offerLast() / pollLast() / peekLast()
        
        Both give O(1) operations.



        
