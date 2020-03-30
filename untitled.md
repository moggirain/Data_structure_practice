# Linked List

* **List: Store all data sequentially in memory**
* **Singly linked List**
  * Recursively defined structure 
  * Node and Value 
  * Reference to another singly linked list 
* **Why use singly linked list?**
  * Versability and flexibility:
    * Does not need to know the size and save the storage space
  * Linear and hierarchy relationship
* LinkedList 的操作中经常需要存head的位置 以免遍历中丢失头指针位置改变linkedlist的本身结构：
  * Solution：另外赋值head的位置  cur = head 然后用cur去遍历
* LinkedList 建立fakehead：链表问题是头结点之前没有元素解决方法是简历dummy head 链表第一个元素为dummy head.next 对应的元素
* LinkedList因为从头结点开始，等于从1 开始计数，size长度 == self.\_tail 的index
* 任何操作中注意check 头结点是否存在。固定操作 if not self.head: self.tail = None
* Search: 找到相应节点
  * Search by index
  * Search by value
  * 是否有头是否有值
* Add：
  * Add by index 
  * Add by value 
  * Fake-head 
* Remove: 
  * Remove by index 
  * Remove by value 

```python
Class _ListNode(object):
  def __init__(self, value):
    self.val = value 
    self.next = None 
Class LinkedList(object):
  def __init__(self):
    # initialize data structure 
    self._head = None 
    self._tail = None 
    self._size = 0
  # search
  def get(self, index):
    """Get the value of the index-th node in the linkedlist. If the index is invalid, return -1"""
    if self._head is None or index < 0:
      return -1
    return self._get(index).val 

  def _get(self,index):
      node = self._head
       For _ in range(index): # 移动index补偿
      node = node.next # 遍历去找
    return node #返回index

  def add_atHead(self, val):
    """Add a node value before the first element of the linkedlist """
    new_head = _ListNode(val) # 建个新node
    if self._size == 0:
      self._head = self._tail = new_head
    else:
      new_head.next = self._head # 新node指向当前头结点
      self._head = new_head # 头结点更新
    self._size+=1

  def add_atTail(self, val):
    """Append a node value to the last element of the linkedlist"""
    new_node = _ListNode(val)
    if self._size == 0: # corner case 
      self._head = self._tail = new_node
    else:
      self._tail.next = new_node # tail next link to new node
      self._tail = self._tail.next # self.tail point to next 
    self._size += 1

  def add_atIndex(self,index, val):
    """Add a node value before the index-th node in the linkedlist"""
    ### 3 cases 
    # corner case
    if not index or index < 0 or index > self._size:
      return
    # add at head
    if index == 0:
      self.add_atHead(val)
    # add at tail 
    elif index == self._size:
      self.add_atTail(val)
    else:
      node = self.get(index-1) # fint its previous node
      new_node = _ListNode(val)
      new_node.next = node.next
      node.next = new_node
      self._size+= 1

  def delete_atIndex(self,index):
    """
    delete the index-th node in the linkedlist, if the index is valid.
    """
    if not index or index < 0 or index > self._size:
      return 
    # delete head
    if index ==0:
      new_head = self._head.next 
      self._head.next = None # 这一步可以不写
      self._head = new_head
      if not self._head:
        self._tail = None # 
    # delete other and tail 
    else:
      # find the node before tail 
      node = self.get(self._size - 1)
      remove_node = node.next 
      node.next = remove_nod.next 
      remove_node.next = None # 可以不写
      # what if remove the tail?
      if index == self._size -1:
        self._tail = node # 直接把tail换成node
    self._size -= 1
```

* 双向链表
  * 具有next and previous指针 可以往前或往后查找
  * 优点：查找方便
  * 缺点：占用更多空间
* 循环链表
  * 不一定是闭环

    **Practices**

    Given a non-empty, singly linked list with head node `head`, return a middle node of linked list.

    If there are two middle nodes, return the second middle node.

```text
**Example 1:**

```
Input: [1,2,3,4,5]
Output: Node 3 from this list (Serialization: [3,4,5])
The returned node has value 3\.  (The judge's serialization of this node is [3,4,5]).
Note that we returned a ListNode object ans, such that:
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, and ans.next.next.next = NULL.
```


**Example 2:**

```
Input: [1,2,3,4,5,6]
Output: Node 4 from this list (Serialization: [4,5,6])
Since the list has two middle nodes with values 3 and 4, we return the second one.
```

**Note:**

*   The number of nodes in the given list will be between `1` and `100`.
```

```python
```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
​
class Solution:
    def middleNode(self, head: ListNode) -> ListNode:

```
```

**Runner Techniques**

-linkedlist 的很多题目需要用到 maintain 快慢指针

Linked List用法

* LRU cache
* Dictionary

Leetcode 142:参考题解

[https://leetcode.com/problems/linked-list-cycle-ii/discuss/258948/%2B-python](https://leetcode.com/problems/linked-list-cycle-ii/discuss/258948/%2B-python)

