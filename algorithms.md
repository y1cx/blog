---
title: Data Structures and Algorithms
date: 2022-12-17 10:34:21
categories: Exercises
tags: LeetCode
description: Exercises from LeetCode
---

- Arrays

Q1. Two Sum

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        d = {}
        for i, j in enumerate(nums):
            r = target - j
            if r in d: return [d[r], i]
            d[j] = i
```

Q811. Subdomain Visit Count

```python
class Solution:
    def subdomainVisits(self, cpdomains: List[str]) -> List[str]:
        d = {}
        for i in cpdomains:
            n, domains = i.split()
            n, domains = int(n), domains.split('.')
            for j in range(len(domains)):
                _str = '.'.join(domains[j:])
                d[_str] = d[_str] + n if _str in d else n
        return [str(d[i]) + ' ' + i for i in d ]
```

Q56. Merge Intervals

```Python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        out = []
        for i in sorted(intervals, key=lambda i: i[0]):
            if out and i[0] <= out[-1][1]:
                out[-1][1] = max(out[-1][1], i[1])
            else:
                out += [i]
        return out
```

Q2225. Find Players With Zero or One Losses

```Python
class Solution:
    def findWinners(self, matches: List[List[int]]) -> List[List[int]]:
        n = max(chain(*matches))+1
        players = [0] * n
        for w, l in matches:
            if players[w]>=0: players[w]=1      
            if players[l]<0: players[l]-=1    
            if players[l]>=0: players[l]=-1    
        
        lostZero,lostOne = [],[]
        for i in range(n):
            if players[i] == 1:
                lostZero.append(i)
            elif players[i] == -1:
                lostOne.append(i)
        
        return [lostZero,lostOne]
