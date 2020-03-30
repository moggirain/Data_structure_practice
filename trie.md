---
description: Trie is used for retrieval of a key in a dataset of strings.
---

# Trie

Properties: 

1. Mapping mechanism

    2. Multiple search tree

    3. Dictionary vs Trie

* The searching in a dictionary will be O\(logn\) and it related to the length of items in the dictionary.
* Time complexity is not related to how many items in the dictionary, but with the length of searching word O\(w\).

## Trie Structure

![Trie \| Algorithms and Data Structures \| Douglas Wilhelm Harder](https://ece.uwaterloo.ca/~dwharder/aads/Projects/3/Trie/images/trie.1.png)



1. Every note has multiple （26） links to the child node \(even it is not shown\)
   * Different goal: Might include capital and punctuation, then the size &gt; 26
   * Different language might be different
   * Map character to nodes
2. Before getting the node, we already knew the item
3. How to define you get the word?
   * Traverse to the leaf node and get the end letter match

_IsEnd_ : _Mark if the node is the end of one word_

* Example: Panda, pan \(n needs to be marked in the path panda\)
* IsEnd \(when visit the node, if it is the end of a word\)

_Insert: Add a new word_ 

* traverse and get the char of a word
* Create each char as a node after check if the char in the path
* If exist, then just traverse the path by get\(\) method
* Evaluate if the word is in the path

_Search: Search if the word is in the dic_ 

* traverse each char of a word
* Check if the char in path 
  * If not return False 
  * If exist, visit the char and continue search next 
  * until the IsEnd = True 

_Start with: Search the prefix only_ 

* traverse each char of a word
* Check if the char in path 
  * If not return False 
  * If exist, visit the char and continue search next 
  * until the end of prefix 

## LC 208 Trie Implementation \(Dictionary\)

Time Complexity: Search/Insert O\(n\)  n = len\(word\)

Space Complexity: O\(m \* n\) n = avg \(len\(word\)\), m = number of word

```python
class Trie:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self. root = {} # create a new dic
        

    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        cur = self.root # initialize the child  node
        for c in word:
            if c not in cur: 
                cur[c] = {} # create a new dic
            cur = cur[c] # visit the new address
        cur["#"] = True  # check the end 
    
    
                
    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        cur = self.root
        for c in word:
            if c not in cur:
                return False 
            cur = cur[c]
        return "#" in cur
    

    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        cur = self.root
        for c in prefix:
            if c not in cur:
                return False 
            cur = cur[c]
        return True 
        
# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```

{% hint style="success" %}
 More info can be found here: 

[https://leetcode.com/articles/implement-trie-prefix-tree/](https://leetcode.com/articles/implement-trie-prefix-tree/)
{% endhint %}





