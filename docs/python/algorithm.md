# 算法

## 数据结构 

在 Python 中，数据结构可以通过内置类型或手动实现来表示和操作。以下是常用数据结构的 Python 实现：

### 1. **栈（Stack）**
栈是一种后进先出（LIFO）的数据结构，元素只能从一端添加或移除。

```python
class Stack:
    def __init__(self):
        self.stack = []

    def is_empty(self):
        return len(self.stack) == 0

    def push(self, item):
        self.stack.append(item)

    def pop(self):
        if not self.is_empty():
            return self.stack.pop()
        else:
            raise IndexError("pop from empty stack")

    def peek(self):
        if not self.is_empty():
            return self.stack[-1]
        else:
            raise IndexError("peek from empty stack")

    def size(self):
        return len(self.stack)
```

**示例：**
```python
s = Stack()
s.push(1)
s.push(2)
s.push(3)
print(s.pop())  # 输出 3
print(s.peek())  # 输出 2
print(s.size())  # 输出 2
```

### 2. **队列（Queue）**
队列是一种先进先出（FIFO）的数据结构，元素从一端入队，从另一端出队。

```python
class Queue:
    def __init__(self):
        self.queue = []

    def is_empty(self):
        return len(self.queue) == 0

    def enqueue(self, item):
        self.queue.append(item)

    def dequeue(self):
        if not self.is_empty():
            return self.queue.pop(0)
        else:
            raise IndexError("dequeue from empty queue")

    def size(self):
        return len(self.queue)
```

**示例：**
```python
q = Queue()
q.enqueue(1)
q.enqueue(2)
q.enqueue(3)
print(q.dequeue())  # 输出 1
print(q.size())  # 输出 2
```

### 3. **链表（Linked List）**
链表是一种线性数据结构，其中每个元素（节点）包含一个指向下一个节点的引用。

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def is_empty(self):
        return self.head is None

    def append(self, data):
        new_node = Node(data)
        if self.is_empty():
            self.head = new_node
            return
        last_node = self.head
        while last_node.next:
            last_node = last_node.next
        last_node.next = new_node

    def prepend(self, data):
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node

    def delete(self, key):
        temp = self.head
        if temp and temp.data == key:
            self.head = temp.next
            temp = None
            return
        prev = None
        while temp and temp.data != key:
            prev = temp
            temp = temp.next
        if temp is None:
            return
        prev.next = temp.next
        temp = None

    def print_list(self):
        current_node = self.head
        while current_node:
            print(current_node.data, end=" -> ")
            current_node = current_node.next
        print("None")
```

**示例：**
```python
ll = LinkedList()
ll.append(1)
ll.append(2)
ll.prepend(0)
ll.print_list()  # 输出 0 -> 1 -> 2 -> None
ll.delete(1)
ll.print_list()  # 输出 0 -> 2 -> None
```

### 4. **二叉树（Binary Tree）**
二叉树是一种树状数据结构，其中每个节点最多有两个子节点（左子节点和右子节点）。

```python
class TreeNode:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

class BinaryTree:
    def __init__(self, root):
        self.root = TreeNode(root)

    def preorder_traversal(self, start, traversal):
        if start:
            traversal.append(start.data)
            traversal = self.preorder_traversal(start.left, traversal)
            traversal = self.preorder_traversal(start.right, traversal)
        return traversal

    def inorder_traversal(self, start, traversal):
        if start:
            traversal = self.inorder_traversal(start.left, traversal)
            traversal.append(start.data)
            traversal = self.inorder_traversal(start.right, traversal)
        return traversal

    def postorder_traversal(self, start, traversal):
        if start:
            traversal = self.postorder_traversal(start.left, traversal)
            traversal = self.postorder_traversal(start.right, traversal)
            traversal.append(start.data)
        return traversal
```

**示例：**
```python
bt = BinaryTree(1)
bt.root.left = TreeNode(2)
bt.root.right = TreeNode(3)
bt.root.left.left = TreeNode(4)
bt.root.left.right = TreeNode(5)

