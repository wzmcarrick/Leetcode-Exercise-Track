## 860. Lemonade Change ##

**Link:**[860. Lemonade Change](https://leetcode.com/problems/lemonade-change/description/)

**Thoughts:**
  - 情况一：账单是5，直接收下。
  - 情况二：账单是10，消耗一个5，增加一个10
  - 情况三：账单是20，优先消耗一个10和一个5，如果不够，再消耗三个5
  - 情况一，情况二，都是固定策略,唯一不确定的其实在情况三
  - 美元10只能给账单20找零，而美元5可以给账单10和账单20找零，美元5更万能！
  - 局部最优：遇到账单20，优先消耗美元10，完成本次找零。全局最优：完成全部账单的找零。

**Solutions:**
```
class Solution:
    def lemonadeChange(self, bills: List[int]) -> bool:
        five, ten = 0, 0
        for bill in bills:
            if bill == 5:
                five += 1
            if bill == 10:
                if five < 1:
                    return False          
                ten += 1
                five -= 1
                  
            if bill == 20:
                # 优先消耗10美元，因为5美元的找零用处更大，能多留着就多留着
                if five >= 1 and ten >= 1:
                    five -= 1
                    ten -= 1
                elif five >=3:
                    five -= 3
                else:
                    return False
        return True
```

## 406. Queue Reconstruction by Height ##

**Link:**[406. Queue Reconstruction by Height](https://leetcode.com/problems/queue-reconstruction-by-height/description/)

**Thoughts:**
  - 本题有两个维度，h和k，看到这种题目一定要想如何确定一个维度，然后再按照另一个维度重新排列。
  - 先按照身高h来排序，身高一定是从大到小排（身高相同的话则k小的站前面），让高个子在前面。
  - 此时我们可以确定一个维度了，就是身高，前面的节点一定都比本节点高！
  - 那么只需要按照k为下标重新插入队列就可以了
  - ![image](https://user-images.githubusercontent.com/69004164/211642839-b60b170c-97f3-4a02-b594-9aa696cfe748.png)
  - 按照身高排序之后，优先按身高高的people的k来插入，后序插入节点也不会影响前面已经插入的节点，最终按照k的规则完成了队列
  - 所以在按照身高从大到小排序后：
  - 局部最优：优先按身高高的people的k来插入。插入操作过后的people满足队列属性
  - 全局最优：最后都做完插入操作，整个队列满足题目队列属性
  - ![image](https://user-images.githubusercontent.com/69004164/211643121-b57e122b-b0cc-4a58-b46e-c802633a5a19.png)

**Solutions:**
```
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people.sort(key = lambda x: (-x[0],x[1]))
        q = []
        for p in people:
            q.insert(p[1],p)
        return q
```
时间复杂度：O(nlog n + n^2)
空间复杂度：O(n)

## 452. Minimum Number of Arrows to Burst Balloons ##

**Link:**[452. Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)

**Thoughts:**
  - 局部最优：当气球出现重叠，一起射，所用弓箭最少。
  - 全局最优：把所有气球射爆所用弓箭最少
  - 如果气球重叠了，重叠气球中右边边界的最小值 之前的区间一定需要一个弓箭
  - ![image](https://user-images.githubusercontent.com/69004164/211643682-c7643378-cc06-4b2a-94f3-f045412b0602.png)

**Solutions:**
```
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        points.sort()
        shot = 1
        for i in range(1,len(points)):

            if points[i][0] > points[i-1][1]:
                shot += 1
            else: 
                points[i][1] = min(points[i][1], points[i-1][1])
        return shot
```
