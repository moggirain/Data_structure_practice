# Stack and Queue

1. Balanced Brackets

We define an express as having balanced bracket brackets when every open bracket has a corresponding closing one and vice versa. 

You are given an arithmetic express on a single line of standard input, consisting of numbers letters, arithmetic operations, i, e,  +, -, \*, / and brackets. 

On a single line of standard output, print the following: 

1. "Y" if the brackets are balanced, and "N" otherwise
2.  A single space
3. All of the brackets from the input line in the same order as the input line, after removing all other characters. 

Example:

Input: \[{\(\(1+2\) \* \(x-4\)\)}\] Output: Y \[{\(\)\(\)}\]

```python
def balanedBracket(line):
	if not line:
		print("Y")
	stack = []
	brackets = []
	dic = dict(zip("([{", ")]}"))
	flag = True 

	for s in line:
		# left bracket
		if s in dic:
			stack.append(s)
			brackets.append(s)
		# right bracket
		elif s in dic.values():
			brackets.append(s)
			if stack and dic[stack[-1]] == s:
				stack.pop()
			else:
				flag = False 
		# non-bracket
		else:
			contineu 

	if stack:
		flag = False 

	if flag:
		print("Y" + " " + " ".join(brackets))
	else:
		print("N" + " " + " ".join(brackets))


if __name__ == "__main__":
	line = input()
	balancedBracket(line)
```

