# 双指针技巧秒杀链表题目

## [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummy = new ListNode();
        ListNode cur = dummy;
        while(list1 != null && list2 != null){
            if(list1.val < list2.val){
                cur.next = list1;
                list1 = list1.next;
                cur = cur.next;
            }else{
                cur.next = list2;
                list2 = list2.next;
                cur = cur.next;
            }
        }
        if(list1 != null) cur.next = list1;
        else cur.next = list2;
        return dummy.next;
    }
}
```

经常有读者问我，什么时候需要用虚拟头结点？我这里总结下：**当你需要创造一条新链表的时候，可以使用虚拟头结点简化边界情况的处理**。

比如说，让你把两条有序链表合并成一条新的有序链表，是不是要创造一条新链表？再比你想把一条链表分解成两条链表，是不是也在创造新链表？这些情况都可以使用虚拟头结点简化边界情况的处理。

## [86. 分隔链表](https://leetcode.cn/problems/partition-list/)

```java
class Solution {
    public ListNode partition(ListNode head, int x) {
        ListNode dummy1 = new ListNode();
        ListNode cur1 = dummy1;
        ListNode dummy2 = new ListNode();
        ListNode cur2 = dummy2;

        while(head != null){
            if(head.val < x){
                cur1.next = head;
                cur1 = cur1.next;
                head = head.next;
            }else{
                cur2.next = head;
                cur2 = cur2.next;
                head = head.next;
            }
        }
        cur1.next = dummy2.next;
        cur2.next = null;//记得断开防止成环！！！
        return dummy1.next;
    }
}
```

## [23. 合并 K 个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists == null || lists.length == 0) return null;
        return mergeKListsIndex(lists,0,lists.length-1);
    }

     public ListNode mergeKListsIndex(ListNode[] lists,int left,int right){
        if(left == right) return lists[left];
        int mid = left + (right-left)/2;
        ListNode a = mergeKListsIndex(lists,left,mid);
        ListNode b = mergeKListsIndex(lists,mid+1,right);
        return merge(a,b);
     }

    public ListNode merge(ListNode left,ListNode right){
        ListNode dummy = new ListNode();
        ListNode cur = dummy;
        while(left != null && right != null){
            if(left.val < right.val){
                cur.next = left;
                left = left.next;
            }else{
                cur.next = right;
                right = right.next;
            }
            cur = cur.next;
        }
        if(left != null) cur.next = left;
        else cur.next = right;
        
        return dummy.next;
    }

}
```

## [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode();
        dummy.next = head;
        ListNode left = dummy;
        ListNode right = dummy;

        for(int i = 0; i < n; i++)
            right = right.next;

        while(right.next != null){
            left = left.next;
            right = right.next;
        }
        
        left.next = left.next.next;
        return dummy.next;
    }
}
```

## [876. 链表的中间结点](https://leetcode.cn/problems/middle-of-the-linked-list/)

```java
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode left = head;
        ListNode right = head;
        while(right != null && right.next != null){
            left = left.next;
            right = right.next.next;
        }
        return left;
    }
}
```

## [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode left = head;
        ListNode right = head;
        while(right != null && right.next != null){
            left = left.next;
            right = right.next.next;
            if(left == right) return true;
        }
        return false;
    }
}
```



## [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode left = head;
        ListNode right = head;
        while(right != null && right.next != null){
            left = left.next;
            right = right.next.next;
            if(left == right) break;
        }
        if(right == null || right.next == null) return null;
        left = head;
        while(left != right){
            left = left.next;
            right = right.next;
        }
        return left;
    }
}
```

## [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int lenA = 0,lenB = 0;
        ListNode p = headA,q = headB;
        while(p != null){
            lenA++;
            p = p.next;
        }
        while(q != null){
            lenB++;
            q = q.next;
        }

        p = headA;
        q = headB;
        if(lenA > lenB){
            for(int i = 0; i < lenA-lenB; i++)
                p = p.next;
        }else{
            for(int i = 0; i < lenB-lenA; i++)
                q = q.next; 
        }
        //如果两条链表不相交，他俩同时走到尾部空指针
        while(p != q){
            p = p.next;
            q = q.next;
        }

        return p;
    }
}
```

# 双指针技巧秒杀数组题目

