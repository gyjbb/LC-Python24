# LC-Python24
Backtracking 4

## 93.Restore IP Addresses, 78.Subsets, 90.Subsets II

June 17, 2023  4h

Congratulations!\
This is the 24st day for leetcode python study. Today we will learn more about backtracking!\
The challenges today are about using backtracking as a kind of loop solution. For each loop, the number of possible choices of elements /size of one set defines the width of the tree, and the number of loops we need to have/k defines the depth of the tree, we use recursion to simplify the multiple loops. Don't forget the backtracking process followed!\
So for K elements, the tree's depth is k, because we choose one element in each layer.\
The palindrome is a hard question!

##  93.Restore IP Addresses
[Reading link](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0093.%E5%A4%8D%E5%8E%9FIP%E5%9C%B0%E5%9D%80.md)\
[video](https://www.bilibili.com/video/BV1XP4y1U73i/?spm_id_from=333.788&vd_source=63f26efad0d35bcbb0de794512ac21f3)\
[leetcode](https://leetcode.com/problems/restore-ip-addresses/)\
This is a hard question too.\
isValid function is to decide if a subdtring is valid for IP address. Only when the cutted substring is valid, we proceed to the next recursion/next level of tree.
```python
# ways 1: recursion+backtracking
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        result = []
        self.backtracking(s, 0, 0, "", result)
        return result
    
    def backtracking(self, s, start_index, point_num, current, result):
        if point_num == 3:      #end the string cutting
            if self.is_valid(s, start_index, len(s)-1):     
                #check if the fourth sunstring is valid
                current += s[start_index:]      #add the fourth substring
                result.append(current)
            return

        for i in range(start_index, len(s)):
            if self.is_valid(s, start_index, i):     
            #check if the [start_index, i] substring is valid
                sub = s[start_index:i + 1]
                self.backtracking(s, i+1, point_num+1, current + sub + '.', result)
            else: 
                break
    
    def is_valid(self, s, start, end):
        if start > end:
            return False
        if s[start] =='0' and start != end:     #the beginning of a substring can't be 0
            return False
        num = 0
        for i in range(start, end+1):
            if not s[i].isdigit():
                return False
            num = num*10+int(s[i])
            if num>255:
                return False
        return True
```


## 78.Subsets
[leetcode](https://leetcode.com/problems/subsets/)\
Here we need to collect the results in every tree node, not just leaf nodes. The results lie in every layer of recursion. The process is very similar to recursion and backtracking.\
The termination condition here is startIndex equals or is larger than the size of given numbers, which means we have reached at the leaf nodes.
```python
# ways 1: 
class Solution:
    def subsets(self, nums):
        result = []
        path = []
        self.backtracking(nums, 0, path, result)
        return result

    def backtracking(self, nums, startIndex, path, result):
        result.append(path[:])  # collect the result, put this line before the termination condition! Otherwise the leaf nodes will be emitted.
        if startIndex >= len(nums):  # termination condition
            return
        for i in range(startIndex, len(nums)):
            path.append(nums[i])
            self.backtracking(nums, i + 1, path, result)
            path.pop()
```


## 90.Subsets II
[leetcode](https://leetcode.com/problems/subsets-ii/)\
Compared to the last question, the set given may contain duplicated items. This will ccause dupliactes in the results, which is not required. \
Then how to **remove duplicates**? Here we define a **used array** to record the processed items. \
Just need to remove duplicates in the same tree layer/tree width, while duplicates in the tree branch/depth is allowed.
```python
# Ways1: remove duplicates by used array.
class Solution:
    def subsetsWithDup(self, nums):
        result = []
        path = []
        used = [False] * len(nums)
        nums.sort()  # 去重需要排序
        self.backtracking(nums, 0, used, path, result)
        return result

    def backtracking(self, nums, startIndex, used, path, result):
        result.append(path[:])  # 收集子集
        for i in range(startIndex, len(nums)):
            # used[i - 1] == True，说明同一树枝 nums[i - 1] 使用过
            # used[i - 1] == False，说明同一树层 nums[i - 1] 使用过
            # 而我们要对同一树层使用过的元素进行跳过
            if i > 0 and nums[i] == nums[i - 1] and not used[i - 1]:
                continue
            path.append(nums[i])
            used[i] = True
            self.backtracking(nums, i + 1, used, path, result)
            used[i] = False
            path.pop()
```




