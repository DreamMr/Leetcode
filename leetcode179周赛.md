# leetcode179周赛

## 生成每种字符都是奇数个的字符串

- 题目描述

  给你一个整数 n，请你返回一个含 n 个字符的字符串，其中每种字符在该字符串中都恰好出现 奇数次 。

  返回的字符串必须只含小写英文字母。如果存在多个满足题目要求的字符串，则返回其中任意一个即可。

   

  示例 1：

  输入：n = 4
  输出："pppz"
  解释："pppz" 是一个满足题目要求的字符串，因为 'p' 出现 3 次，且 'z' 出现 1 次。当然，还有很多其他字符串也满足题目要求，比如："ohhh" 和 "love"。

  示例 2：

  输入：n = 2
  输出："xy"
  解释："xy" 是一个满足题目要求的字符串，因为 'x' 和 'y' 各出现 1 次。当然，还有很多其他字符串也满足题目要求，比如："ag" 和 "ur"。

  示例 3：

  输入：n = 7
  输出："holasss"

   

  提示：

      1 <= n <= 500

- ACcode

  ```
  class Solution:
      def getOu(self,n):
          a = n // 2
          if a % 2 == 0:
              return (a-1,a+1)
          else:
              return (a,a)
  
      def getJi(self,n):
          a = n // 2
          b = a + 1
          c = a if a % 2 == 0 else b
          o_a,o_b = self.getOu(c)
          return (a+b-c,o_a,o_b)
  
      def generateTheString(self, n: int) -> str:
          if n == 1:
              return 'a'
          if n % 2 == 0:
              a,b = self.getOu(n)
              ans = ['a' for i in range(a)]
              for i in range(b):
                  ans.append('b')
              return ''.join(ans)
          else:
              a,b,c = self.getJi(n)
              ans = ['a' for i in range(a)]
              for i in range(b):
                  ans.append('b')
              for i in range(c):
                  ans.append('c')
              return ''.join(ans)
  ```

  

- 解题思路

  偶=奇+奇 or（（偶+1）+（偶-1））

  奇=奇+偶

## 灯泡开关III

- 题目描述

  房间中有 n 枚灯泡，编号从 1 到 n，自左向右排成一排。最初，所有的灯都是关着的。

  在 k  时刻（ k 的取值范围是 0 到 n - 1），我们打开 light[k] 这个灯。

  灯的颜色要想 变成蓝色 就必须同时满足下面两个条件：

      灯处于打开状态。
      排在它之前（左侧）的所有灯也都处于打开状态。

  请返回能够让 所有开着的 灯都 变成蓝色 的时刻 数目 。

   

  示例 1：

  ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/08/sample_2_1725.png)

  输入：light = [2,1,3,5,4]
  输出：3
  解释：所有开着的灯都变蓝的时刻分别是 1，2 和 4 。

  示例 2：

  输入：light = [3,2,4,1,5]
  输出：2
  解释：所有开着的灯都变蓝的时刻分别是 3 和 4（index-0）。

  示例 3：

  输入：light = [4,1,2,3]
  输出：1
  解释：所有开着的灯都变蓝的时刻是 3（index-0）。
  第 4 个灯在时刻 3 变蓝。

  示例 4：

  输入：light = [2,1,4,3,6,5]
  输出：3

  示例 5：

  输入：light = [1,2,3,4,5,6]
  输出：6

   

  提示：

      n == light.length
      1 <= n <= 5 * 10^4
      light 是 [1, 2, ..., n] 的一个排列。

  

- ACcode

  ```
  class Solution:
      def numTimesAllBlue(self, light: List[int]) -> int:
          ans = 0
          s = set()
          cur = 0
          for i in range(len(light)):
              s.add(light[i])
              while True:
                  if cur + 1 in s:
                      cur += 1
                  else:
                      break
              if cur == i+1:
                  ans += 1
          return ans
  ```

  

- 解题思路

  这道题使用一个变量进行控制当前状态。

## 通知所有员工所需的时间

