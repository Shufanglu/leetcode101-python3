# 双指针

1. 算法解释
    - 双指针主要用于遍历数组，两个指针指向不同的元素，从而协同完成任务。也可以延伸到多 个数组的多个指针。
    - 若两个指针指向同一数组，遍历方向相同且不会相交，则也称为滑动窗口(两个指针包围的 区域即为当前的窗口)，经常用于区间搜索。
    - 若两个指针指向同一数组，但是遍历方向相反，则可以用来进行搜索，待搜索的数组往往是 排好序的。

## 案例一 Two Sum    （两个指针指向数组中不同的元素）

167. Two Sum II - Input array is sorted (Easy) <https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/>

Given a 1-indexed array of integers numbers that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number. Let these two numbers be numbers[index1] and numbers[index2] where 1 <= index1 < index2 <= numbers.length.

Return the indices of the two numbers, index1 and index2, added by one as an integer array [index1, index2] of length 2.

The tests are generated such that there is exactly one solution. You may not use the same element twice.

 

Example 1:

Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].


Example 2:

Input: numbers = [2,3,4], target = 6
Output: [1,3]
Explanation: The sum of 2 and 4 is 6. Therefore index1 = 1, index2 = 3. We return [1, 3].


Example 3:

Input: numbers = [-1,0], target = -1
Output: [1,2]
Explanation: The sum of -1 and 0 is -1. Therefore index1 = 1, index2 = 2. We return [1, 2].




```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        l = 0
        r = len(numbers)-1
        
        while l<r:
            if (numbers[l]+numbers[r])==target:
                return [l+1, r+1]
            elif (numbers[l]+numbers[r])>target:
                r-=1
            else:
                l+=1
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-1-dcc8f837688e> in <module>
    ----> 1 class Solution:
          2     def twoSum(self, numbers: List[int], target: int) -> List[int]:
          3         l = 0
          4         r = len(numbers)-1
          5 


    <ipython-input-1-dcc8f837688e> in Solution()
          1 class Solution:
    ----> 2     def twoSum(self, numbers: List[int], target: int) -> List[int]:
          3         l = 0
          4         r = len(numbers)-1
          5 


    NameError: name 'List' is not defined


633. Sum of Square Numbers (medium) <https://leetcode.com/problems/sum-of-square-numbers/>
    
    Given a non-negative integer c, decide whether there're two integers a and b such that a2 + b2 = c.

Example 1:

Input: c = 5
Output: true
Explanation: 1 * 1 + 2 * 2 = 5
    
Example 2:

Input: c = 3
Output: false
    
    
Example 3:

Input: c = 4
Output: true
    
Example 4:

Input: c = 2
Output: true
    
Example 5:

Input: c = 1
Output: true


```python
class Solution:
    def judgeSquareSum(self, c: int) -> bool:
        import math
        l = 0
        r = int(math.sqrt(c))+1 #缩小循环的搜索范围，如果r=c， 会exceed time limits
        while l<=r:
            if l*l+r*r<c:
                l+=1
            elif l*l+r*r>c:
                r-=1
            else:
                return True
            
        return False
```

680. Valid Palindrome II (Easy) <https://leetcode.com/problems/valid-palindrome-ii/>

Given a string s, return true if the s can be palindrome after deleting at most one character from it.

 
Example 1:

Input: s = "aba"
Output: true

Example 2:

Input: s = "abca"
Output: true
Explanation: You could delete the character 'c'.

Example 3:

Input: s = "abc"
Output: false

***题解：首尾两个指针，一旦遇到不相等，就计算去掉头的部分或者去掉尾的部分是否是回文， python里面回文可以用逆向切片判断s==s[::-1] ***



```python
class Solution:
    def validPalindrome(self, s: str) -> bool:
        l = 0
        r = len(s)-1
        while l<r:
            if s[l] == s[r]:
                l+=1
                r-=1
            else:
                a = s[l:r]
                b = s[l+1: r+1]
                return a==a[::-1] or b==b[::-1]
                
        return True
        
