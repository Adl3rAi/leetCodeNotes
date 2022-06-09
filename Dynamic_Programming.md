# Dynamic Programming

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟢      | [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/) |      |
|     🟠      | [322. Coin Change](https://leetcode.com/problems/coin-change/) |      |



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
    dp[p] = 0;
    for(int i = 0; i < dp.size(); i++) {
      for(int coin : coins) {
        if(i - coin < 0) {
          continue;
        }
        dp[i] = min(dp[i], 1 + dp[i - coin]);
      }
    }
    return (dp[amount] == amount + 1) ? -1 : dp[amount];
  }
};
```

