# Array

| Type |
| :----: |
|  [preSum](#presum)      |



## preSum

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟢      | [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/) |  [303. Range Sum Query - Immutable](#303-range-sum-query---immutable)    |
|     🟠      | [304. Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/) | [304. Range Sum Query 2D - Immutable](#304-range-sum-query-2d---immutable)     |

### 303. Range Sum Query - Immutable

Given an integer array `nums`, handle multiple queries of the following type:

1. Calculate the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** where `left <= right`.

Implement the `NumArray` class:

- `NumArray(int[] nums)` Initializes the object with the integer array `nums`.
- `int sumRange(int left, int right)` Returns the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** (i.e. `nums[left] + nums[left + 1] + ... + nums[right]`).

 

**Example 1:**

```
Input
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
Output
[null, 1, -1, -3]

Explanation
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `-105 <= nums[i] <= 105`
- `0 <= left <= right < nums.length`
- At most `104` calls will be made to `sumRange`.

---

```cpp
class NumArray {
  private:
  		vector<int> preSum;
  public:
  		NumArray(vector<int>& nums) {
        int size = nums.size();
        preSum.resize(size + 1);
        preSum[0] = 0;
        for(int i = 1; i <= size + 1; i++) {
          preSum[i] = preSum[i-1] + nums[i-1];
        }
      }
  		int sumRange(int left, int right) {
        return preSum[right+1] - preSum[left];
      }
}
```

### 304. Range Sum Query 2D - Immutable

Given a 2D matrix `matrix`, handle multiple queries of the following type:

- Calculate the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

Implement the NumMatrix class:

- `NumMatrix(int[][] matrix)` Initializes the object with the integer matrix `matrix`.
- `int sumRegion(int row1, int col1, int row2, int col2)` Returns the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/14/sum-grid.jpg)

```
Input
["NumMatrix", "sumRegion", "sumRegion", "sumRegion"]
[[[[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]], [2, 1, 4, 3], [1, 1, 2, 2], [1, 2, 2, 4]]
Output
[null, 8, 11, 12]

Explanation
NumMatrix numMatrix = new NumMatrix([[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]);
numMatrix.sumRegion(2, 1, 4, 3); // return 8 (i.e sum of the red rectangle)
numMatrix.sumRegion(1, 1, 2, 2); // return 11 (i.e sum of the green rectangle)
numMatrix.sumRegion(1, 2, 2, 4); // return 12 (i.e sum of the blue rectangle)
```

 

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 200`
- `-105 <= matrix[i][j] <= 105`
- `0 <= row1 <= row2 < m`
- `0 <= col1 <= col2 < n`
- At most `104` calls will be made to `sumRegion`.

---

```cpp
class NumMatrix {
  private:
  		vector<vector<int>> preSum;
 	public:
  		NumMatrix(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        preSum.resize(m+1);
        for(int i = 0; i < m+1; i++) {
          preSum[i].resize(n+1);
        }
        for(int i = 1; i < m+1; i++) {
          for(int j = 1; j < n+1; j++) {
            preSum[i][j] = preSum[i-1][j] + preSum[i][j-1]-preSum[i-1][j-1]+matrix[i-1[j-1];
          }
        }                                                                      
      }
   		int sumRegion(int row1, int col1, int row2, int col2) {
        return preSum[row2+1][col2+1] - preSum[row2+1][col1] - preSum[row1][col2+1] + preSum[row1][col1] 
      }                                                                               
};
```

Note:

注意`preSum`的index右移1位，`matrix[i,j]`对应的前缀和数组是`preSum[i+1, j+1]`