```

## 案例二 归并两个有序数组       （多个数组的多个指针）

88. Merge Sorted Array (Easy) <https://leetcode.com/problems/merge-sorted-array/>
You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

 

Example 1:

Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.


Example 2:

Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].


Example 3:

Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.


***Follow up: Can you come up with an algorithm that runs in O(m + n) time?***


```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        pos = len(nums1)-1
        for i in range(len(nums1)-1, -1,-1):
            if m>0 and n>0:
                if nums1[m-1]<=nums2[n-1]:
                    nums1[pos] = nums2[n-1]
                    n-=1
                else:
                    nums1[pos] = nums1[m-1]
                    m-=1
                pos-=1
            
            if m==0:
                nums1[:n]=nums2[:n]
            # if n==0:
            #     nums1[:m]
```

524. Longest Word in Dictionary through Deleting (Medium)  <https://leetcode.com/problems/longest-word-in-dictionary-through-deleting/>

Given a string s and a string array dictionary, return the longest string in the dictionary that can be formed by deleting some of the given string characters. If there is more than one possible result, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.

Example 1:

Input: s = "abpcplea", dictionary = ["ale","apple","monkey","plea"]
Output: "apple"


Example 2:

Input: s = "abpcplea", dictionary = ["a","b","c"]
Output: "a"

***题解： 对于dictionary中的每一个word ，去判断是否满足题目的要求，并且记录最长的单词或者索引。 其中word和s比较的时候设置两个指针，如果word里面每个字符能遍历完，说明符合条件***



```python
class Solution:
    def findLongestWord(self, s: str, dictionary: List[str]) -> str:
        dictionary.sort()
        temp = 0
        maxword=''
        for word in dictionary:
            i=j=0
            while i<len(s) and j <len(word):
                if s[i] == word[j]:
                    j+=1
                i+=1
            
            if j==len(word) and j>temp:
                    maxword = word
                    temp = j
          
                
        return maxword
    
```

## 案例三 快慢指针 
142. Linked List Cycle II (Medium) <https://leetcode.com/problems/linked-list-cycle-ii/>
Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return null.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to (0-indexed). It is -1 if there is no cycle. Note that pos is not passed as a parameter.

Do not modify the linked list.

 

Example 1:


Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
Example 2:


Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
Example 3:


Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.


***解释***

题目： 给定一个链表，如果有环路，找出环路的开始点。

题解： 对于链表找环路的问题，有一个通用的解法——快慢指针(Floyd 判圈法)。给定两个指针， 分别命名为 slow 和 fast，起始位置在链表的开头。每次 fast 前进两步，slow 前进一步。如果 fast 可以走到尽头，那么说明没有环路;如果 fast 可以无限走下去，那么说明一定有环路，且一定存在一个时刻 slow 和 fast 相遇。当 slow 和 fast 第一次相遇时，我们将 fast 重新移动到链表开头，并 让 slow 和 fast 每次都前进一步。当 slow 和 fast 第二次相遇时，相遇的节点即为环路的开始点。


```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        
        if not head or not head.next:
            return None
        
        #solution 1 利用python的set数据结构 
        visited = set()
        while head and head.next:
            if head in visited:
                return head
            else:
                visited.add(head)
                head = head.next
        return None
            
                
                
                
                
        #solution2 快慢指针        
        if not head or not head.next:
            return None
        #step1: identify whether the linked list has a cycle or not 
        slow = fast = head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            
            #step 2: if there exists a cycle, return the node where the cycle begins 
            if fast==slow:
                # while head: 这样写会错过head和slow相等的情况 
                #     head = head.next
                #     slow = slow.next
                #     if head==slow:
                #         return head
                fast = head
                while fast!=slow: #这个循环条件非常重要 
                    fast = fast.next
                    slow = slow.next
                return fast
                    
        return None
                
        
```

## 案例四 滑动窗口  （未完成）
76. Minimum Window Substring (Hard)
340. Longest Substring with At Most K Distinct Characters (Hard)
