**AllOne Data Structure – O(1) Design (String + Count)**
Problem

**Design a data structure that supports the following operations in O(1) time:**

inc(String key)

Inserts a new key with value 1 if it doesn’t exist.

Otherwise, increments its value by 1.

dec(String key)

If key’s value is 1, remove it from the data structure.

Otherwise, decrement its value by 1.

If the key does not exist, do nothing.

getMaxKey()

Returns one key with the maximum value.

If no element exists, return "".

getMinKey()

Returns one key with the minimum value.

If no element exists, return "".

All operations must run in O(1) average time.

Approach

We need:

O(1) update of counts for a key

O(1) access to min and max count keys

**Data Structures**

**Doubly Linked List of Buckets**

**Each bucket represents a count and holds all keys having that count.**

**head <-> [count=1: {k1, k2}] <-> [count=2: {k3}] <-> [count=5: {k4, k5}] <-> tail**


Each bucket node:

int count

Set<String> keys

Bucket prev, next

The DLL is sorted by count increasing:

head.next → bucket with minimum count

tail.prev → bucket with maximum count

HashMap<String, Bucket> keyBucket

Maps each key to its current bucket node.

Allows us to find and move a key in O(1).

Key Operations (High Level)

**inc(key):**

If key is new:

Place it into count 1 bucket (create if needed after head).

If key exists in bucket with count c:

Move it to bucket with count c+1 (create if needed next to current).

Remove from old bucket; delete bucket if empty.

**dec(key):**

If key doesn’t exist → do nothing.

If key has count 1:

Remove key completely from map and bucket.

If key has count c > 1:

Move it to bucket with count c-1 (create if needed before current).

If old bucket becomes empty → remove bucket.

**getMaxKey():**

If no bucket → return "".

Else, return any key from tail.prev.keys.

**getMinKey():**

If no bucket → return "".

Else, return any key from head.next.keys.

All of these are O(1) because:

DLL operations (insert/remove/move) are O(1)

HashMap lookups/updates are O(1)

Bucket creation/removal is O(1)

Java Implementation
import java.util.*;

public class AllOne {

    private static class Bucket {
        int count;
        Set<String> keys;
        Bucket prev, next;

        Bucket(int count) {
            this.count = count;
            this.keys = new HashSet<>();
        }
    }

    private Bucket head, tail; // sentinel nodes
    private Map<String, Bucket> keyBucket;

    public AllOne() {
        head = new Bucket(0);
        tail = new Bucket(0);
        head.next = tail;
        tail.prev = head;
        keyBucket = new HashMap<>();
    }

    public void inc(String key) {
        if (!keyBucket.containsKey(key)) {
            // New key with count 1
            Bucket first = head.next;
            if (first == tail || first.count > 1) {
                // Need a new bucket with count 1 after head
                Bucket newBucket = new Bucket(1);
                insertAfter(head, newBucket);
                first = newBucket;
            }
            first.keys.add(key);
            keyBucket.put(key, first);
        } else {
            // Existing key: move from count c to c+1
            Bucket curBucket = keyBucket.get(key);
            int newCount = curBucket.count + 1;
            Bucket nextBucket = curBucket.next;

            if (nextBucket == tail || nextBucket.count > newCount) {
                // Need a new bucket with newCount after curBucket
                Bucket newBucket = new Bucket(newCount);
                insertAfter(curBucket, newBucket);
                nextBucket = newBucket;
            }

            // Move key
            nextBucket.keys.add(key);
            keyBucket.put(key, nextBucket);

            curBucket.keys.remove(key);
            if (curBucket.keys.isEmpty()) {
                removeBucket(curBucket);
            }
        }
    }

    public void dec(String key) {
        Bucket curBucket = keyBucket.get(key);
        if (curBucket == null) {
            // Key doesn't exist
            return;
        }

        int curCount = curBucket.count;
        curBucket.keys.remove(key);

        if (curCount == 1) {
            // Remove key entirely
            keyBucket.remove(key);
        } else {
            int newCount = curCount - 1;
            Bucket prevBucket = curBucket.prev;

            if (prevBucket == head || prevBucket.count < newCount) {
                // Need a new bucket with newCount before curBucket
                Bucket newBucket = new Bucket(newCount);
                insertAfter(prevBucket, newBucket);
                prevBucket = newBucket;
            }

            prevBucket.keys.add(key);
            keyBucket.put(key, prevBucket);
        }

        if (curBucket.keys.isEmpty()) {
            removeBucket(curBucket);
        }
    }

    public String getMaxKey() {
        if (tail.prev == head) {
            return "";
        }
        Bucket maxBucket = tail.prev;
        // return any key
        return maxBucket.keys.iterator().next();
    }

    public String getMinKey() {
        if (head.next == tail) {
            return "";
        }
        Bucket minBucket = head.next;
        // return any key
        return minBucket.keys.iterator().next();
    }

    // Insert newBucket right after prev
    private void insertAfter(Bucket prev, Bucket newBucket) {
        Bucket next = prev.next;
        prev.next = newBucket;
        newBucket.prev = prev;
        newBucket.next = next;
        next.prev = newBucket;
    }

    // Remove bucket from the doubly linked list
    private void removeBucket(Bucket bucket) {
        Bucket prev = bucket.prev;
        Bucket next = bucket.next;
        prev.next = next;
        next.prev = prev;
        // keys set will be GC'd when no references left
    }
}

Complexity
Operation	Time
inc	O(1)
dec	O(1)
getMaxKey	O(1)
getMinKey	O(1)