print("Preorder:", bt.preorder_traversal(bt.root, []))  # 输出 [1, 2, 4, 5, 3]
print("Inorder:", bt.inorder_traversal(bt.root, []))    # 输出 [4, 2, 5, 1, 3]
print("Postorder:", bt.postorder_traversal(bt.root, []))# 输出 [4, 5, 2, 3, 1]
```

### 5. **哈希表（Hash Table）**
哈希表是一种通过哈希函数将键映射到值的数据结构，通常用于快速查找。

```python
class HashTable:
    def __init__(self):
        self.MAX = 100
        self.arr = [None for _ in range(self.MAX)]

    def get_hash(self, key):
        hash = 0
        for char in key:
            hash += ord(char)
        return hash % self.MAX

    def __setitem__(self, key, val):
        h = self.get_hash(key)
        self.arr[h] = val

    def __getitem__(self, key):
        h = self.get_hash(key)
        return self.arr[h]

    def __delitem__(self, key):
        h = self.get_hash(key)
        self.arr[h] = None
```

**示例：**
```python
ht = HashTable()
ht["march 6"] = 120
ht["march 7"] = 130
print(ht["march 6"])  # 输出 120
del ht["march 6"]
print(ht["march 6"])  # 输出 None
```

### 6. **图（Graph）**
图是一种表示节点和节点之间连接关系的数据结构。

```python
class Graph:
    def __init__(self):
        self.graph = {}

    def add_edge(self, node, neighbor):
        if node not in self.graph:
            self.graph[node] = []
        self.graph[node].append(neighbor)

    def print_graph(self):
        for node in self.graph:
            print(f"{node}: {self.graph[node]}")

    def bfs(self, start):
        visited = []
        queue = [start]
        while queue:
            node = queue.pop(0)
            if node not in visited:
                visited.append(node)
                queue.extend(self.graph[node])
        return visited

    def dfs(self, start, visited=None):
        if visited is None:
            visited = []
        visited.append(start)
        for neighbor in self.graph[start]:
            if neighbor not in visited:
                self.dfs(neighbor, visited)
        return visited
```

**示例：**
```python
g = Graph()
g.add_edge("A", "B")
g.add_edge("A", "C")
g.add_edge("B", "D")
g.add_edge("C", "D")
g.add_edge("D", "E")

g.print_graph()
# 输出:
# A: ['B', 'C']
# B: ['D']
# C: ['D']
# D: ['E']

print("BFS:", g.bfs("A"))  # 输出 BFS: ['A', 'B', 'C', 'D', 'E']
print("DFS:", g.dfs("A"))  # 输出 DFS: ['A', 'B', 'D', 'E', 'C']
```

以上是 Python 中一些常见数据结构的实现示例，这些数据结构是构建算法和处理数据的基础。可以根据具体应用场景选择合适的数据结构。

## 算法


Python 提供了许多工具来实现经典的算法，这些算法通常可以分为几大类：排序算法、查找算法、递归算法、贪心算法、动态规划、回溯算法、图算法等。以下是对这些常用算法的详细介绍与示例。

### 一、排序算法

#### 1.1 冒泡排序（Bubble Sort）
冒泡排序是一种简单的排序算法，它重复遍历列表，将相邻的元素两两比较并交换顺序，直到整个列表有序。
```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(0, n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]

arr = [64, 34, 25, 12, 22, 11, 90]
bubble_sort(arr)
print("Sorted array:", arr)
```

#### 1.2 快速排序（Quick Sort）
快速排序是一种基于分治思想的高效排序算法。它选择一个“基准”，将列表分为两个子列表，然后递归地排序两个子列表。
```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quick_sort(left) + middle + quick_sort(right)

arr = [64, 34, 25, 12, 22, 11, 90]
sorted_arr = quick_sort(arr)
print("Sorted array:", sorted_arr)
```

#### 1.3 归并排序（Merge Sort）
归并排序也是基于分治法的排序算法。它将数组分成两半，分别排序，然后合并两个有序数组。
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result

arr = [64, 34, 25, 12, 22, 11, 90]
sorted_arr = merge_sort(arr)
print("Sorted array:", sorted_arr)
```

### 二、查找算法

#### 2.1 线性查找（Linear Search）
线性查找是最简单的查找算法，它逐一检查列表中的每个元素，直到找到目标值。
```python
def linear_search(arr, x):
    for i in range(len(arr)):
        if arr[i] == x:
            return i
    return -1

arr = [64, 34, 25, 12, 22, 11, 90]
x = 22
result = linear_search(arr, x)
print("Element found at index:", result)
```

