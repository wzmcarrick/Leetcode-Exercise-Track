## 332. Reconstruct Itinerary ##

**Link:**[332. Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/description/)

**Thoughts:**
  - 终止条件是：我们回溯遍历的过程中，遇到的机场个数，如果达到了（航班数量+1），那么我们就找到了一个行程，把所有航班串在一起了
  - ![image](https://user-images.githubusercontent.com/69004164/210938057-8ccd1be2-905e-4c9d-97e6-851072a55cab.png)

**Solutions:**
```
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        # defaultdic(list) 是为了方便直接append
        tickets_dict = defaultdict(list)
        for item in tickets:
            tickets_dict[item[0]].append(item[1])
        # 给每一个机场的到达机场排序，小的在前面，在回溯里首先被pop(0）出去
	    # 这样最先找的的path就是排序最小的答案，直接返回
        
        for airport in tickets_dict:
            
            '''
            tickets_dict里面的内容是这样的
            {'JFK': ['ATL', 'SFO'], 'SFO': ['ATL'], 'ATL': ['JFK', 'SFO']})
            '''

            tickets_dict[airport].sort()

        path = ['JFK']
        def backtracking(start):

            if len(path) == len(tickets) + 1:
                return True
            
            for _ in tickets_dict[start]:
                #必须及时删除，避免出现死循环
                end = tickets_dict[start].pop(0)
                path.append(end)
                # 只要找到一个就可以返回了
                if backtracking(end):
                    return True
                path.pop()
                tickets_dict[start].append(end)
    
        backtracking('JFK')
        return path
```

## 51. N-Queens ##

**Link:**[51. N-Queens](https://leetcode.com/problems/n-queens/description/)

**Thoughts:**
  - ![image](https://user-images.githubusercontent.com/69004164/210938577-4eb522bf-b6bc-4df6-a3e7-ae05d425126d.png)


**Solutions:**
```
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        res = []
        board = [['.'] * n for _ in range(n)]
        print(board)

        def isValid(board, row, col):
            #判断同一列是否冲突
            for i in range(len(board)):
                if board[i][col] == 'Q':
                    return False
            
            # 判断左上角是否冲突
            i = row - 1
            j = col - 1
            while i >= 0 and j >= 0:
                if board[i][j] == 'Q':
                    return False
                i -= 1
                j -= 1
            
            # 判断右上角是否冲突
            i = row - 1
            j = col + 1
            while i >= 0 and j < len(board):
                if board[i][j] == 'Q':
                    return False
                i -= 1
                j += 1

            return True               
                
        def backtrack(board, row, n):
            if row == n:
                temp_res = []
                for temp in board:
                    temp_str = "".join(temp)
                    temp_res.append(temp_str)
                res.append(temp_res)
            
            for col in range(n):
                if not isValid(board, row, col):
                    continue
                board[row][col] = 'Q'
                backtrack(board, row+1, n)
                board[row][col] = '.'
        
        backtrack(board, 0 ,n)
        return res
```

## 37. Sudoku Solver ##

**Link:**[37. Sudoku Solver](https://leetcode.com/problems/sudoku-solver/description/)

**Thoughts:**
  - ![image](https://user-images.githubusercontent.com/69004164/210939019-5e0fa4e7-15e6-4612-848f-5a6755b92662.png)



**Solutions:**
```
class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        self.backtracking(board)

    def backtracking(self, board: List[List[str]]) -> bool:
        # 若有解，返回True；若无解，返回False
        for i in range(len(board)): # 遍历行
            for j in range(len(board[0])):  # 遍历列
                # 若空格内已有数字，跳过
                if board[i][j] != '.': continue
                for k in range(1, 10):  
                    if self.is_valid(i, j, k, board):
                        board[i][j] = str(k)
                        if self.backtracking(board): return True
                        board[i][j] = '.'
                # 若数字1-9都不能成功填入空格，返回False无解
                return False
        return True # 有解

    def is_valid(self, row: int, col: int, val: int, board: List[List[str]]) -> bool:
        # 判断同一行是否冲突
        for i in range(9):
            if board[row][i] == str(val):
                return False
        # 判断同一列是否冲突
        for j in range(9):
            if board[j][col] == str(val):
                return False
        # 判断同一九宫格是否有冲突
        start_row = (row // 3) * 3
        start_col = (col // 3) * 3
        for i in range(start_row, start_row + 3):
            for j in range(start_col, start_col + 3):
                if board[i][j] == str(val):
                    return False
        return True
```
