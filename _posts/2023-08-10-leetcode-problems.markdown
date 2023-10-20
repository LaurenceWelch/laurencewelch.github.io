---
layout: post
title:  "Leetcode Problems"
date:   2023-10-03
categories: jekyll update
---

|![a key](https://images.unsplash.com/photo-1555949963-ff9fe0c870eb?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1470&q=80)|
|:--:|
|*photo by Shahadat Rahman via unsplash*|

# 88. Merge Sorted Array
For this problem we are give two lists: nums1 and nums2 and we need to merge them together. Furthermore, the merged list must be sorted, and we must merge nums2 into nums1, i.e. we cannot return a new list. For this problem, we can create an index representing a current index of the merged list and then iterate backwards, adding either from nums1 or nums2 depending on their value. This index, called `cur` will be equal to the length of nums1 plus the length of nums2 - 1. We also need to store two other indexes, `i1` and `i2`, so that we can track the current indexes of nums1 and nums2 as we pull numbers from them.
```python
def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
    i1 = m - 1
    i2 = n - 1
    cur = m + n - 1
    while i2 >= 0:
        if i1 >= 0 and nums1[i1] > nums2[i2]:
            nums1[cur] = nums1[i1]
            i1 -= 1
        else:
            nums1[cur] = nums2[i2]
            i2 -= 1
        cur -=1
```

# 27. Remove Element
```python
def removeElement(self, nums: List[int], val: int) -> int:
    cur = 0
    for c in range(len(nums)):
        if nums[c] != val:
            nums[cur] = nums[c]
            cur += 1
    return cur
```

# 28. Remove Duplicates from Sorted Array
```python
def removeDuplicates(self, nums: List[int]) -> int:
    dic = {}
    cur = 0
    for c in range(len(nums)):
        if nums[c] not in dic:
            dic[nums[c]] = 1
            nums[cur] = nums[c]
            cur += 1
    return cur
```

# 80. Remove Duplicates from Sorted Array 2
```python
def removeDuplicates(self, nums: List[int]) -> int:
    dic = {}
    cur = 0
    for c in range(len(nums)):
        if nums[c] not in dic:
            nums[cur] = nums[c]
            cur += 1
            dic[nums[c]] = 1
        else:
            if dic[nums[c]] == 1:
                nums[cur] = nums[c]
                cur += 1
                dic[nums[c]] += 1
    return cur
```

# 169. Majority Element
```python
def majorityElement(self, nums: List[int]) -> int:
    dic = {}
    goal = len(nums) * 0.5
    for num in nums:
        if num in dic:
            dic[num] += 1
        else:
            dic[num] = 1
        if num in dic and dic[num] > goal:
            return num
```

# 189. Rotate Array
```python
def rotate(self, nums: List[int], k: int) -> None:
    dic = {}
    length = len(nums)
    for c in range(length):
        dic[c] = nums[c]
        target = (c - k + length) % length
        if target in dic:
            nums[c] = dic[target]
        else:
            nums[c] = nums[target]
```

# 121. Best Time to Buy and Sell Stock
```python
def maxProfit(self, prices: List[int]) -> int:
    highestp = 0
    lowestb = prices[0]
    dic = {}
    for c in range(1, len(prices)):
        cur = prices[c]
        profit = cur - lowestb
        if profit > highestp:
            highestp = profit
        if cur < lowestb:
            lowestb = cur
    return highestp
```

# 135. Candy
```python
class Solution:
    def candy(self, ratings: List[int]) -> int:
        lenList = len(ratings)
        #exit conditions
        if lenList == 0: return 0
        if lenList == 1: return 1

        candies = [1] * lenList
        
        for c in range(1, lenList):
            prev = ratings[c - 1]
            curr = ratings[c]
            if curr > prev:
                candies[c] = candies[c - 1] + 1

        for c in range(lenList - 2, -1, -1):
            prev = ratings[c + 1]
            curr = ratings[c]
            if curr > prev and candies[c] <= candies[c + 1]:
                candies[c] = candies[c + 1] + 1

        return sum(candies)
```

# 13. Roman to Integer
```python
class Solution:
    def romanToInt(self, s: str) -> int:
        numerals = {}
        numerals["I"] = 1
        numerals["V"] = 5
        numerals["X"] = 10
        numerals["L"] = 50
        numerals["C"] = 100
        numerals["D"] = 500
        numerals["M"] = 1000
        result = 0
        c = 0
        #start
        while c < len(s):
            ch = s[c]
            #look ahead
            if c != len(s) - 1 and numerals[ch] < numerals[s[c + 1]]:
                add = numerals[s[c + 1]] - numerals[ch]
                result += add
                c += 1
            else: 
                result += numerals[ch]
            c += 1

        return result
```
       
# 125. Valid Palindrome
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        s = re.sub("[^a-zA-Z0-9]", "", s)
        s = s.lower()
        if len(s) <= 1: return True
        for i in range(0, int(len(s) * 0.5)):
            if s[i] != s[-i - 1]: return False
        return True
```        

# 383. Ransom Note
```python
def fun(x, d):
    if x in d: d[x] += 1
    else: d[x] = 1

class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        rd = {}
        md = {}
        for c in ransomNote:
            fun(c, rd)
        for c in magazine:
            fun(c, md)
        for key in rd:
            if key in md and md[key] >= rd[key]:
                pass
            else: 
                return False
        return True 
```

# 228. Summary Ranges
```python
class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        if len(nums) == 0: return []
        ranges = []
        prevVal = nums[0]
        chain = str(prevVal)
        chainLen = 1
        for c in range(1, len(nums)):
            currVal = nums[c]
            if currVal == prevVal + 1:
                #continue the chain
                chainLen += 1
                prevVal = currVal
            else:
                #end the chain
                if chainLen > 1:
                    chain += "->" + str(prevVal)
                    ranges.append(chain)
                else:
                    ranges.append(str(prevVal))
                prevVal = currVal
                chainLen = 1
                chain = str(currVal)
        #finish the list
        if chainLen > 1:
            chain += "->" + str(currVal)
        ranges.append(chain)
        return ranges
```

# 141. Linked List Cycle
This problem gives a linked list. Sometimes the linked list wraps around itself and becomes infinite, other times, the linked list ends normally. We need to discover if the linked list wraps around, but how can we do that? 
Think about a high school track team running laps. Let's say you have a slow runner and a fast runner. The fast runner would end up looping around and passing the slow runner multiple times. On the other hand, if they were running a race from the parking lot to the principal's office, they would never see each other. The fast runner would simply finish the race and then head to the cafeteria for some pasta carbonara. So, all we have to do, is make two virtual runners traversing through the list. If they meet, we know that it's a loop.
First, we need to account for two possible exit conditions, the list is null, or the list has no next. In both of these situations the list doesn't loop. Then, we need to create two runners, cur1 and cur2. Cur2 is faster and we need to give cur2 a headstart so that they don't meet up right away. Thus cur1 (slower runner) will be set to head and cur2 (faster runner) will be set to head->next. Then, we loop through the list making sure that cur1 is moving once per iteration and cur2 is moving twice per iteration. If they ever meet up, it meens cur2 looped around and caught up to cur1: in other words, the list is looping.
```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        #exit conditions
        if not head: return False
        if not head.next: return False
        #initial cur1 and cur2
        cur1 = head
        cur2 = head.next
        while True:
            #pace of 1 for cur1
            if cur1.next:
                cur1 = cur1.next
            else: return False
            #pace of 2 for cur2
            for _ in range(2):
                if cur2.next:
                    cur2 = cur2.next
                else:
                    return False
            #if cur2 catches cur1, it's looping
            if cur1 is cur2:
                return True     
        return True
```

# 136. Single Number
For this problem we are given a list of numbers. Every number in the list appears twice except for one special number that appears only once, we need to find that special number. Below is an example list.
```
[1, 1, 2, 3, 3]
```
Two is the special number because it appears only once, every other number appears twice. How can we find the special number?
One way to solve this problem is to iterate over a sorted version of the list and look for numbers which aren't doubled. Before we create a loop, we need to check for exit conditions. The list can have a length of 1, in which case the special number is simply the only number. We also need to create some variables: one to store the previous number, and one to store the current count.
There are two possible situations as we loop through the list: one, the current number is equal to the previous number, and two, the current number does not equal the previous number. If the numbers are equal, we know, it's not the special number, we can increment the counter and look at the next number. If they aren't equal, we need to check the counter. If the counter does not equal two, it means that we passed over the special number, we can simply return that special number. If the counter is equal to two, we passed over a grouping of two numbers and now we have a new number which might possibly be the special number, this case we need to reset the counter to 1 and keep looping.
```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        #exit condition
        ll = len(nums)
        if ll == 1: return nums[0]
        #start
        nums.sort()
        pre = nums[0]
        counter = 1
        #main loop
        for i in range(1, ll):
            cur = nums[i]
            if cur != pre: 
                if counter != 2: return pre
                counter = 1
            else:
                counter = 2
            pre = cur
        return nums[-1]
```

This alternate version uses the set function. A set is different from a list in that a set can only contain unique elements. let's look at our previous example list.
```
[1, 1, 2, 3, 3]
```
Imagine two more lists, one list has one of each element and the other has two of each element.
```
[1, 2, 3]
[1, 1, 2, 2, 3, 3]
```
What would be the sums of each of these lists? The sum of the first list is 6. The sum of the second list is 12, exactly double of the first list. Makes sense because the second list has two of each element. Another way to think about this is shown below.
```
2 * sum([1, 2, 3]) == sum([1, 1, 2, 2, 3, 3])
```
Now to put this concept into action let's replace the second list with the original list.
```
2 * sum([1, 2, 3]) == sum([1, 1, 2, 3, 3]) + 2
```
or
```
2 * sum([1, 2, 3)] - sum([1, 1, 2, 3, 3]) = 2
```
With this knowledge, we can start coding.

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        #exit condition
        ll = len(nums)
        if ll == 1: return nums[0]
        #start
        setSum = sum(set(nums))
        listSum = sum(nums)
        return 2 * setSum - listSum
```

# 66. Plus One
For this problem we are given a list of digits that represent a number. `[1, 2, 3, 4]` for example would represent the number 1234. We need to add 1 to this one number and then return a list of digits representing the new number.
One way to solve this problem is to: convert the digits list into one number, then add 1 to this number, then convert this number back into a list. We can use the join() method which will transform a list into a string, except to use join, we need a list of strings, not integers. To convert a value into a list, we can use list(), although the value must be a string, not an integer.
```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        # str(x) for x in digits is used to cast each digit in digits into a string
        # "".join() is used join this list of strings into a single string
        # int() is then used to convert that single string into an integer
        num = int("".join(str(x) for x in digits))

        # add 1 to that number
        num += 1

        # str() converts the number into a string, we do this so that we can split it
        # list() is used to split the string into a list of string digits
        # finally, [int(x) for x in list(str(num))] is a list comprehension which creates a list.
        # the list will be created, executing the int() function on each string in list(str(num))
        return [int(x) for x in list(str(num))]
```

# 70. Climbing Stairs
For this problem we are given the following rules: a staircase has `n` steps, we can either go up 2 steps at a time or 1 step at a time. Give a staircase with `n` steps, how many different ways can we go up the staircase.
If n is 1, there is only 1 way.
```
[1]
```
if n is 2, there are two ways.
```
[1, 1] or [2]
```
if n is 3, there are three ways.
```
[1, 1, 1] or [1, 2] or [2, 1]
```
If we continue this, we can see that the number of ways is equal to the the sum of the previous two ways: answer(n) = answer(n - 1) + answer(n -2). Therefore, we can use a loop to calculate the answers of incrementing n, and then get the next answer by adding the previous answers together.
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        # known solutions:
        if n <= 3: return n
        # track previous values
        prev1 = 1
        prev2 = 2
        # loop through range(n)
        # stop at n - 2 because we are essentially starting at n = 3
        for i in range(n - 2):
            # next value is sum of previous values
            val = prev1 + prev2
            # keep track of prev
            prev1 = prev2
            prev2 = val

        # done looping, return sum
        return prev2
```

# 67. Add Binary
```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        d = { '0': 0, '1': 1 }
        s = ''
        i = -1
        carry = 0
        # loop from end to start
        while True:
            # check i to see if we are done looping
            if -i > len(a) and -i > len(b):
                if carry == 1: s += '1'
                return s[::-1]
            # get next values from a and b
            av = d[a[i]] if len(a) >= -i else 0
            bv = d[b[i]] if len(b) >= -i else 0
            # perform addition
            add = av + bv + carry
            carry = 0
            # add to str, set carry if needed
            if add == 0: 
                s += '0'
            elif add == 1: 
                s += '1'
            elif add == 2: 
                s += '0'
                carry = 1
            else:
                s += '1'
                carry = 1
            i -= 1
        # return    
        return ''
```

# 58. Length of Last Word
For this problem we need to find the length of the last word in a sentence. Python makes this one easy. We can use the `split()` method, which by default, splits a string by spaces into a list. Then, we can take the `[-1]` or last index of the list and return the `len()` of it.
```python
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
        return len(s.split()[-1])
```

# 28. Find the Index of first Occ. in Str
This problem asks us to find the index of the first appearance of a str in another str. We can utilize the `index()` method to find the index, however, since `index()` can throw an error, we can first check if the index exists using `needle in haystack`.
```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if needle in haystack:
            return haystack.index(needle)
        return -1
```

# 392. Is Subsequence
This problem asks us to determine if a string s is a subsequence of string t. S is a subsequence of t if some or none of t's characters can be removed to result in s being a substring of t. So basically we want to check if it is possible to remove 'junk characters' from t so that s is in t. One way to do this is to iterate through s, checking at each loop if the character from s is in t. The moment we find a character from s that is not in t, we know it's not a subsequence and we can return False. If an iteration succeeds, we can continue, but we need to ensure that the character in t that matched, can't match again, so we need to keep track of its index and search from beyond that index.
```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        toSearch = t
        for c in s:
            if c not in toSearch:
                return False
            index = toSearch.index(c)
            toSearch = toSearch[index + 1:]
        return True
```

# 205. Isomorphic Strings
For this problem we need to see if two strings, s and t are isomorphic. S and t are isomorphic if characters in s can be translated to a different character to get t. However, no two characters can map to the same character and same characters must map to a specific character. For this problem we can use two dictionaries. One dict. is used to keep track of mappings, i.e. 'a' is mapped to '[some character]'. Another dictionary is used to make sure that mappings don't overlap, for example both 'a' and 'b' can't be mapped to 'c'. We can iterate through the characters of s, updating our dictionaries and checking if any issues occur, in which we return False.
```python
class Solution:
    def isIsomorphic(self, s: str, t: str) -> bool:
        d = {}
        alphaD = {}
        for i in range(len(s)):
            sc = s[i]; tc = t[i]
            #sc is in d, it's value better match tc
            if sc in d:
                if d[sc] != tc: return False
            #it's not in d, let's add it, making sure it's not in alphaD
            #i.e. we don't want two keys mapping to the same value
            else:
                if tc in alphaD: return False
                else: alphaD[tc] = True
                d[sc] = tc
        return True
```

# 202. Happy Number
For this problem, we have to determine if a number if a happy number. Imagine we have the number 19. Square each of the digits (1 and 9) and add them together and we get 82. Now add the square of those digits (8 and 2) and we get 68. Repeat this process and if we eventually get 1, the number is happy. On the other hand, if the process loops forever without ever getting 1, it's not a happy number. How can we detect an endless loop? We can keep a dictionary of results, if we get a sum that's already in the dictionary, we can assume that a loop is happening and we can return False.
```python
class Solution:
    def isHappy(self, n: int) -> bool:
        d = {}
        num = n
        while True:
            newNum = 0
            for c in str(num):
                newNum += int(c) * int(c)
            if newNum == 1: return True
            if newNum in d:
                return False
            else:
                d[newNum] = True  
            num = newNum 
```

# 21. Merge Two Sorted Lists
For this problem we need to merge two linked list which are sorted. We need to ensure that our returned list is also sorted. To solve this problem we can make a new list. Each iteration we can check list1 and list2, take the lower value, and append it to our new linked list. To make sure we aren't repeating ourselves, after appending a node, we have to increment the list in which we pulled a value from. After this, we have to check if their are any leftover nodes in either list and if so, append them to the end of our new list.
```python
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        #create a new head
        newHead = ListNode()
        cur = newHead
        #iterate through lists appending the lower value
        while list1 and list2:
            if list1.val < list2.val:
                cur.next = list1
                cur = cur.next
                list1 = list1.next
            else:
                cur.next = list2
                cur = cur.next
                list2 = list2.next
        #check for leftovers
        if list1: cur.next = list1
        elif list2: cur.next = list2
        return newHead.next
```

# 104. Maximum Depth of Binary Tree
For this problem we are given a binary search tree and we need to determine it's maximum depth. For this problem we can use a breadth-first search. A bfs searches each child before moving to the child nodes as opposed to depth-first search which goes as far as possible until it hits a null node. 
We also need a dictionary to store each nodes current depth. Using this dictionary, when we find a new child node, we can set that nodes depth to parent depth + 1. bfs uses a queue (first-in first-out), to store nodes. So as we discover nodes, we append them to the queue, and the earliest node will be considered next. Everytime we find a child node, we can check if it's depth is deeper than the current max depth.
```python
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        #exit condition
        if root == None: return 0
        d = {} #store depths of each node
        d[root] = 1
        maxDepth = 1
        queue = [root]
        #start Breadth-First Search
        while len(queue) != 0:
            v = queue.pop(0)
            for node in [v.left, v.right]:
                if node:
                    nodeDepth = d[v] + 1
                    d[node] = nodeDepth
                    maxDepth = max(maxDepth, nodeDepth)
                    queue.append(node)                
        return maxDepth
```

# 100. Same Tree
For this problem we are given two binary search trees and we have to determine if they are equal, ie they have the same structure and each node has the same value. Similarily to the previous problem, we can use a bfs to traverse the trees and compare each node. I made a helper function to reduce clutter when comparing nodes. After all, nodes can be considered equal if they have the same value or if they are both null (None).
```python
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        
        #return 0 if both null, 1 if both equal, -1 if not equal
        def isEqual(n1, n2):
            if n1 and n2 and n1.val == n2.val: return 1
            if n1 is None and n2 is None: return 0
            return -1

        #exit condition
        result = isEqual(p, q)
        if result == -1: return False
        if result == 0: return True
        #create queues
        pq = [p]
        qq = [q]
        #bfs
        while len(pq) != 0 and len(qq) != 0:
            pv = pq.pop(0)
            qv = qq.pop(0)
            #p children
            pc = [pv.left, pv.right]
            #q children
            qc = [qv.left, qv.right]
            #loop over children
            for i in range(2):
                c1 = pc[i]
                c2 = qc[i]
                result = isEqual(c1, c2)
                if result == -1: return False
                if result == 1:
                    pq.append(c1)
                    qq.append(c2)
        return True
```

# 701. Insert into a Binary Search Tree
This problem asks us to insert a new node into a binary search tree. We can solve this problem by looping through the tree until we reach a null (None) node. If the number we are inserting is lower, we go to the left child and if the number is higher we go to the right child. We also need to store the true root of the tree so that we can return it, and changing previous node so that we can append our new node to this previous node.
```python
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        #exit condition
        if not root: return TreeNode(val)
        #store original root
        trueRoot = root
        #store previous node
        prev = None
        #loop through tree until None is found
        while root:
            if val < root.val:
                prev = root
                root = root.left
            else:
                prev = root
                root = root.right
        #after loop, insert the new node
        if val < prev.val:
            prev.left = TreeNode(val)
        else:
            prev.right = TreeNode(val)
        #return the stored root node
        return trueRoot
```

# 226. Invert Binary Tree
For this problem we need to invert a binary tree. By invert, they mean that if you were to traverse the tree in order, the values of be descending. For example if you have a binary search tree with a root of 2 and children with values of 1 and 3, the inverted tree should have a root of 2 with children of values 3 and 1. One way to do this is to use a Breadth-First Search and simply swap the left and right children. The bfs algorithm goes row by row, so we can simply swap the children as we descend down.
```python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        #exit condition
        if root == None: return None
        #bfs so we use a queue
        q = [root]
        while len(q) != 0:
            v = q.pop(0)
            #swap left and right
            temp = v.left
            v.left = v.right
            v.right = temp
            for node in [v.left, v.right]:
                if node: q.append(node)
        return root
```

# 2415. Reverse Odd Levels of Binary Tree
This problem is weird. We are given a binary tree. Each odd depth of the tree we have to reverse the node values. It is important that our reversing doesn't effect the even depth nodes at all.
First, I tried reversing odd level depths. Problem there is that it upsets the even ones too. Then I tried reversing the value of odd level depths. No Dice. As the tree goes, it wasn't truly reversing the entirety of the odd rows; ie. if the row was something like 8, 13, 21, 34, it wasn't giving me the correct row of 34, 21, 13, 8. Eventually, I got a working solution that used one dictionary to store the depth of each node and another dictionary to store arrays of values that I could pull from to reverse the odd depths. However, it was confusing, arcane, and non-satisfying. I wanted a cleaner solution.
As the minutes passed a certain frustartion started boiling inside of me. I turned to the internet and finally figured it out. What we need is a bfs, but it needs to be rows by row. Typically, a bfs loop is node by node not row by row. Here is a typical bfs algorithm.
```python
def bfs(root):
    q = [root]
    while q:
        v = q.pop(0)
        print(v.val)
        for node in [v.left, v.right]:
            if node: q.append(node)
```
Each iteration is going to look at one node. Here's how a depth-level bfs looks like.
```python
def rdfs(root):
    q = [root]
    while q:
        for _ in range(len(q)):
            v = q.pop(0)
            print(v.val)
            for node in [v.left, v.right]:
                if node: q.append(node)
```
Looks pretty similar doesn't it? The major difference is the nested `for` loop in the `while q` loop. Each iteration of `while q` will print one row of the tree. With that in mind, we can create our solution.
```python
class Solution:
    def reverseOddLevels(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        #exit condition
        if root == None: return None
        #bfs queue
        q = [root]
        #track current depth
        depth = 0
        while q:
            #if current depth is odd, flip the node values
            if depth % 2 == 1:
                l = 0
                r = len(q) - 1
                while l < r:
                    temp = q[l].val
                    q[l].val = q[r].val
                    q[r].val = temp
                    l += 1
                    r -= 1
            #bfs but go row by row
            for _ in range(len(q)):
                v = q.pop(0)
                for node in [v.left, v.right]:
                    if node: q.append(node)
            #increment depth
            depth += 1
        return root
```

# 11. Path Sum
Another Binary Tree problem! This one wants us to see if there is a path, from root to leaf, that has a sum equal to a given number. We can't skip nodes or stop before reaching a leaf. How can we solve this? First I thought a DFS would solve this. There probably is a DFS to solve it but I couldn't figure it out because there's a problem. DFS goes from root to leaf, but then it goes to the next leaf, i.e. it doesn't start from root to leaf each time. How can we know what the current sum is when we have to go backwards? It gets messy fast. Then, I thought of BFS. With BFS I can calculate row by row and I won't have to think about going backwards. Specifically I decided to use the row by row version of BFS as explained in the previous problem.
The only addition I need is a dictionary. The dictionary will use nodes as keys and each key-value pair will store the sum of the previous nodes. Every time I visit a node, I will check if it's a leaf. If so, I can check the sum and if it equals the desired sum, I can return True. When adding nodes to the queue, I can also update the sum stored in the dictionary.
```python
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root: return False
        q = [root]
        #dictionary to sum prev nodes sum
        d = {root: 0}
        while q:
            for _ in range(len(q)):
                v = q.pop(0)
                #it's a leaf, test the sum
                if not v.left and not v.right:
                    if v.val + d[v] == targetSum:
                        return True
                for node in [v.left, v.right]:
                    if node:
                        #append and make sure to store current sum
                        d[node] = v.val + d[v]
                        q.append(node)
        return False
```

# 637. Average of Levels in Binary Tree
Time for another row based BFS! For this one, we can do our row based BFS as explained in the previous two problems. Each level, we need to add the values and then average them and append it to an array.
```python
class Solution:
    def averageOfLevels(self, root: Optional[TreeNode]) -> List[float]:
        q = [root]
        result = []
        while q:
            #count and sum
            c = 0
            s = 0
            for _ in range(len(q)):
                v = q.pop(0)
                c += 1
                s += v.val
                for node in [v.left, v.right]:
                    if node: q.append(node)
            result.append(s / c)
        return result
```

# 530. Minimum Absolute Difference
For this problem, we need to find the minimum difference between any two nodes in a Binary search tree. How can we find this? If we were to view the BST in order, i.e. something like: 1, 4, 7, 8, we can clearly see that the smallest difference is 1 because 8 - 7 = 1. So, let's view the BST in order using in-order traversal! We also need to keep track of the previous nodes value so that we can calculate the difference as well as the current lowest difference which we will return at the end of the algorithm. I set the lowest difference variable to a very high number, so that we don't accidently return the wrong number. I set the previous value to a very low number because in the first iteration, we don't actually have a prev value, so we have to make one up.
```python
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        stack = []
        #start high as to not corrupt results
        lowestDiff = 100000
        #start low as to not corrupt results
        prevVal = -100000
        #algorithm is based off of in-order traversal
        while stack or root:
            if root:
                stack.append(root)
                root = root.left
            else:
                v = stack.pop()
                #calc difference, then update previous node value
                diff = v.val - prevVal
                if diff < lowestDiff:
                    lowestDiff = diff
                prevVal = v.val
                if v.right:
                    root = v.right
        return lowestDiff
```
