# Array

| Type |
| :----: |
|  [preSum](#presum)      |
| [diff array](#diff-array) |
| [rotate matrix](#rotate-matrix-or-spiral-matrix) |
| [sliding window](#sliding-window) |



## preSum

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟢      | [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/) |  [303. Range Sum Query - Immutable](#303-range-sum-query---immutable)    |
|     🟠      | [304. Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/) | [304. Range Sum Query 2D - Immutable](#304-range-sum-query-2d---immutable)     |

Condition: When the original array won't be changed, to query the sum of a range.

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

## diff array

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟠      | [1109. Corporate Flight Bookings](https://leetcode.com/problems/corporate-flight-bookings/) |[1101. Corporate Flight Bookings](#1109-corporate-flight-bookings)      |
|     🟠      | [1094. Car Pooling](https://leetcode.com/problems/car-pooling/) |[1094. Car Pooling](#1094-car-pooling)      |

### 1109. Corporate Flight Bookings

There are `n` flights that are labeled from `1` to `n`.

You are given an array of flight bookings `bookings`, where `bookings[i] = [firsti, lasti, seatsi]` represents a booking for flights `firsti` through `lasti` (**inclusive**) with `seatsi` seats reserved for **each flight** in the range.

Return *an array* `answer` *of length* `n`*, where* `answer[i]` *is the total number of seats reserved for flight* `i`.

 

**Example 1:**

```
Input: bookings = [[1,2,10],[2,3,20],[2,5,25]], n = 5
Output: [10,55,45,25,25]
Explanation:
Flight labels:        1   2   3   4   5
Booking 1 reserved:  10  10
Booking 2 reserved:      20  20
Booking 3 reserved:      25  25  25  25
Total seats:         10  55  45  25  25
Hence, answer = [10,55,45,25,25]
```

**Example 2:**

```
Input: bookings = [[1,2,10],[2,2,15]], n = 2
Output: [10,25]
Explanation:
Flight labels:        1   2
Booking 1 reserved:  10  10
Booking 2 reserved:      15
Total seats:         10  25
Hence, answer = [10,25]
```

 

**Constraints:**

- `1 <= n <= 2 * 104`
- `1 <= bookings.length <= 2 * 104`
- `bookings[i].length == 3`
- `1 <= firsti <= lasti <= n`
- `1 <= seatsi <= 104`

---

```cpp
class Solution {
  private:
  	vector<int> diff;
  public:
  	vector<int> corpFlightBookings(vector<vector<int>>& bookings, int n) {
      diff.resize(n);
      for(auto booking : bookings) {
        int i = booking[0] - 1;
        int j = booking[1] - 1;
        int val = booking[2];
        increment(i, j, val);
      }
      vector<int> res;
      res[0] = diff[0];
      for(int i = 1; i < n; i++) {
        res[i] = res[i-1] + diff[i];
      }
      return res;
    }
  	void increment(int i, int j, int val) {
      diff[i] += val;
      if(j+1 < diff.size()) {
        diff[j+1] -= val;
      }
    }
};
```

### 1094. Car Pooling

There is a car with `capacity` empty seats. The vehicle only drives east (i.e., it cannot turn around and drive west).

You are given the integer `capacity` and an array `trips` where `trips[i] = [numPassengersi, fromi, toi]` indicates that the `ith` trip has `numPassengersi` passengers and the locations to pick them up and drop them off are `fromi` and `toi` respectively. The locations are given as the number of kilometers due east from the car's initial location.

Return `true` *if it is possible to pick up and drop off all passengers for all the given trips, or* `false` *otherwise*.

 

**Example 1:**

```
Input: trips = [[2,1,5],[3,3,7]], capacity = 4
Output: false
```

**Example 2:**

```
Input: trips = [[2,1,5],[3,3,7]], capacity = 5
Output: true
```

 

**Constraints:**

- `1 <= trips.length <= 1000`
- `trips[i].length == 3`
- `1 <= numPassengersi <= 100`
- `0 <= fromi < toi <= 1000`
- `1 <= capacity <= 105`

---

```cpp
class Solution {
  private:
  	vector<int> diff;
  public:
  	bool carPooling(vector<vector<int>>& trips, int capacity) {
      diff.resize(1001);
      for(auto trip : trips) {
        int val = trip[0];
        int i = trip[1];
        int j = trip[2] - 1;
        increment(i,j,val);
      }
      vector<int> res(1001);
      res[0] = diff[0];
      if(res[0] > capacity) return false;
      for(int i = 1; i < 1001; i++) {
        res[i] = res[i-1] + diff[i];
        if(res[i]>capacity) return false;
      }
      return true;
    }
  	void increment(int i, int j, int val) {
      diff[i] += val;
      if(j+1 < diff.size()) {
        diff[j+1] -= val;
      }
    }
};
```

## Rotate Matrix or Spiral Matrix

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🟠      | [48. Rotate Image](https://leetcode.com/problems/rotate-image/) | [48. Rotate Image](#48-rotate-image)     |
|     🟠      | [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/) | [54. Spiral Matrix](#54-spiral-matrix)     |
|     🟠      | [59. Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/) | [59. Spiral Matrix II](#59-spiral-matrix-ii)     |

### 48. Rotate Image

You are given an `n x n` 2D `matrix` representing an image, rotate the image by **90** degrees (clockwise).

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg)

```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/08/28/mat2.jpg)

```
Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```

 

**Constraints:**

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 20`
- `-1000 <= matrix[i][j] <= 1000`

---

```cpp
class Solution {
  public:
  	void rotate(vector<vector<int>>& matrix) {
      int n = matrix.size();
      for(int i = 0; i < n; i++) {
        for(int j = i; j < n; j++) {
          int temp = matrix[i][j];
          matrix[i][j] = matrix[j][i];
          matrix[j][i] = temp;
        }
      }
      for(int i = 0; i < n; i++) {
        reverse(matrix[i])
      }
    }
  void reverse(vector<int>& v) {
    int i = 0;
    int j = v.size() - 1;
    while(i < j) {
      int temp = v[i];
      v[i] = v[j];
      v[j] = temp;
      i++;
      j--;
    }
  }
};
```

### 54. Spiral Matrix

Given an `m x n` `matrix`, return *all elements of the* `matrix` *in spiral order*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)

```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,3,6,9,8,7,4,5]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiral.jpg)

```
Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

 

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 10`
- `-100 <= matrix[i][j] <= 100`

---

```cpp
class Solution {
  private:
  	vector<int> res;
  public:
  	vector<int> spiralOrder(vector<vector<int>>& matrix) {
      int m = matrix.size();
      int n = matrix[0].size();
      int upper = 0;
      int lower = m-1;
      int left = 0;
      int right = n -1;
      while(res.size() < m*n) {
        if(upper <= lower) {
          for(int i = left; i <= right; i++) {
            res.push_back(matrix[upper][i]);
          }
          upper++;
        }
        if(left <= right) {
          for(int i = upper; i <= lower; i++) {
            res.push_back(matrix[i][right]);
          }
          right--;
        }
        if(upper <= lower) {
          for(int i = right; i >= left; i--) {
            res.push_back(matrix[lower][i]);
          }
          lower--;
        }
        if(left <= right) {
          for(int i = lower; i >= upper; i--) {
            res.push_back(matrix[i][left])
          }
          left++;
        }
      }
      return res;
    }
};
```

### 59. Spiral Matrix II

Given a positive integer `n`, generate an `n x n` `matrix` filled with elements from `1` to `n2` in spiral order.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

```
Input: n = 3
Output: [[1,2,3],[8,9,4],[7,6,5]]
```

**Example 2:**

```
Input: n = 1
Output: [[1]]
```

 

**Constraints:**

- `1 <= n <= 20`

---

```cpp
class Solution {
private:
    vector<vector<int>> matrix;
public:
    vector<vector<int>> generateMatrix(int n) {
        int upper = 0;
        int lower = n-1;
        int left = 0;
        int right = n-1;
        int num = 1;
        matrix.resize(n);
        for(int i = 0; i < n; i++) {
            matrix[i].resize(n);
        }
        while(num <= n * n) {
            if(upper <= lower) {
                for(int i = left; i <= right; i++) {
                    matrix[upper][i] = num++;
                }
                upper++;
            }
            if(left <= right) {
                for(int i = upper; i <= lower; i++) {
                    matrix[i][right] = num++;
                }
                right--;
            }
            if(upper <= lower) {
                for(int i = right; i >= left; i--) {
                    matrix[lower][i] = num++;
                }
                lower--;
            }
            if(left <= right) {
                for(int i = lower; i >= upper; i--) {
                    matrix[i][left] = num++;
                }
                left++;
            }
        }
        return matrix;
    }
};
```

## Sliding window

| Difficulty |                           LeetCode                           | Note |
| :--------: | :----------------------------------------------------------: | :--: |
|     🔴      | [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) | [76. Minimum Window Substring](#76-minimum-window-substring)     |
|     🟠      | [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/) |[567. Permutation in String](#567-permutation-in-string)      |
|     🟠      | [438. Find Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/) |[438. Find Anagrams in a String](#438-find-all-anagrams-in-string)      |
|     🟠      | [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) |[3. Longest Substring Without Repeating Characters](#3-longest-substring-without-repeating-characters)      |

### 76. Minimum Window Substring

Given two strings `s` and `t` of lengths `m` and `n` respectively, return *the **minimum window substring** of* `s` *such that every character in* `t` *(**including duplicates**) is included in the window. If there is no such substring**, return the empty string* `""`*.*

The testcases will be generated such that the answer is **unique**.

A **substring** is a contiguous sequence of characters within the string.

 

**Example 1:**

```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

**Example 2:**

```
Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.
```

**Example 3:**

```
Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.
```

 

**Constraints:**

- `m == s.length`
- `n == t.length`
- `1 <= m, n <= 105`
- `s` and `t` consist of uppercase and lowercase English letters.

---

```cpp
class Solution {
  public:
  	string minWindow(string s, string t) {
      map<char, int> need;
      map<char, int> window;
      for(char c : t) {
        need[c]++;
      }
      int left = 0;
      int right = 0;
      int valid = 0;
      int start = 0;
      int len = INT_MAX;
      while(right < s.size()) {
        char c = s[right];
        if(need.count(c)) {
          window[c]++;
          if(window[c] == need[c]) {
            valid++;
          }
        }
        while(valid == need.size()) {
          if(right - left < len) {
            start = left;
            len = right - left;
          }
          char d = s[left];
          left++;
          if(need.count(d)) {
            if(window[d] == need[d]) {
              valid--;
            }
            window[d]--;
          }
        }
      }
      return len == INT_MAX ? "" : s.substr(start, len);
    }
};
```

### 567. Permutation in String

Given two strings `s1` and `s2`, return `true` *if* `s2` *contains a permutation of* `s1`*, or* `false` *otherwise*.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

 

**Example 1:**

```
Input: s1 = "ab", s2 = "eidbaooo"
Output: true
Explanation: s2 contains one permutation of s1 ("ba").
```

**Example 2:**

```
Input: s1 = "ab", s2 = "eidboaoo"
Output: false
```

 

**Constraints:**

- `1 <= s1.length, s2.length <= 104`
- `s1` and `s2` consist of lowercase English letters.

---

```cpp
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        map<char, int> need;
        map<char, int> window;
        for(char c : s1) {
            need[c]++;
        }
        int left = 0;
        int right = 0;
        int valid = 0;
        while(right < s2.size()) {
            char c = s2[right];
            right++;
            if(need.count(c)) {
                window[c]++;
                if(window[c] == need[c]) {
                    valid++;
                }
            }
            while(right - left >= s1.size()) {
                if(valid == need.size()) {
                    return true;
                }
                char d = s2[left];
                left++;
                if(need.count(d)) {
                    if(window[d] == need[d]) {
                        valid--;
                    }
                    window[d]--;
                }
            }
        }
        return false;
    }
};
```

### 438. Find All Anagrams in String

Given two strings `s` and `p`, return *an array of all the start indices of* `p`*'s anagrams in* `s`. You may return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

 

**Example 1:**

```
Input: s = "cbaebabacd", p = "abc"
Output: [0,6]
Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

**Example 2:**

```
Input: s = "abab", p = "ab"
Output: [0,1,2]
Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

 

**Constraints:**

- `1 <= s.length, p.length <= 3 * 104`
- `s` and `p` consist of lowercase English letters.

---

```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        map<char, int> need;
        map<char, int> window;
        vector<int> res;
        for(char c : p) {
            need[c]++;
        }
        int left = 0;
        int right = 0;
        int valid = 0;
        while(right < s.size()) {
            char c = s[right];
            right++;
            if(need.count(c)) {
                window[c]++;
                if(window[c] == need[c]) {
                    valid++;
                }
            }
            while(right - left >= p.size()) {
                if(valid == need.size()) {
                    res.push_back(left);
                }
                char d = s[left];
                left++;
                if(need.count(d)) {
                    if(window[d] == need[d]) {
                        valid--;
                    }
                    window[d]--;
                }
            }
        }
        return res;
    }
};
```

### 3. Longest Substring Without Repeating Characters

Given a string `s`, find the length of the **longest substring** without repeating characters.

 

**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2:**

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

 

**Constraints:**

- `0 <= s.length <= 5 * 104`
- `s` consists of English letters, digits, symbols and spaces.

---

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        map<char, int> window;
        int left = 0;
        int right = 0;
        int res = 0;
        while(right < s.size()) {
            char c = s[right];
            right++;
            window[c]++;
            while(window[c] > 1) {
                char d = s[left];
                left++;
                window[d]--;
            }
            res = max(res, right - left);
        }
        return res;
    }
};
```

## Binary Search