```

- Recursion

Q273. Integer to English Words

```Python
class Solution:
    def numberToWords(self, num: int) -> str:
        to19 = 'One Two Three Four Five Six Seven Eight Nine Ten Eleven Twelve ' \
           'Thirteen Fourteen Fifteen Sixteen Seventeen Eighteen Nineteen'.split()
        tens = 'Twenty Thirty Forty Fifty Sixty Seventy Eighty Ninety'.split()
        thousand = 'Thousand Million Billion'.split()
        
        def word(num, idx=0):
            if num == 0:
                return []
            if num < 20:
                return [to19[num-1]]
            if num < 100:
                return [tens[num//10-2]] + word(num%10)
            if num < 1000:
                return [to19[num//100-1]] + ['Hundred'] + word(num%100)

            p, r = num//1000, num%1000
            space = [thousand[idx]] if p % 1000 !=0 else []
            return  word(p, idx+1) + space + word(r)
        return ' '.join(word(num)) or 'Zero'
```

- Dynamic Programming

动态规划参考：https://zhuanlan.zhihu.com/p/91582909

Q一维：一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

```
if n <= 2:
    return n;

dp[0] = 0;
dp[1] = 1;
dp[2] = 2;

for (i = 3; i <= n; i++) {
    dp[i] = dp[i-1] + dp[i-2];
}
```

Q二维：一个机器人位于一个 m x n 网格的左上角，机器人每次只能向下或者向右移动一步，机器人试图达到网格的右下角。问总共有多少条不同的路径？

```
# 空间复杂度O(n * m)
if (m <= 0 || n <= 0) {
    return 0;
}

for(int i = 0; i < m; i++){
  dp[i][0] = 1;
}
for(int i = 0; i < n; i++){
  dp[0][i] = 1;
}

for (int i = 1; i < m; i++) {
    for (int j = 1; j < n; j++) {
        dp[i][j] = dp[i-1][j] + dp[i][j-1];
    }
}
return dp[m-1][n-1];

# 空间复杂度O(n)
if (m <= 0 || n <= 0) {
    return 0;
}

for(int i = 0; i < n; i++){
  dp[i] = 1;
}

for (int i = 1; i < m; i++) {
    dp[0] = 1;
    for (int j = 1; j < n; j++) {
        dp[j] = dp[j-1] + dp[j];
    }
}
return dp[n-1];
```

Q最优：给定一个包含非负整数的 m x n 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

```
if (m <= 0 || n <= 0) {
    return 0;
}

dp[0][0] = arr[0][0];

for(int i = 1; i < m; i++){
  dp[i][0] = dp[i-1][0] + arr[i][0];
}

for(int i = 1; i < n; i++){
  dp[0][i] = dp[0][i-1] + arr[0][i];
}

for (int i = 1; i < m; i++) {
    for (int j = 1; j < n; j++) {
        dp[i][j] = Math.min(dp[i-1][j], dp[i][j-1]) + arr[i][j];
    }
}
return dp[m-1][n-1];
```

Q编辑距离
```
# 空间复杂度O(n*m)
for (int j = 1; j <= n2; j++) 
    dp[0][j] = dp[0][j - 1] + 1;

for (int i = 1; i <= n1; i++) dp[i][0] = dp[i - 1][0] + 1;

for (int i = 1; i <= n1; i++) {
    for (int j = 1; j <= n2; j++) {

        if (word1.charAt(i - 1) == word2.charAt(j - 1)){
            p[i][j] = dp[i - 1][j - 1];
        }else {
           dp[i][j] = Math.min(Math.min(dp[i - 1][j - 1], dp[i][j - 1]), dp[i - 1][j]) + 1;
        }         
    }
}
return dp[n1][n2];  

# 空间复杂度O(n)
for (int j = 0; j <= n; j++) 
    dp[j] = j;

for (int j = 0; j <= n2; j++) 
    dp[j] = j;

for (int i = 1; i <= n1; i++) {
    int temp = dp[0];
    dp[0] = i;
    
    for (int j = 1; j <= n; j++) {
        int pre = temp;
        temp = dp[j];

        if (word1.charAt(i - 1) == word2.charAt(j - 1)){
            dp[j] = pre;
        }else {
           dp[j] = Math.min(Math.min(dp[j - 1], pre), dp[j]) + 1;
        }      
    }
}
return dp[n]; 
```

Q718. Maximum Length of Repeated Subarray

```Python
# Time Limit Exceed
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        a = 0
        for i in range(len(nums1)-1):
            for j in range(i+1,len(nums1)+1):
                if ('#'+'#'.join([str(x) for x in nums1[i:j]])+'#') in ('#'+'#'.join([str(x) for x in nums2])+'#'):
                    if len(nums1[i:j]) > a:
                        a = len(nums1[i:j])
        return a

# Classic longest common substring problem
# dp[i][j] is the longest common suffix between nums1[0..i-1] and nums2[0..j-1]
# Time O(m * n), Space O(m * n)
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        m, n = len(nums1), len(nums2)
        dp = [[0] * (n+1) for _ in range(m+1)] # Create a 2D array with m+1 rows and n+1 columns, each element in the list is initialized to 0.
        ans = 0
        for i in range(1, m+1):
            for j in range(1, n+1):
                if nums1[i-1] == nums2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = 0
                ans = max(ans, dp[i][j])
        return ans

# Time O(m * n), Space O(n)
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        if len(nums1) < len(nums2): 
            return self.findLength(nums2, nums1)
        # Make sure len(nums1) > len(nums2) to optimize space
        m, n = len(nums1), len(nums2)
        dp, prevDP = [0] * (n+1), [0] * (n+1)
        ans = 0
        for i in range(1, m+1):
            for j in range(1, n+1):
                if nums1[i-1] == nums2[j-1]:
                    dp[j] = prevDP[j-1] + 1
                else:
                    dp[j] = 0
                ans = max(ans, dp[j])
            dp, prevDP = prevDP, dp
        return ans
```

- Tree
- Graph
- Topological sort 拓扑排序 有向无环图

```
L = Empty list that will contain the sorted elements
S = Set of all nodes with no incoming edge

while S is non-empty do
    remove a node n from S
    add n to tail of L
    for each node m with an edge e from n to m do
        remove edge e from the graph
        if m has no other incoming edges then
            insert m into S

if graph has edges then
    return error   (graph has at least one cycle)
else 
    return L   (a topologically sorted order)
```

Q207. Course Schedule

Return true if you can finish all courses. Otherwise, return false.

```Python
# Algorithm: BFS Topological Sorting
# Time: O(E + V)
# Space: O(E + V)
class Solution(object):
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        if numCourses <= 0:
            return False
        graph = {i : [] for i in range(numCourses)}
        inDegree = {i : 0 for i in range(numCourses)}
        # graph = defaultdict(list)
        # graph = [[] for _ in range(numCourses)]
        # inDegree = [0] * numCourses
        # inDegree = [0 for _ in range(numCourses)]
        
        for child, parent in prerequisites:
            graph[parent].append(child)
            inDegree[child] += 1

        queue = deque()
        
        for key in inDegree:
            if inDegree[key] == 0:
                queue.append(key)
        # for i in range(numCourses):
        #    if inDegree[i] == 0:
        #        queue.append(i)

        visited = 0    

        while queue:
            vertex = queue.popleft()
            visited += 1
            for child in graph[vertex]:
                inDegree[child] -= 1
                if inDegree[child] == 0:
                    queue.append(child)
        
        return visited == numCourses

class Solution:

    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        graph = [[] for _ in range(numCourses)]
        # graph = collections.defaultdict(list)
        indegree = [0, ] * numCourses
        # inDegree = [0 for _ in range(numCourses)]

        for child, parent in prerequisites:
            # for every prerequisite parent, it's needed before take it's value in dict
            graph[parent].append(child)
            # number of courses need to take before take child
            indegree[child] += 1

        # index of courses that can be taken without any other prerequisites
        queue = collections.deque(v for v in range(numCourses) 
                                  if indegree[v] == 0)
        n = len(queue)

        while queue and n != numCourses: 
            # adding n != numCourses to terminate loop earlier
            v = queue.popleft()
            # these courses become the prerequisites of other courses
            # loop through the courses that need it as a prerequisite
            for to_ in graph[v]:
                # decrement it in 'indegree'
                indegree[child] -= 1
                # if there is no other prerequisite needed
                if indegree[child] == 0:
                    n += 1
                    # append it to bfs, for further searching
                    queue.append(child)
        # if every course, as a node, is visited, then it's stored in bfs
        # so return the length of bfs equaling to numCourses                   
        return n == numCourses
```

Q208. Course Schedule II

Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.

```
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        if numCourses <= 0:
            return False
        graph = {i : [] for i in range(numCourses)}
        inDegree = {i : 0 for i in range(numCourses)}
        
        for child, parent in prerequisites:
            graph[parent].append(child)
            inDegree[child] += 1

        queue = deque()
        
        for key in inDegree:
            if inDegree[key] == 0:
                queue.append(key)
        
        ans = []

        while queue:
            vertex = queue.popleft()
            ans.append(vertex)
            for child in graph[vertex]:
                inDegree[child] -= 1
                if inDegree[child] == 0:
                    queue.append(child)
        
        return ans if len(ans) == numCourses else []
```

Q630. Course Schedule III

Return the maximum number of courses that you can take.

```Python
class Solution:
    def scheduleCourse(self, courses: List[List[int]]) -> int:
        course = []
        # Sort all courses by end time
        for i in sorted(courses, key = lambda x:x[1]):
            course.append(i[0])
            while sum(course) > i[1]:
                course.remove(max(course))
        return len(course)
```

Q1462. Course Schedule IV

Return if the ancestor is correct.

```Python
import collections
class Solution:
    def checkIfPrerequisite(self, n: int, prerequisites: List[List[int]], queries: List[List[int]]) -> List[bool]:
        graph = collections.defaultdict(list)
        in_degree = [0] * n
        pres = [set() for _ in range(n)]

        for pre, course in prerequisites:
            graph[pre].append(course)
            in_degree[course] += 1
            pres[course].add(pre)

        queue = collections.deque(course for course, degree in enumerate(in_degree) if degree == 0)

        while queue:
            pre = queue.popleft()
            for course in graph[pre]:
                pres[course] |= pres[pre]
                in_degree[course] -= 1
                if in_degree[course] == 0:
                    queue.append(course)
        return [pre in pres[course] for pre, course in queries]
```

Q2192. All Ancestors of a Node in a Directed Acyclic Graph

```Python
class Solution:
    def getAncestors(self, n: int, edges: List[List[int]]) -> List[List[int]]:

        graph = defaultdict(set)
        inDegree = [0] * n
        ans = [set() for _ in range(n)]
        
        for parent, child in edges:
            graph[parent].add(child)
            inDegree[child] += 1
            ans[child].add(parent)

        queue = deque([u for u, degree in enumerate(inDegree) if degree == 0])
        
        while queue:
            parent = queue.popleft()
            for child in graph[parent]:
                ans[child].update(ans[parent])
                inDegree[child] -= 1
                if inDegree[child] == 0:
                    queue.append(child)
        
        return [sorted(s) for s in  ans]
```

- Time / Space Complexity
- System Design / Object Oriented Design

- Codility

Binary Gap: This function works by iterating through the binary representation of n and identifying the positions of the ones. Whenever a one is encountered, the function calculates the length of the gap between the current position and the previous one (if there is a previous one) and adds it to the gap_lengths list. Finally, the function returns the maximum value in the gap_lengths list, or 0 if the list is empty (i.e. there are no binary gaps).

```Python
def binary_gaps(n):
    binary = bin(n)[2:]
    gap_lengths = []
    gap_start = None
    
    for i in range(len(binary)):
        if binary[i] == '1':
            if gap_start is not None:
                gap_lengths.append(i - gap_start - 1)
            gap_start = i
    
    return max(gap_lengths) if gap_lengths else 0
```

Given an array A of N integers, returns the smallest positive integer (greater than 0) that does not occur in A.

Note: The function first removes all non-positive integers from the array A and then sorts it. It then checks if 1 is missing, in which case it returns 1. If 1 is present, it iterates over the sorted array and checks for missing integers. If it finds a missing integer between two consecutive elements, it returns the value of the missing integer. If all integers are present, it returns the next integer after the largest integer in the array.

```Python
def solution(A):
    # Implement your solution here
    # Keep positive integers
    A = [a for a in A if a > 0]
    # Sort
    A.sort()
    # Check if 1 is missing
    if len(A) == 0 or A[0] != 1:
        return 1
    # Check for missing integers
    for i in range(1, len(A)):
        if A[i] - A[i-1] > 1:
            return A[i-1] + 1
    # If all integers are present, return the next integer
    return A[-1] + 1
```



