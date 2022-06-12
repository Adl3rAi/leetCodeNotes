# Dynamic Programming



## Basics

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟢      | [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/) |[509. Fibonacci Number](#509-fibonacci-number)      |
|     🟠      | [322. Coin Change](https://leetcode.com/problems/coin-change/) |[322. Coin Change](#322-coin-change)      |



**动态规划本质上就是求最值**

**明确base case ➡️ 明确“状态” ➡️ 明确选择 ➡️ 定义`dp`数组/函数的含义**

### 509. Fibonacci Number

The **Fibonacci numbers**, commonly denoted `F(n)` form a sequence, called the **Fibonacci sequence**, such that each number is the sum of the two preceding ones, starting from `0` and `1`. That is,

```
F(0) = 0, F(1) = 1
F(n) = F(n - 1) + F(n - 2), for n > 1.
```

Given `n`, calculate `F(n)`.

 

**Example 1:**

```
Input: n = 2
Output: 1
Explanation: F(2) = F(1) + F(0) = 1 + 0 = 1.
```

**Example 2:**

```
Input: n = 3
Output: 2
Explanation: F(3) = F(2) + F(1) = 1 + 1 = 2.
```

**Example 3:**

```
Input: n = 4
Output: 3
Explanation: F(4) = F(3) + F(2) = 2 + 1 = 3.
```

 

**Constraints:**

- `0 <= n <= 30`

---

简单的递归写法包含过多的重复子问题，例如计算`f(20)=f(19)+f(18)`会重复计算`f(18)`

```cpp
int fib(int n) {
  if(n == 0) return 0;
  if(n == 1 || n == 2) return 1;
  return fib(n-1) + fib(n-2);
}
```

带有备忘录`memo`的递归 **Top-down结构**

```cpp
int fib(int n) {
  vector<int> memo;
  memo.resize(n+1);
  return f(memo, n);
}
int f(vector<int>& memo, int n) {
  // 明确 base case
  if(n == 0 || n == 1) return n;
  //
  if(memo[n] != 0) return memo[n];
  memo[n] = f(memo, n-1) + f(memo, n-2);
  return memo[n];
}
```

`dp`数组的迭代解法

```cpp
int fib(int n) {
  if(n == 0) return 0;
  vector<int> dp;
  dp.resize(n+1);
  // 明确 base case
  dp[0] = 0;
  dp[1] = 1;
  //
  for(int i = 2; i <= n; i++) {
    dp[i] = dp[i-1] + dp[i-2];
  }
  return dp[n];
}
```

---

### 322. Coin Change

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the fewest number of coins that you need to make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

 

**Example 1:**

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

**Example 2:**

```
Input: coins = [2], amount = 3
Output: -1
```

**Example 3:**

```
Input: coins = [1], amount = 0
Output: 0
```

 

**Constraints:**

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`

```cpp
class Solution {
public:
    vector<int> memo;
    int coinChange(vector<int>& coins, int amount) {
        memo.resize(amount+1);
        return dp(coins, amount);
    }
    int dp(vector<int>& coins, int amount) {
        if(amount == 0) return 0;
        if(amount < 0) return -1;
        if(memo[amount] != 0) return memo[amount];
        int res = INT_MAX;
        for(int coin : coins) {
            int sub = dp(coins, amount - coin);
            if(sub == -1) continue;
            res = min(res, sub + 1);
        }
        memo[amount] = (res == INT_MAX) ? -1 : res;
        return memo[amount];
    }
};
```

```cpp
class Solution {
  public:
  int coinChange(vector<int>& coins, int amount) {
    vector<int>& dp;
    dp.resize(amount + 1);
    dp[0] = 0;
    for(int i = 0; i < dp.size(); i++) {
      for(int coin : coins) {
        if(i - coin < 0) {
          continue;
        }
        dp[i] = min(dp[i], 1 + dp[i - coin]);
      }
    }
    return (dp[amount] == 0) ? -1 : dp[amount];
  }
};
```

---

## Longest Increasing Subsequence(LIS)

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟠      | [300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) |[300. Longest Increasing Subsequence](#300-longest-increasing-subsequence)      |
|     🔴      | [354. Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/) |[354. Russian Doll Envelopes](#354-russian-doll-envelopes)      |

### 300. Longest Increasing Subsequence

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.

 

**Example 1:**

```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

**Example 2:**

```
Input: nums = [0,1,0,3,2,3]
Output: 4
```

**Example 3:**

```
Input: nums = [7,7,7,7,7,7,7]
Output: 1
```

 

**Constraints:**

- `1 <= nums.length <= 2500`
- `-104 <= nums[i] <= 104`

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> dp;
        dp.resize(nums.size(), 1);
        for(int i = 0; i < nums.size(); i++) {
            for(int j = 0; j < i; j++) {
                if(nums[j] < nums[i]) {
                    dp[i] = max(dp[i], 1 + dp[j]);
                }
            }
        }
        int res = 0;
        for(int i = 0; i < dp.size(); i++) {
            res = max(res, dp[i]);
        }
        return res;
    }
};
```

---

### 354. Russian Doll Envelopes

You are given a 2D array of integers `envelopes` where `envelopes[i] = [wi, hi]` represents the width and the height of an envelope.

One envelope can fit into another if and only if both the width and height of one envelope are greater than the other envelope's width and height.

Return *the maximum number of envelopes you can Russian doll (i.e., put one inside the other)*.

**Note:** You cannot rotate an envelope.

 

**Example 1:**

```
Input: envelopes = [[5,4],[6,4],[6,7],[2,3]]
Output: 3
Explanation: The maximum number of envelopes you can Russian doll is 3 ([2,3] => [5,4] => [6,7]).
```

**Example 2:**

```
Input: envelopes = [[1,1],[1,1],[1,1]]
Output: 1
```

 

**Constraints:**

- `1 <= envelopes.length <= 105`
- `envelopes[i].length == 2`
- `1 <= wi, hi <= 105`

```cpp
class Solution {
public:
    struct cmp1 {
        bool operator()(vector<int>& a, vector<int>& b) {
            if(a[0] != b[0]) return a[0] <= b[0];
            else{
                return a[1] >= b[1];
            }
        }
    }cmp1;
    int maxEnvelopes(vector<vector<int>>& envelopes) {
        int n = envelopes.size();
        sort(envelopes.begin(), envelopes.end(), cmp1);
        vector<int> height;
        height.resize(n);
        for(int i = 0; i < n; i++) {
            height[i] = envelopes[i][1];
        }
        return lenOfLists(height);
    }
    int lenOfLists(vector<int>& nums) {
        vector<int> dp;
        dp.resize(nums.size(), 1);
        for(int i = 0; i < dp.size(); i++) {
            for(int j = 0; j < i; j++) {
                if(nums[j] < nums[i]) {
                    dp[i] = max(dp[i], 1 + dp[j]);
                }
            }
        }
        int res = 0;
        for(int i = 0; i < dp.size(); i++) {
            res = max(res, dp[i]);
        }
        return res;
    }
};
```

---

### 64. Minimum Path Sum

Given a `m x n` `grid` filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

```
Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
Output: 7
Explanation: Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.
```

**Example 2:**

```
Input: grid = [[1,2,3],[4,5,6]]
Output: 12
```

 

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 200`
- `0 <= grid[i][j] <= 100`

