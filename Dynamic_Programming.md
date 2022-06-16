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

---

### 10. Regular Expression Matching

Given an input string `s` and a pattern `p`, implement regular expression matching with support for `'.'` and `'*'` where:

- `'.'` Matches any single character.
- `'*'` Matches zero or more of the preceding element.

The matching should cover the **entire** input string (not partial).

 

**Example 1:**

```
Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

**Example 2:**

```
Input: s = "aa", p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

**Example 3:**

```
Input: s = "ab", p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

 

**Constraints:**

- `1 <= s.length <= 20`
- `1 <= p.length <= 30`
- `s` contains only lowercase English letters.
- `p` contains only lowercase English letters, `'.'`, and `'*'`.
- It is guaranteed for each appearance of the character `'*'`, there will be a previous valid character to match.

```cpp
class Solution {
public:
    map<string, bool> memo;
    bool isMatch(string s, string p) {
        return dp(s, 0, p, 0);
    }
    bool dp(string s, int i, string p, int j) {
        int m = s.size();
        int n = p.size();
        if(j == n) {
            return i == m;
        }
        if(i == m) {
            if((n-j) % 2 == 1) {
                return false;
            }
            for(; j + 1 < n; j += 2) {
                if(p[j+1] != '*') {
                    return false;
                }
            }
            return true;
        }
        string key = to_string(i) + "," + to_string(j);
        if(memo.count(key)) return memo[key];
        bool res = false;
        if(s[i] == p[j] || p[j] == '.') {
            if(j < n - 1 && p[j+1] == '*') {
                res = dp(s,i,p,j+2) || dp(s,i+1,p,j);
            }
            else {
                res = dp(s,i+1,p,j+1);
            }
        }
        else {
            if(j < n-1 && p[j+1] == '*') {
                res = dp(s,i,p,j+2);
            }
            else {
                res = false;
            }
        }
        memo[key] = res;
        return res;
    }
};
```

---

### 72. Edit Distance

Given two strings `word1` and `word2`, return *the minimum number of operations required to convert `word1` to `word2`*.

You have the following three operations permitted on a word:

- Insert a character
- Delete a character
- Replace a character

 

**Example 1:**

```
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```

**Example 2:**

```
Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```

 

**Constraints:**

- `0 <= word1.length, word2.length <= 500`
- `word1` and `word2` consist of lowercase English letters.

```cpp
class Solution {
public:
    vector<vector<int>> memo;
    int minDistance(string word1, string word2) {
        int m = word1.size();
        int n = word2.size();
        memo.resize(m);
        for(int i = 0; i < m; i++) {
            memo[i].resize(n, -1);
        }
        return dp(word1, m-1, word2, n-1);
    }
  // dp函数定义word1[0,..,i]变为word2[0,..,j]的最短距离
    int dp(string word1, int i, string word2, int j) {
        if(i == -1) return j+1;
        if(j == -1) return i+1;
        if(memo[i][j] != -1) return memo[i][j];
        if(word1[i] == word2[j]) {
            memo[i][j] = dp(word1, i-1, word2, j-1);
        }
        else {
            memo[i][j] = min(dp(word1, i, word2, j-1)+1, min(dp(word1, i-1, word2, j)+1, dp(word1, i-1, word2, j-1) + 1)); // 分别为插入、删除、修改
        }
        return memo[i][j];
    }
};
```







---

## Stock Exchange Problem

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟢      | [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) |[121. Best Time to Buy and Sell Stock](#121-best-time-to-buy-and-sell-stock)      |
|     🟠      | [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/) |[122. Best Time to Buy and Sell Stock II](#122-best-time-to-buy-and-sell-stock-ii)      |
|     🔴      | [123. Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/) |[123. Best Time to Buy and Sell Stock III](#123-best-time-to-buy-and-sell-stock-iii)      |
|     🔴      | [188. Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/) |[188. Best Time to Buy and Sell Stock IV](#188-best-time-to-buy-and-sell-stock-iv)      |
|     🟠      | [309. Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) |[309. Best Time to Buy and Sell Stock with Cooldown](#309-best-time-to-buy-and-sell-stock-with-cooldown)      |
|     🟠      | [714. Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/) |[714. Best Time to Buy and Sell Stock with Transaction Fee](#714-best-time-to-buy-and-sell-stock-with-transaction-fee)      |

**通用的套路：** 

`dp`三维数组`dp[i][k][0/1]` 其中`i`表示第几天`0 <= i <= n`，`k`表示交易次数的上限，`0/1`表示当天持有/未持有股票

base case就是开盘之前的情况`i==0`，`dp[0][k][0] = 0, dp[0][k][1] = -Infinity`未开盘时，是不可能持有股票的

状态转换方程

```cpp
// 当天未持有股票：昨天也未持有 或 昨天持有但今天卖出
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i]);
// 当天持有股票：昨天也持有 或 昨天未持有但今天购入
dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])''
```

可以通过第二个状态转换方程推导base case

```cpp
dp[0][k][1] = max(dp[-1][k][1], dp[-1][k-1][0] - prices[0]) = max(-Infinity, -prices[0]);
dp[0][k][1] = -prices[0];
```

### 121. Best Time to Buy and Sell Stock

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return *the maximum profit you can achieve from this transaction*. If you cannot achieve any profit, return `0`.

 

**Example 1:**

```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
```

**Example 2:**

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.
```

 

