# Longest Valid Parentheses
Hard

Given a string containing just the characters '(' and ')', return the length of the longest valid (well-formed) parentheses substring.

### Example 1:

- Input: s = "(()"
- Output: 2
- Explanation: The longest valid parentheses substring is "()".

### Example 2:

- Input: s = ")()())"
- Output: 4
- Explanation: The longest valid parentheses substring is "()()".

### Example 3:

- Input: s = ""
- Output: 0
 
### Constraints:

- 0 <= s.length <= 3 * 104
- `s[i]` is `'('`, or `')'`.

### Solution:

- Create stack to store characters.

```
longest_length = 0, length = 0

for i in 0 to s.size():
    if s[i] is '(', push i into stack
    else:
        if stack is empty, push i into stack
        else:
            if top of stack is '(', pop stack
                if stack is empty, length = i + 1
                else length = i - stack.top()
                longest_length = max(longest_length, length)
            else:
                push i into stack

return longest_length
```

#### Example:
```
( ( ) ( ( ) ( )
0 1 2 3 4 5 6 7
stack:
0 1      i = 2，pop 1
0        length = 2 - 0 = 2
0 3 4    i = 5，pop 4
0 3      length = 5 - 3 = 2
0 3 6    i = 7，pop 6
0 3      length = 7 - 3 = 4
longest length is 4
```

#### Complexity:
time: O(n), traverse string
space: O(n) for stack

### Solution without extra space (TO DO):

In this approach, we make use of two counters `left` and `right`. 
1. We start traversing the string from the left towards the right and for every '(' encountered, we increment the `left` counter and for every ')' encountered, we increment the `right` counter.
    
2. Whenever `left` becomes equal to `right`, we calculate the length of the current valid string and keep track of maximum length substring found so far. If `right` becomes greater than `left`, we reset `left` and `right` to `0`.

3. Next, we start traversing the string from right to left and similar procedure is applied. However, this time, if `left` becomes greater than `right`, we reset `left` and `right` to `0`.

#### Proof:

assume the longest substring is `s[i: j + 1]`
1. From left to right，when we reach `i - 1`
- if `left = right != 0`, then we found that the `s[i: j + 1]` is not the longest substring. Contradict!
- if `left = right = 0`, then we can found substring, because when we reach `j`, `left = right`, we will calculate the length of the current valid string.
- if `left < right`, due to rule 2, we reset `left` and `right` to `0`  
- if `left > right`, we cannot find the substring `s[i: j + 1]`, because we will not stop when we reach `j`

2. From right to left, we get the similar result, point 1,2 in Case 1 is also applied to this case. We only analyze other two points.
- if `left > right`, due to rule 3, we reset `left` and `right` to `0`
-   if `left < right`, we cannot find the substring `s[i: j + 1]`, because we will not stop when we reach `i`

3. We can prove the Case 1 Point 4 cannot exist with Case 2 Point 2 simulatenously.

<font size = 8>

```
(    (  )(  ) (  )  )
i-1  i           j  j+1
 |         |         |
L>R       L=R       L<R

```
</font>

- if `i - 1` is `)`, because L > R, therefore, `i - 1` must be a part of valid parentheses, therefore substring `s[i: j + 1]` will not be the longest.
- if `j + 1` is `(`, becasue L < R, therefore, `j + 1` must be a part of valid parentheses, therefore substring `s[i: j + 1]` will not be the longest.
- Therefore,  `i - 1` must be `(`,`j + 1` must be `)`, the `s[i - 1 : j + 2]` is also the valid parenthese. Contradict.

4. Therefore Case 1 Point 4 cannot exist with Case 2 Point 2 simulatenously. One of them will not exist. Therefore, we can find the substring `s[i: j + 1]` either from left to right scan or right to left scan.