```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        vector<vector<int>> dp;
        dp.resize(m);
        for(int i = 0; i < m; i++) {
            dp[i].resize(n);
        }
        dp[0][0] = grid[0][0];
        for(int i = 1; i < m; i++) {
            dp[i][0] = dp[i-1][0] + grid[i][0];
        }
        for(int j = 1; j < n; j++) {
            dp[0][j] = dp[0][j-1] + grid[0][j];
        }
        for(int i = 1; i < m; i++) {
            for(int j = 1; j < n; j++) {
                dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
            }
        }
        return dp[m-1][n-1];
    }
};
```

---

### 887. Super Egg Drop

You are given `k` identical eggs and you have access to a building with `n` floors labeled from `1` to `n`.

You know that there exists a floor `f` where `0 <= f <= n` such that any egg dropped at a floor **higher** than `f` will **break**, and any egg dropped **at or below** floor `f` will **not break**.

Each move, you may take an unbroken egg and drop it from any floor `x` (where `1 <= x <= n`). If the egg breaks, you can no longer use it. However, if the egg does not break, you may **reuse** it in future moves.

Return *the **minimum number of moves** that you need to determine **with certainty** what the value of* `f` is.

 

**Example 1:**

```
Input: k = 1, n = 2
Output: 2
Explanation: 
Drop the egg from floor 1. If it breaks, we know that f = 0.
Otherwise, drop the egg from floor 2. If it breaks, we know that f = 1.
If it does not break, then we know f = 2.
Hence, we need at minimum 2 moves to determine with certainty what the value of f is.
```

**Example 2:**

```
Input: k = 2, n = 6
Output: 3
```

**Example 3:**

```
Input: k = 3, n = 14
Output: 4
```

 

**Constraints:**

- `1 <= k <= 100`
- `1 <= n <= 104`

```cpp
class Solution {
public:
    vector<vector<int>> memo;
    int superEggDrop(int k, int n) {
        memo.resize(k+1);
        for(int i = 0; i <= k; i++) {
            memo[i].resize(n+1);
        }
        return dp(k,n);
    }
    int dp(int k, int n) {
        if(k == 1) return n;
        if(n == 0) return 0;
        int res = 10001;
        if(memo[k][n] != 0) {
            return memo[k][n];
        }
        for(int i = 1; i <= n; i++) {
            res = min(res, max(dp(k,n-i), dp(k-1, i-1))+1);
        }
        memo[k][n] = res;
        return res;
    }
};
```

