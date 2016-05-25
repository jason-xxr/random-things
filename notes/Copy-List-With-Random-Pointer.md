pizza [11:03 PM] 
added a Java snippet 
```java
public class Solution {
    /**
     * @param head: The head of linked list with a random pointer.
     * @return: A new head of a deep copy of the list.
     */
    public RandomListNode copyRandomList(RandomListNode head) {
    
        if (head == null) {
            return null;
        }
        
        RandomListNode dummyCopy = new RandomListNode(0);
        RandomListNode dummy = dummyCopy;
        dummy.next = head;
        while (head  != null ) {
            dummyCopy.next  = new RandomListNode (head.label);
            dummyCopy.next.random = ( head.random == null ?
                                      head.random : 
                                      new RandomListNode (head.random.label));
            dummyCopy.next.next = ( head.next == null ? 
                                      head.next : 
                                      new RandomListNode (head.next.label));
            dummyCopy = dummyCopy.next;
            head = head.next;
        }
        
        return dummy.next;
        
    }
}
```
Add Comment Collapse

pizza [11:03 PM] 
copy-list-with-random-pointer 这题这么做有什么不好吗？

[11:04] 
oj 能 ac 。

Francis Yang [11:15 PM] 
为什么觉得不好？

[11:15] 
return dummyCopy.next?

[11:16] 
你这个return的是原list 好像

pizza [11:49 PM] 
后来觉得不好了。空间耗费太大。每个节点重复地复制。其实只需复制一次。

[11:50] 
还有一个问题。这题在leetcode算hard。这么简单地做不太可能对。

[11:51] 
奇怪的是ac了