#### 2.2 二分查找（Binary Search）
二分查找是一种高效的查找算法，适用于已排序的列表。它通过反复将搜索范围减半来快速查找目标值。
```python
def binary_search(arr, x):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == x:
            return mid
        elif arr[mid] < x:
            left = mid + 1
        else:
            right = mid - 1
    return -1

arr = [11, 12, 22, 25, 34, 64, 90]
x = 22
result = binary_search(arr, x)
print("Element found at index:", result)
```

### 三、递归算法

#### 3.1 斐波那契数列（Fibonacci Sequence）
递归算法通常用于解决分解为更小相同子问题的情况。斐波那契数列就是一个经典例子。
```python
def fibonacci(n):
    if n <= 1:
        return n
    else:
        return fibonacci(n-1) + fibonacci(n-2)

n = 10
print("Fibonacci sequence:")
for i in range(n):
    print(fibonacci(i), end=" ")
```

#### 3.2 阶乘（Factorial）
阶乘是另一个递归算法的例子，用于计算一个正整数的阶乘。
```python
def factorial(n):
    if n == 0 or n == 1:
        return 1
    else:
        return n * factorial(n-1)

n = 5
print("Factorial of", n, "is", factorial(n))
```

### 四、贪心算法（Greedy Algorithm）

#### 4.1 找零问题（Coin Change Problem）
贪心算法是一种在每一步都选择当前最优解的算法，适用于某些优化问题。
```python
def coin_change(coins, amount):
    coins.sort(reverse=True)
    count = 0
    for coin in coins:
        while amount >= coin:
            amount -= coin
            count += 1
    return count

coins = [1, 5, 10, 25]
amount = 63
print("Minimum coins required:", coin_change(coins, amount))
```

### 五、动态规划（Dynamic Programming）

#### 5.1 背包问题（Knapsack Problem）
动态规划是一种优化算法，用于解决具有重叠子问题和最优子结构性质的问题。
```python
def knapsack(weights, values, capacity):
    n = len(values)
    dp = [[0 for _ in range(capacity + 1)] for _ in range(n + 1)]
    
    for i in range(n + 1):
        for w in range(capacity + 1):
            if i == 0 or w == 0:
                dp[i][w] = 0
            elif weights[i-1] <= w:
                dp[i][w] = max(values[i-1] + dp[i-1][w-weights[i-1]], dp[i-1][w])
            else:
                dp[i][w] = dp[i-1][w]
    
    return dp[n][capacity]

weights = [1, 3, 4, 5]
values = [1, 4, 5, 7]
capacity = 7
print("Maximum value in Knapsack:", knapsack(weights, values, capacity))
```

#### 5.2 最长公共子序列（Longest Common Subsequence）
动态规划也可以用于解决字符串问题，如最长公共子序列问题。
```python
def lcs(X, Y):
    m = len(X)
    n = len(Y)
    dp = [[None]*(n+1) for i in range(m+1)]
    
    for i in range(m+1):
        for j in range(n+1):
            if i == 0 or j == 0:
                dp[i][j] = 0
            elif X[i-1] == Y[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    return dp[m][n]

X = "AGGTAB"
Y = "GXTXAYB"
print("Length of LCS is", lcs(X, Y))
```

### 六、回溯算法（Backtracking）

#### 6.1 N 皇后问题（N-Queens Problem）
回溯算法通过递归地尝试不同的解决方案来解决组合问题。
```python
def is_safe(board, row, col):
    for i in range(col):
        if board[row][i] == 1:
            return False
    for i, j in zip(range(row, -1, -1), range(col, -1, -1)):
        if board[i][j] == 1:
            return False
    for i, j in zip(range(row, len(board), 1), range(col, -1, -1)):
        if board[i][j] == 1:
            return False
    return True

def solve_n_queens_util(board, col):
    if col >= len(board):
        return True
    for i in range(len(board)):
        if is_safe(board, i, col):
            board[i][col] = 1
            if solve_n_queens_util(board, col + 1):
                return True
            board[i][col] = 0
    return False

def solve_n_queens(n):
    board = [[0 for _ in range(n)] for _ in range(n)]
    if not solve_n_queens_util(board, 0):
        print("Solution does not exist")
        return False
    for row in board:
        print(row)
    return True

n = 4
```
