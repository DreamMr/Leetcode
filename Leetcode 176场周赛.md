# Leetcode 176场周赛

## 统计有序矩阵中得负数

- 题目描述

  给你一个 m * n 的矩阵 grid，矩阵中的元素无论是按行还是按列，都以非递增顺序排列。 

  请你统计并返回 grid 中 负数 的数目。

   

  示例 1：

  输入：grid = [[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]]
  输出：8
  解释：矩阵中共有 8 个负数。

  示例 2：

  输入：grid = [[3,2],[1,0]]
  输出：0

  示例 3：

  输入：grid = [[1,-1],[-1,-1]]
  输出：3

  示例 4：

  输入：grid = [[-1]]
  输出：1

   

  提示：

      m == grid.length
      n == grid[i].length
      1 <= m, n <= 100
      -100 <= grid[i][j] <= 100

- ACcode

  ```
  class Solution:
      def countNegatives(self, grid: List[List[int]]) -> int:
          ans = 0
          for i in range(len(grid)):
              for j in range(len(grid[i])):
                  if grid[i][j] < 0:
                      ans += 1
          return ans
          
  ```

  

- 解题思路

  简单遍历

## 最后K个数得乘积

- 题目描述

  请你实现一个「数字乘积类」ProductOfNumbers，要求支持下述两种方法：

  1. add(int num)

      将数字 num 添加到当前数字列表的最后面。

  2. getProduct(int k)

      返回当前数字列表中，最后 k 个数字的乘积。
      你可以假设当前列表中始终 至少 包含 k 个数字。

  题目数据保证：任何时候，任一连续数字序列的乘积都在 32-bit 整数范围内，不会溢出。

   

  示例：

  输入：
  ["ProductOfNumbers","add","add","add","add","add","getProduct","getProduct","getProduct","add","getProduct"]
  [[],[3],[0],[2],[5],[4],[2],[3],[4],[8],[2]]

  输出：
  [null,null,null,null,null,null,20,40,0,null,32]

  解释：
  ProductOfNumbers productOfNumbers = new ProductOfNumbers();
  productOfNumbers.add(3);        // [3]
  productOfNumbers.add(0);        // [3,0]
  productOfNumbers.add(2);        // [3,0,2]
  productOfNumbers.add(5);        // [3,0,2,5]
  productOfNumbers.add(4);        // [3,0,2,5,4]
  productOfNumbers.getProduct(2); // 返回 20 。最后 2 个数字的乘积是 5 * 4 = 20
  productOfNumbers.getProduct(3); // 返回 40 。最后 3 个数字的乘积是 2 * 5 * 4 = 40
  productOfNumbers.getProduct(4); // 返回  0 。最后 4 个数字的乘积是 0 * 2 * 5 * 4 = 0
  productOfNumbers.add(8);        // [3,0,2,5,4,8]
  productOfNumbers.getProduct(2); // 返回 32 。最后 2 个数字的乘积是 4 * 8 = 32 

   

  提示：

      add 和 getProduct 两种操作加起来总共不会超过 40000 次。
      0 <= num <= 100
      1 <= k <= 40000

- ACcode

  ```
  class ProductOfNumbers:
  
      def __init__(self):
          self.li = [1]
  
      def add(self, num: int) -> None:
          if num == 0:
              self.li = [1]
          else:
              self.li.append(self.li[-1] * num)
          
  
      def getProduct(self, k: int) -> int:
          if k >= len(self.li):
              return 0
          else:
              return self.li[-1] // self.li[len(self.li) - 1 - k]
  ```

  

- 解题思路

  这道题最开始用的方法就是累乘的方法，但是没有考虑到有0的情况。

  如果遇到0的话就清空所有列表的元素，因为包含0的序列乘积一定是0，所以需要判断k是否超过列表的长度，如果超过直接返回0，否则就是累乘的方法返回。

## 最多可以参加的会议数目

- 题目描述

  给你一个数组 events，其中 events[i] = [startDayi, endDayi] ，表示会议 i 开始于 startDayi ，结束于 endDayi 。

  你可以在满足 startDayi <= d <= endDayi 中的任意一天 d 参加会议 i 。注意，一天只能参加一个会议。

  请你返回你可以参加的 最大 会议数目。

   ![pic](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/16/e1.png)

  示例 1：

  输入：events = [[1,2],[2,3],[3,4]]
  输出：3
  解释：你可以参加所有的三个会议。
  安排会议的一种方案如上图。
  第 1 天参加第一个会议。
  第 2 天参加第二个会议。
  第 3 天参加第三个会议。

  示例 2：

  输入：events= [[1,2],[2,3],[3,4],[1,2]]
  输出：4

  示例 3：

  输入：events = [[1,4],[4,4],[2,2],[3,4],[1,1]]
  输出：4

  示例 4：

  输入：events = [[1,100000]]
  输出：1

  示例 5：

  输入：events = [[1,1],[1,2],[1,3],[1,4],[1,5],[1,6],[1,7]]
  输出：7

   

  提示：

      1 <= events.length <= 10^5
      events[i].length == 2
      1 <= events[i][0] <= events[i][1] <= 10^5

  