- 题目描述

  公司里有 n 名员工，每个员工的 ID 都是独一无二的，编号从 0 到 n - 1。公司的总负责人通过 headID 进行标识。

  在 manager 数组中，每个员工都有一个直属负责人，其中 manager[i] 是第 i 名员工的直属负责人。对于总负责人，manager[headID] = -1。题目保证从属关系可以用树结构显示。

  公司总负责人想要向公司所有员工通告一条紧急消息。他将会首先通知他的直属下属们，然后由这些下属通知他们的下属，直到所有的员工都得知这条紧急消息。

  第 i 名员工需要 informTime[i] 分钟来通知它的所有直属下属（也就是说在 informTime[i] 分钟后，他的所有直属下属都可以开始传播这一消息）。

  返回通知所有员工这一紧急消息所需要的 分钟数 。

   

  示例 1：

  ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/08/graph.png)

  输入：n = 1, headID = 0, manager = [-1], informTime = [0]
  输出：0
  解释：公司总负责人是该公司的唯一一名员工。

  示例 2：

  输入：n = 6, headID = 2, manager = [2,2,-1,2,2,2], informTime = [0,0,1,0,0,0]
  输出：1
  解释：id = 2 的员工是公司的总负责人，也是其他所有员工的直属负责人，他需要 1 分钟来通知所有员工。
  上图显示了公司员工的树结构。

  示例 3：

  ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/08/1730_example_3_5.PNG)

  输入：n = 7, headID = 6, manager = [1,2,3,4,5,6,-1], informTime = [0,6,5,4,3,2,1]
  输出：21
  解释：总负责人 id = 6。他将在 1 分钟内通知 id = 5 的员工。
  id = 5 的员工将在 2 分钟内通知 id = 4 的员工。
  id = 4 的员工将在 3 分钟内通知 id = 3 的员工。
  id = 3 的员工将在 4 分钟内通知 id = 2 的员工。
  id = 2 的员工将在 5 分钟内通知 id = 1 的员工。
  id = 1 的员工将在 6 分钟内通知 id = 0 的员工。
  所需时间 = 1 + 2 + 3 + 4 + 5 + 6 = 21 。

  示例 4：

  输入：n = 15, headID = 0, manager = [-1,0,0,1,1,2,2,3,3,4,4,5,5,6,6], informTime = [1,1,1,1,1,1,1,0,0,0,0,0,0,0,0]
  输出：3
  解释：第一分钟总负责人通知员工 1 和 2 。
  第二分钟他们将会通知员工 3, 4, 5 和 6 。
  第三分钟他们将会通知剩下的员工。

  示例 5：

  输入：n = 4, headID = 2, manager = [3,3,-1,2], informTime = [0,0,162,914]
  输出：1076

   

  提示：

      1 <= n <= 10^5
      0 <= headID < n
      manager.length == n
      0 <= manager[i] < n
      manager[headID] == -1
      informTime.length == n
      0 <= informTime[i] <= 1000
      如果员工 i 没有下属，informTime[i] == 0 。
      题目 保证 所有员工都可以收到通知。

- ACcode

  ```
  class TreeNode:
      def __init__(self,costTime):
          self.cost = costTime
          self.left = None
          self.right = None
  
  class Solution:
      def __init__(self):
          self.mp = {}
  
      def build_tree(self,headId,manager,informTime):
          self.mp[headId] = []
          for i in range(len(manager)):
              if manager[i] != -1:
                  parent = self.mp.get(manager[i],[])
                  parent.append(i)
                  self.mp[manager[i]] = parent
  
      def solve(self,informTime,manager,headId):
          childs = self.mp.get(headId,[])
          if not childs:
              return informTime[headId]
          ans = -1
          for i in range(len(childs)):
              ans = max(ans,self.solve(informTime,manager,childs[i]))
          return ans + informTime[headId]
  
          
  
      def numOfMinutes(self, n: int, headID: int, manager: List[int], informTime: List[int]) -> int:
          self.build_tree(headID,manager,informTime)
          return self.solve(informTime,manager,headID)
  ```

  

- 解题思路

  构造树，求最大的时间消耗



由于最后一题题意有点问题，不添加。