# 2911. Minimum Changes to Make K Semi-palindromes

**Difficulty:** Hard
**Link:** https://leetcode.com/problems/minimum-changes-to-make-k-semi-palindromes/

## Problem

Given a string `s` and an integer `k`, partition `s` into `k` substrings such that the letter changes needed to make each substring a semi-palindrome are minimized.

Return the minimum number of letter changes required.

A semi-palindrome is a special type of string that can be divided into palindromes based on a repeating pattern. To check if a string is a semi-palindrome:

1. Choose a positive divisor `d` of the string's length. `d` can range from `1` up to, but not including, the string's length. For a string of length `1`, it does not have a valid divisor as per this definition, since the only divisor is its length, which is not allowed.
2. For a given divisor `d`, divide the string into groups where each group contains characters from the string that follow a repeating pattern of length `d`. Specifically, the first group consists of characters at positions `1`, `1 + d`, `1 + 2d`, and so on; the second group includes characters at positions `2`, `2 + d`, `2 + 2d`, etc.
3. The string is considered a semi-palindrome if each of these groups forms a palindrome.

Consider the string `"abcabc"`:

- The length of `"abcabc"` is `6`. Valid divisors are `1`, `2`, and `3`.
- For `d = 1`: The entire string `"abcabc"` forms one group. Not a palindrome.
- For `d = 2`:
  - Group 1 (positions `1, 3, 5`): `"acb"`
  - Group 2 (positions `2, 4, 6`): `"bac"`
  - Neither group forms a palindrome.
- For `d = 3`:
  - Group 1 (positions `1, 4`): `"aa"`
  - Group 2 (positions `2, 5`): `"bb"`
  - Group 3 (positions `3, 6`): `"cc"`
  - All groups form palindromes. Therefore, `"abcabc"` is a semi-palindrome.

### Example 1

```
Input: s = "abcac", k = 2
Output: 1
Explanation: Divide s into "ab" and "cac". "cac" is already semi-palindrome.
Change "ab" to "aa", it becomes semi-palindrome with d = 1.
```

### Example 2

```
Input: s = "abcdef", k = 2
Output: 2
Explanation: Divide s into substrings "abc" and "def".
Each needs one change to become semi-palindrome.
```

### Example 3

```
Input: s = "aabbaa", k = 3
Output: 0
Explanation: Divide s into substrings "aa", "bb" and "aa".
All are already semi-palindromes.
```

### Constraints

- `2 <= s.length <= 200`
- `1 <= k <= s.length / 2`
- `s` contains only lowercase English letters.

## Approach

Precompute the cost of every substring, then run a partition DP.

1. **Factors:** for each length `len`, list its proper divisors (including `1`, excluding `len` itself).
2. **Substring cost:** `cost[i][j]` is the minimum number of changes to make `s[i..j]` a semi-palindrome. For each proper divisor `d` of the length, split the substring into `d` groups of characters spaced `d` apart. Count the mismatched pairs when checking each group as a palindrome from both ends, and take the minimum over all `d`.
3. **DP:** `dp[i][j]` is the minimum changes to split `s[i:]` into `j` valid parts. Try every end `l` for the first part: `dp[i][j] = min(dp[l + 1][j - 1] + cost[i][l])`. Parts of length 1 are skipped because a single character has no valid divisor.
4. The answer is `dp[0][k]`.

**Time:** about O(n³ log n) for the cost table plus O(n² · k) for the DP
**Space:** O(n²)

## Solution

See [`Solution.java`](./Solution.java).