**Constraints:**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> dp;
        dp.resize(n);
        for(int i = 0; i < n; i++) {
            dp[i].resize(2);
        }
        for(int i = 0; i < n; i++) {
        		if(i - 1 == -1) {
              dp[i][0] = 0;
              dp[i][1] = -prices[i];
              continue;
            }
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i]);
            dp[i][1] = max(dp[i-1][1], -prices[i]);
        }
        return dp[n-1][0];
    }
};
```

---

### 122. Best Time to Buy and Sell Stock II

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can buy it then immediately sell it on the **same day**.

Find and return *the **maximum** profit you can achieve*.

 

**Example 1:**

```
Input: prices = [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.
```

**Example 2:**

```
Input: prices = [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Total profit is 4.
```

**Example 3:**

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: There is no way to make a positive profit, so we never buy the stock to achieve the maximum profit of 0.
```

 

**Constraints:**

- `1 <= prices.length <= 3 * 104`
- `0 <= prices[i] <= 104`

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> dp;
        dp.resize(n);
        for(int i = 0; i < n; i++) {
            dp[i].resize(2);
        }
        for(int i = 0; i < n; i++) {
          	if(i - 1 == -1) {
              dp[i][0] = 0;
              dp[i][1] = -prices[i];
              continue;
            }
            dp[i][0] = max(dp[i-1][0], dp[i-1][1]+prices[i]);
            dp[i][1] = max(dp[i-1][1], dp[i-1][0]-prices[i]);
        }
        return dp[n-1][0];
    }
};
```

---

### 123. Best Time to Buy and Sell Stock III

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete **at most two transactions**.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

 

**Example 1:**

```
Input: prices = [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
```

**Example 2:**

```
Input: prices = [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.
```

**Example 3:**

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

 

**Constraints:**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 105`

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        int max_k = 2;
        vector<vector<vector<int>>> dp(n, vector<vector<int>>(3, vector<int>(2)));
        for(int i = 0; i < n; i++) {
            for(int k = max_k; k >= 1; k--) {
                if(i - 1 == -1) {
                    dp[i][k][0] = 0;
                    dp[i][k][1] = -prices[i];
                    continue;
                }
                dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1]+prices[i]);
                dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0]-prices[i]);
            }
        }
        return dp[n-1][max_k][0];
    }
};
```

---

### 188. Best Time to Buy and Sell Stock IV

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `k`.

Find the maximum profit you can achieve. You may complete at most `k` transactions.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

 

**Example 1:**

```
Input: k = 2, prices = [2,4,1]
Output: 2
Explanation: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.
```

**Example 2:**

```
Input: k = 2, prices = [3,2,6,5,0,3]
Output: 7
Explanation: Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4. Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
```

 

**Constraints:**

- `0 <= k <= 100`
- `0 <= prices.length <= 1000`
- `0 <= prices[i] <= 1000`

```cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int n = prices.size();
        if(n <= 0 ) return 0;
        if(k > n/2) return maxProfit_Inf(prices);
        vector<vector<vector<int>>> dp(n, vector<vector<int>>(k+1, vector<int>(2)));
        for(int i = 0; i < n; i++) {
            dp[i][0][1] = INT_MIN;
            dp[i][0][0] = 0;
        }
        for(int i = 0; i < n; i++) {
            for(int p = k; p >= 1; p--) {
                if(i - 1 == -1) {
                    dp[i][p][0] = 0;
                    dp[i][p][1] = -prices[i];
                    continue;
                }
                dp[i][p][0] = max(dp[i-1][p][0], dp[i-1][p][1]+prices[i]);
                dp[i][p][1] = max(dp[i-1][p][1], dp[i-1][p-1][0]-prices[i]);
            }
        }
        return dp[n-1][k][0];
    }
    int maxProfit_Inf(vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> dp;
        dp.resize(n);
        for(int i = 0; i < n; i++) {
            dp[i].resize(2);
        }
        // base case
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        for(int i = 1; i < n; i++) {
            dp[i][0] = max(dp[i-1][0], dp[i-1][1]+prices[i]);
            dp[i][1] = max(dp[i-1][1], dp[i-1][0]-prices[i]);
        }
        return dp[n-1][0];
    }
};
```

---

### 309. Best Time to Buy and Sell Stock with Cooldown

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

- After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

 

**Example 1:**

```
Input: prices = [1,2,3,0,2]
Output: 3
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```

**Example 2:**

```
Input: prices = [1]
Output: 0
```

 

**Constraints:**

- `1 <= prices.length <= 5000`
- `0 <= prices[i] <= 1000`

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> dp;
        dp.resize(n);
        for(int i = 0; i < n; i++) {
            dp[i].resize(2);
        }
        for(int i = 0; i < n; i++) {
            if(i-1 == -1) {
                dp[i][0] = 0;
                dp[i][1] = -prices[i];
                continue;
            }
            if(i-2 == -1) {
                dp[i][0] = max(dp[i-1][0], dp[i-1][1]+prices[i]);
                dp[i][1] = max(dp[i-1][1], -prices[i]);
                continue;
            }
            dp[i][0] = max(dp[i-1][0], dp[i-1][1]+prices[i]);
            dp[i][1] = max(dp[i-1][1], dp[i-2][0]-prices[i]);
        }
        return dp[n-1][0];
    }
};
```

---

### 714. Best Time to Buy and Sell Stock with Transaction Fee

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `fee` representing a transaction fee.

Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

 

**Example 1:**

```
Input: prices = [1,3,2,8,4,9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
- Buying at prices[0] = 1
- Selling at prices[3] = 8
- Buying at prices[4] = 4
- Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```

**Example 2:**

```
Input: prices = [1,3,7,5,10,3], fee = 3
Output: 6
```

 

**Constraints:**

- `1 <= prices.length <= 5 * 104`
- `1 <= prices[i] < 5 * 104`
- `0 <= fee < 5 * 104`

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int n = prices.size();
        vector<vector<int>> dp;
        dp.resize(n);
        for(int i = 0; i < n; i++) {
            dp[i].resize(2);
        }
        for(int i = 0; i < n; i++) {
          	if(i-1 == -1) {
              dp[i][0] = 0;
              dp[i][1] = -prices[0] - fee;
              continue;
            }
            dp[i][0] = max(dp[i-1][0], dp[i-1][1]+prices[i]);
            dp[i][1] = max(dp[i-1][1], dp[i-1][0]-prices[i]-fee);
        }
        return dp[n-1][0];
    }
};
```

