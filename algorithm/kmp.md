- **What is KMP used for?**

  KMP is used for handling string matching and relative problems. The basic form is to detect whether a pattern "p" is a substring of a give string "s". If so, find out the index of the starting character in "s".

- **Naive Solution**

  Start matching from the first character in s. If the matching fails, start again from the second character in s. If the length of s is n and the length os p is m, it takes `O(nm)` time.

- **Intuition**

  Give a pattern "abcabcd", and a string "abcabcabcd", the matching goes on quite well on first 6 characters "abcabc". However, the seventh "d" and "a" don't match. Notice, in the matched part "abcabc", there exists a **suffix** "abc" that is also the **prefix** of the pattern. This observation tells us that we don't have to restart the matching from the very beginning. Instead, we can make use of this.

- **Implementation**

  ```python
  def kmp(s: str, p: str) -> int:
      s_len, p_len = len(s), len(p)
      # array m[i] represents the length of suffix that is
      # also the prefix of s in the first i characters    
      # m[0] has to be 0; obviously, a single character is 
      # both the prefix and suffix of itself. It cannot help
      # us expedite the matching
      m = [0] * p_len
  
      # to construct m, we will do matching between pattern and itself
      i, j = 1, 0
      while i < p_len:
          # if the matching goes well
          if p[i] == p[j]:
              # since m[i] is the length, so it should be the index
              # plus one
              m[i] = j + 1
              i += 1
              j += 1
          else:
              # make use of the array m to move the pointer to a
              # proper position
              if j > 0:
                  j = m[j - 1]
              # if the matching fails on the first character, s[i]
              # is definitely not part of the matching
              else:
                  i += 1
      i, j = 0, 0
      while i < s_len and j < p_len:
          if s[i] == p[j]:
              i += 1
              j += 1
          else:
              if j > 0:
                  j = m[j - 1]
              else:
                  i += 1
  
      if j == p_len:
          return i - p_len
  
      return -1
  ```

- Practices:

  Leetcode 28, 459, 686

- Notes:

  - In python, self-implemented kmp is faster than std interface`find` (Tested on Leetcode).

