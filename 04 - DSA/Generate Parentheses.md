## Problem Description

The problem requires us to generate all possible combinations of well-formed parentheses given `n` pairs. A well-formed combination of parentheses means that each opening bracket "(" has a corresponding closing bracket ")", and they are correctly nested. To better understand, for `n=3`, one such correct combination would be `"((()))"`, whereas `"(()"` or `"())("` would be incorrect formations.

## Intuition

To arrive at the solution, we need to think about how we can ensure we create well-formed parentheses. For that, we use Depth First Search (DFS), which is a recursive method to explore all possible combinations of the parentheses.

1. We start with an empty string and at each level of the recursion we have two choices: add an opening parenthesis "(" or a closing parenthesis ")".
2. However, we have to maintain the correctness of the parentheses. This means we cannot add a closing parenthesis if there are not enough opening ones that need closing.
3. We keep track of the number of opening and closing parentheses used so far. We are allowed to add an opening parenthesis if we have not used all `n` available opening parentheses.
4. We can add a closing parenthesis if the number of closing parentheses is less than the number of opening parentheses used. This ensures we never have an unmatched closing parenthesis.
5. We continue this process until we have used all `n` pairs of parentheses.
6. When both the opening and closing parentheses counts equal `n`, it means we have a valid combination, so we add it to our list of answers.

The code uses a helper function `dfs` which takes 3 parameters: the number of opening and closing parentheses used so far (`l` and `r`), and the current combination of parentheses (`t`).

By calling this function and starting our recursion with 0 used opening and closing parenthesis and an empty string, we will explore all valid combinations and store them in the list `ans`.

## Solution Approach

The solution uses the DFS (Depth First Search) algorithm to generate the combinations. It employs recursion as a mechanism to explore all possible combinations and backtracks when it hits a dead end (an invalid combination).

Here's a step-by-step breakdown of the DFS algorithm as implemented in the provided solution:

1. **Initial Call**: The `generateParenthesis` function initiates the process by calling the nested `dfs` (Depth First Search) function with initial values of zero used opening parentheses (`l`), zero used closing parentheses (`r`), and an empty string for the current combination (`t`).
    
2. **DFS Function**: This is the recursive function that contains the logic for the depth-first search. It takes three parameters:
    
    - `l`: The number of opening parentheses used so far.
    - `r`: The number of closing parentheses used so far.
    - `t`: The current combination string formed by adding parentheses.
3. **Base Case**: The recursion has two base cases within the DFS function: a. Invalid Condition: When the number of used opening parentheses `l` is more than `n`, or the closing parentheses `r` is more than `n` or more than `l`, it indicates an incorrect combination. The function returns without doing anything. b. Valid Combination: When both `l` and `r` equal `n`, it indicates that a valid combination of parentheses has been found. The current combination string `t` is added to the solution set `ans`.
    
4. **Recursive Exploration**: If neither base case is met, the function continues to explore:
    
    - Adding an opening parenthesis: If not all `n` opening parentheses have been used (`l < n`), the `dfs` function calls itself with `l + 1`, `r`, and appends `"("` to the current string `t`.
    - Adding a closing parenthesis: If the number of closing parentheses used is less than the number of opening parentheses (`r < l`), it implies that there are some unmatched opening parentheses. Thus, the `dfs` function calls itself with `l`, `r + 1`, and appends `")"` to `t`.

By calling these two lines of code, we ensure that we explore the decisions to either add an opening parenthesis or a closing one, thus generating all valid paths.

5. **Storage of Valid Combinations**: The `ans` list is the container that holds all valid combinations. Each time a complete valid combination is generated, it's added to this list. After all recursive calls are completed, `ans` will contain all the possible well-formed parentheses combinations.
    
6. **Return Result**: Finally, once all possible combinations have been explored, the `ans` list is returned as the result of the `generateParenthesis` function.
    

This implementation provides a sleek and efficient way to solve the problem of generating all combinations of well-formed parentheses, relying solely on the DFS strategy without needing any additional complex data structures.

```python
class Solution:
2    def generateParenthesis(self, n: int) -> List[str]:
3        # Helper function for depth-first search
4        def backtrack(open_count, close_count, path):
5            # If there are more open or more close parens than 'n', or more close parens than open, it's invalid
6            if open_count > n or close_count > n or open_count < close_count:
7                return
8            # When the current path uses all parens correctly, add the combination to the results
9            if open_count == n and close_count == n:
10                combinations.append(path)
11                return
12            # Continue the search by adding an open paren if possible
13            backtrack(open_count + 1, close_count, path + '(')
14            # Continue the search by adding a close paren if possible
15            backtrack(open_count, close_count + 1, path + ')')
16
17        # This list will hold all the valid combinations
18        combinations = []
19        # Start the recursive search with initial counts of open and close parentheses
20        backtrack(0, 0, '')
21        # Return all the valid combinations found
22        return combinations
```

```cpp
class Solution {
2public:
3    // Function to generate all combinations of well-formed parentheses.
4    vector<string> generateParenthesis(int n) {
5        vector<string> result; // This will store the valid combinations.
6
7        // Use a lambda function to perform depth-first search.
8        // 'leftCount' and 'rightCount' track the count of '(' and ')' used respectively.
9        // 'current' is the current combination of parentheses.
10        function<void(int, int, string)> depthFirstSearch = [&](int leftCount, int rightCount, string current) {
11            // If the current combination is invalid (more ')' than '(' or counts exceed 'n'), stop exploration.
12            if (leftCount > n || rightCount > n || leftCount < rightCount) return;
13
14            // If the combination is valid and complete, add it to the result list.
15            if (leftCount == n && rightCount == n) {
16                result.push_back(current);
17                return;
18            }
19
20            // If we can add a '(', do so and continue the search.
21            depthFirstSearch(leftCount + 1, rightCount, current + "(");
22
23            // If we can add a ')', do so and continue the search.
24            depthFirstSearch(leftCount, rightCount + 1, current + ")");
25        };
26
27        // Start the search with zero counts and an empty combination.
28        depthFirstSearch(0, 0, "");
29      
30        // Return all the valid combinations found.
31        return result;
32    }
33};
```
## Time and Space Complexity

### Time Complexity

The time complexity of the given code is `O(4^n / sqrt(n))`. This complexity arises because each valid combination can be represented by a path in a decision tree, which has `2n` levels (since we make a decision at each level to add either a left or a right parenthesis, and we do this `n` times for each parenthesis type). However, not all paths in the tree are valid; the number of valid paths follows the nth Catalan number, which is proportional to `4^n / (n * sqrt(n))`, and `n` is a factor that represents the polynomial part that gets smaller as `n` gets larger. Since we're looking at big-O notation, we simplify this to `4^n / sqrt(n)` for large `n`.

### Space Complexity

The space complexity is `O(n)` because the depth of the recursive call stack is proportional to the number of parentheses to generate, which is `2n`, and the space required to store a single generated set of parentheses is also linear to `n`. Hence, the complexity due to the call stack is `O(n)`. The space used to store the answers is separate and does not affect the complexity from a big-O perspective. Keep in mind that the returned list itself will contain `O(4^n / sqrt(n))` elements, and if you consider the space for the output list, the overall space complexity would be `O(n * 4^n / sqrt(n))`, which includes the length of each string times the number of valid strings. Typically, the space complexity considers only the additional space required, not the space for the output. Therefore, we only consider the `O(n)` space used by the call stack for our space complexity analysis.

Ref - https://algo.monster/liteproblems/22
