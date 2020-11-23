# Friendship Series

```text
Friendships

Friendships
user1   	| user2     | date
A	   	| B 	 | 2019-12-31
A	   	| C 	 | 2020-01-01
A	   	| D 	 | 2020-03-08
B	   	| D        | 2020-03-08
E       	 	| F 	 | 2020-11-03
E       		| G        | 2020-11-04

Q1. How many friends does each user have?



Desired result
User     | # friends
A    	 | 3
B    	 | 2
C    	 | 1
D    	 | 2
E    	 | 2
F    	 | 1
G    	 | 1




Friend_Actions
sender     | receiver 	 | action     	| date
A   	    | B          	 | approve    	| 2019-12-31
E    	    | A    	 | approve 	| 2019-12-31
A      	    | G 		 | deny     	| 2019-12-31
C    	    | A    	 | approve 	| 2020-01-01
E    	    | A    	 | unfriend 	| 2020-01-01
A    	    | D    	 | approve 	| 2020-03-08
D    	    | B    	 | approve 	| 2020-11-03
D    	    | E    	 | deny     	| 2020-11-04
E    	    | G    	 | approve     	| 2020-11-04
	 
Q2. Recreate friendships table before a specific date
```