- ACcode

  ```
  class Solution:
      def keyFirst(self,x):
          return x[0]
  
      def maxEvents(self, events: List[List[int]]) -> int:
          events.sort(key=self.keyFirst)
  
          day = 1
          ans = 0
          h = []
          while events or h:
              last_i = -1
              for i in range(len(events)):
                  event = events[i]
                  if event[0] <= day:
                      heapq.heappush(h,event[1])
                      last_i = i
                  else:
                      break
              for i in range(last_i+1):
                  if events:
                      events.pop(0)
                  else:
                      break
              while h and h[0] < day:
                  heapq.heappop(h)
              if h:
                  heapq.heappop(h)
                  ans += 1
              day += 1
          return ans
  ```

  

- 解题思路

  这道题一眼看上去像是用dp，但是看了网上的题解，发现其实是个贪心。

  由于会议只需要参加一天就行，所以优先选择参加结束日期考前的会议可以获得最多会议数。

  模拟每一天，每次从按照会议开始日期排序，将当天能够参加的会议加入最小堆（按照结束最早日期排序），然后从最小堆中将选择无法参加的排除掉，然后选择一个结束日期最早的会议参加也排除掉。

## 多次求和构造目标数组

- 题目描述

  给你一个整数数组 target 。一开始，你有一个数组 A ，它的所有元素均为 1 ，你可以执行以下操作：

      令 x 为你数组里所有元素的和
      选择满足 0 <= i < target.size 的任意下标 i ，并让 A 数组里下标为 i 处的值为 x 。
      你可以重复该过程任意次

  如果能从 A 开始构造出目标数组 target ，请你返回 True ，否则返回 False 。

   

  示例 1：

  输入：target = [9,3,5]
  输出：true
  解释：从 [1, 1, 1] 开始
  [1, 1, 1], 和为 3 ，选择下标 1
  [1, 3, 1], 和为 5， 选择下标 2
  [1, 3, 5], 和为 9， 选择下标 0
  [9, 3, 5] 完成

  示例 2：

  输入：target = [1,1,1,2]
  输出：false
  解释：不可能从 [1,1,1,1] 出发构造目标数组。

  示例 3：

  输入：target = [8,5]
  输出：true

   

  提示：

      N == target.length
      1 <= target.length <= 5 * 10^4
      1 <= target[i] <= 10^9

- ACcode

  ```
  class Solution:
      def isPossible(self, target: List[int]) -> bool:
          if len(target) == 1 and target[0] == 1:
              return True
          elif len(target) == 1 and target[0] != 1:
              return False
          q = []
          s = 0
          for t in target:
              heapq.heappush(q,-t)
              s += t
          while True:
              larget = - heapq.heappop(q)
              if larget == 1:
                  return True
              if s - larget > larget:
                  return False
              second = - q[0]
              if second == 1:
                  if (larget - second) % (s - larget) == 0:
                      return True
                  else:
                      return False
              else:
                  k = (larget - second) // (s - larget) + 1
                  p_n = larget - (s - larget) * k
                  s_n = larget - (s - larget) * (k - 1)
                  s = s_n
                  if p_n < 1:
                      return False
                  heapq.heappush(q,-p_n)
  ```

  

- 解题思路

  这道题一开始就想可以通过逆推法，根据target数组逆推回去，但是数据范围很大，如果这样模拟计算会超时，所以要优化。

  首先看如果是[1e9,1]这种数据，由于每次逆推将最大值-(sum - max)，但是最大的位置还是原来的位置，要重复1e9次，所以需要在这里进行优化。

  假设列表中最大值是p，次小值是q，

  1. q == 1的情况：

     那么就希望有$p-(sum-p)*k<=q$

     所以化简得：$k >=(p-q)/(sum-p)​$

     注意，这里应该为：$k = (p-q)/(sum-p)​$

     因为如果如果不能刚好整除的话，就说明不能逆推回[1,1]的形式，所以返回false，如果等式成立，返回true。

  2. q ！= 1 的情况：

     同样推得：$p-(sum-p)*k<q​$

     使得最大值小于次大值，化简得：$k>(p-q)/(sum-p)+1$ 这里得除法向下取整，所以要+1，确保使最大值小于次大值如果是刚好整除也没关系，也要让最大值小于次大值。如果次大值不是1，但是能刚好整除，那肯定也是不行的。

     更新后p的值为：$p=p-(sum-p)*k$

     更新后sum的值为：$sum = p-(s-p)*(n-1)$

     推导更新后sum的值：
     $$
     sum=(s-p)+p-(s-p)*n\\
     =p+(s-p)*(1-n)\\
     =p-(s-p)*(n-1)
     $$
     用一个大根堆维护最大值和次小值，每次重复上述操作即可。

     这里python的大根堆用heapq来模拟，插入的时候取反即可。