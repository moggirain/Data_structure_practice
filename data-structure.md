# Hash Map

## What is a Hash Map or Hash table in Python? 

Hash: Hash function for converting the input into an index. The mapping methods is based on a function, usually calculate the module to map key value pair. 

Convert each key to an index 

Hash conflict 

Hash table: 空间换时间

Collision 

Hash Conflict: 

* Separate Chaining 
* linked-list 
* Open addressing  

        - Linear probing 

       -  Quadratic probing 

       -  Double hashing 

{% embed url="https://en.wikipedia.org/wiki/Hash\_table" %}

## What will happen when insert a key and a value?

1. Use a hash function to find the index of the key 
2. Find a value corresponding to the index in the hash table 
3. Assume the implementation of hash table is through linkedlist \(red-black tree, etc\), traverse the linkedlist to check if the value in the linkedlist.
4. If exist, update the value and if not,  insert the value into the linkedlist 
5. check if the size exceed the capacity, and if not, do nothing, if yes, resize the capacity. 

## Time complexity of a hash map?

O\(1\)

## Have you had a chance to answer the previous question?

Yes, after a few months we finally found the answer. Sadly, Mike is on vacations right now so I'm afraid we are not able to provide the answer at this point.