## [26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int left = 0;
        for(int right = 1; right < nums.length; right++){
            if(nums[right] != nums[left]){
                left++;
                nums[left] = nums[right];
            }
        }
        return left+1;
    }
}
```

## [83. 删除排序链表中的重复元素](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/)

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head == null) return null;
        ListNode p = head;
        while(p.next != null){
            if(p.val == p.next.val)
                p.next = p.next.next;
            else
                p = p.next;
        }
        return head;
    }
}
```

## [27. 移除元素](https://leetcode.cn/problems/remove-element/)

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int left = 0;
        for(int right = 0; right < nums.length; right++){
            if(nums[right] != val){
                nums[left] = nums[right];
                left++;
            }
        }
        return left;
    }
}
```

## [283. 移动零](https://leetcode.cn/problems/move-zeroes/)

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int left = 0;
        for(int right = 0; right < nums.length; right++){
            if(nums[right] != 0){
                nums[left] = nums[right];
                left++;
            }
        }
        for(;left < nums.length; left++)
            nums[left] = 0;
    }
}
```

# 滑动窗口

## [76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)

```java
class Solution {
    public String minWindow(String s, String t) {
        Map<Character,Integer> need = new HashMap<>();
        Map<Character,Integer> window = new HashMap<>();

        for(int i = 0; i < t.length(); i++){
            char c = t.charAt(i);
            need.put(c,need.getOrDefault(c,0)+1);
        }
        
        int cur = 0;
        int start = 0,end = Integer.MAX_VALUE;

        int left = 0;
        for(int right = 0; right < s.length(); right++){
            char c = s.charAt(right);
            if(need.containsKey(c)){
                window.put(c,window.getOrDefault(c,0)+1);
                if(need.get(c).equals(window.get(c)))//!!!小心
                    cur++;
            }
            while(cur == need.size()){
                if(right-left < end-start){
                    start = left;
                    end = right;
                }
                c = s.charAt(left);
                if(window.containsKey(c)){
                    window.put(c,window.get(c)-1);
                    if(window.get(c) < need.get(c))
                        cur--;
                }
                left++;
            }
        }
        return end == Integer.MAX_VALUE ? "" : s.substring(start,end+1);
    }
}
```

## [567. 字符串的排列](https://leetcode.cn/problems/permutation-in-string/)

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        Map<Character,Integer> window = new HashMap<>();
        Map<Character,Integer> need = new HashMap<>();

        for(char c : s1.toCharArray()){
            need.put(c,need.getOrDefault(c,0)+1);
        }

        int cur = 0;
        int left = 0;
        for(int right = 0; right < s2.length(); right++){
            char c = s2.charAt(right);
            if(need.containsKey(c)){
                window.put(c,window.getOrDefault(c,0)+1);
                if(window.get(c).equals(need.get(c))){
                    cur++;
                }
            }
            while(cur == need.size()){
                if(right-left+1 == s1.length()) return true;
                c = s2.charAt(left);
                if(window.containsKey(c)){
                    window.put(c,window.get(c)-1);
                    if(window.get(c) < need.get(c))
                        cur--;
                }
                left++;
            }
        }
        return false;
    }
}
```

## [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        Map<Character,Integer> need = new HashMap<>();
        Map<Character,Integer> window = new HashMap<>();
        List<Integer> list = new ArrayList<>();

        for(char c : p.toCharArray())
            need.put(c,need.getOrDefault(c,0)+1);

        int cur = 0;
        int left = 0;
        for(int right = 0; right < s.length(); right++){
            char c = s.charAt(right);
            if(need.containsKey(c)){
                window.put(c,window.getOrDefault(c,0)+1);
                if(window.get(c).equals(need.get(c)))
                    cur++;
            }

            while(cur == need.size()){
                if(right-left+1 == p.length()){
                    list.add(left);
                }
                    
                c = s.charAt(left);
                if(window.containsKey(c)){
                    window.put(c,window.get(c)-1);
                    if(window.get(c) < need.get(c))
                        cur--;
                }
                left++;
            }

        }
        return list;
    }
}
```

## [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> window = new HashMap<>();

        int max = 0;
        int left = 0;
        for(int right = 0; right < s.length(); right++){
            char c = s.charAt(right);
            if(!window.containsKey(c) || window.get(c) < left){
                max = Math.max(max,right-left+1);
            }else{
                left = window.get(c)+1;
            }
            window.put(c,right);
        }
        return max;
    }
}
```

# 二分搜索算法核心代码模板





















































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































