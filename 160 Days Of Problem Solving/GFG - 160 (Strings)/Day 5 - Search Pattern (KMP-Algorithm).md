---
Difficulty: Medium
Source: 160 Days of Problem Solving
Tags:
  - Strings
  - Pattern Searching
---

# 🚀 _Day 5. Search Pattern (KMP Algorithm)_ 🧠

The problem can be found at the following link: [Problem Link](https://www.geeksforgeeks.org/batch/gfg-160-problems/track/string-gfg-160/problem/search-pattern0205)

## 💡 **Problem Description:**

You are given two strings:

- `txt`: The text string in which the pattern is to be searched.
- `pat`: The pattern string to search for.

The task is to print all indices in `txt` where `pat` starts, using 0-based indexing. Return an empty list if no occurrences are found.

## 🔍 **Example Walkthrough:**

**Input:**  
`txt = "abcab", pat = "ab"`  
**Output:**  
`[0, 3]`

**Explanation:**  
The pattern `ab` appears at indices 0 and 3 in the text string.

**Input:**  
`txt = "aaaaa", pat = "aa"`  
**Output:**  
`[0, 1, 2, 3]`

**Input:**  
`txt = "hello", pat = "world"`  
**Output:**  
`[]`

## Constraints:

- `1 <= txt.length, pat.length <= 10^6`
- Both strings consist of lowercase English alphabets.

## 🎯 **My Approach:**

The **Knuth-Morris-Pratt (KMP)** algorithm is an efficient pattern matching algorithm that avoids unnecessary comparisons, making it well-suited for this task.

1. **Compute the Longest Prefix Suffix (LPS) Array:**

   - Preprocess the pattern string `pat` to build the LPS array.
   - The LPS array stores the length of the longest prefix which is also a suffix for substrings of `pat`.
   - This preprocessing helps in skipping characters during comparisons.

2. **Search Using the LPS Array:**

   - Traverse the `txt` and use the `pat` LPS array to efficiently find matches.
   - If a mismatch occurs, use the LPS array to skip unnecessary comparisons.

3. **Output the Indices:**
   - Store the starting indices of matches found in `txt`.

## 🕒 **Time and Auxiliary Space Complexity**

- **Preprocessing (LPS Array):** O(m), where `m` is the length of the pattern string.
- **Searching:** O(n), where `n` is the length of the text string.
- **Overall Time Complexity:** O(n + m).

### Auxiliary Space Complexity:

- **Space for LPS Array:** O(m).

## 📝 **Solution Code**

## Code (C)

```c
void computeLPSArray(char* pat, int m, int* lps) {
    int len = 0;
    int i = 1;
    lps[0] = 0;

    while (i < m) {
        if (pat[i] == pat[len]) {
            len++;
            lps[i] = len;
            i++;
        } else {
            if (len != 0) {
                len = lps[len - 1];
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }
}

int* search(char pat[], char txt[], int* resultSize) {
    int m = strlen(pat);
    int n = strlen(txt);

    int* lps = (int*)malloc(m * sizeof(int));
    if (!lps) {
        fprintf(stderr, "Memory allocation failed for lps\n");
        exit(1);
    }

    computeLPSArray(pat, m, lps);

    int* result = (int*)malloc(10 * sizeof(int));
    if (!result) {
        fprintf(stderr, "Memory allocation failed for result\n");
        free(lps);
        exit(1);
    }
    int resCapacity = 10;
    int resIndex = 0;

    int i = 0, j = 0;
    while (i < n) {
        if (txt[i] == pat[j]) {
            i++;
            j++;
        }

        if (j == m) {
            if (resIndex >= resCapacity) {
                resCapacity *= 2;
                result = (int*)realloc(result, resCapacity * sizeof(int));
                if (!result) {
                    fprintf(stderr, "Memory reallocation failed for result\n");
                    free(lps);
                    exit(1);
                }
            }
            result[resIndex++] = i - j;
            j = lps[j - 1];
        } else if (i < n && txt[i] != pat[j]) {
            if (j != 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }
    }
    *resultSize = resIndex;
    free(lps);

    return result;
}
```

## Code (C++)

```cpp
class Solution {
  public:
    vector<int> search(string& pat, string& txt) {
        int m = pat.size();
        int n = txt.size();
        vector<int> lps(m, 0);
        vector<int> result;
        int len = 0;
        for (int i = 1; i < m; ) {
            if (pat[i] == pat[len]) {
                len++;
                lps[i] = len;
                i++;
            } else {
                if (len != 0) {
                    len = lps[len - 1];
                } else {
                    lps[i] = 0;
                    i++;
                }
            }
        }
        int i = 0;
        int j = 0;
        while (i < n) {
            if (txt[i] == pat[j]) {
                i++;
                j++;
            }
            if (j == m) {
                result.push_back(i - j);
                j = lps[j - 1];
            } else if (i < n && txt[i] != pat[j]) {
                if (j != 0) {
                    j = lps[j - 1];
                } else {
                    i++;
                }
            }
        }
        return result;
    }
};
```

## Code (Java)

```java
class Solution {
    private void computeLPSArray(String pat, int m, int[] lps) {
        int len = 0;
        int i = 1;
        while (i < m) {
            if (pat.charAt(i) == pat.charAt(len)) {
                len++;
                lps[i] = len;
                i++;
            } else {
                if (len != 0) {
                    len = lps[len - 1];
                } else {
                    lps[i] = 0;
                    i++;
                }
            }
        }
    }

    ArrayList<Integer> search(String pat, String txt) {
        ArrayList<Integer> result = new ArrayList<>();
        int m = pat.length();
        int n = txt.length();
        int[] lps = new int[m];

        computeLPSArray(pat, m, lps);

        int i = 0, j = 0;
        while (i < n) {
            if (txt.charAt(i) == pat.charAt(j)) {
                i++;
                j++;
            }

            if (j == m) {
                result.add(i - j);
                j = lps[j - 1];
            } else if (i < n && txt.charAt(i) != pat.charAt(j)) {
                if (j != 0) {
                    j = lps[j - 1];
                } else {
                    i++;
                }
            }
        }
        return result;
    }
}
```

## Code (Python)

```python
class Solution:
    def computeLPSArray(self, pat, m, lps):
        len = 0
        i = 1
        while i < m:
            if pat[i] == pat[len]:
                len += 1
                lps[i] = len
                i += 1
            else:
                if len != 0:
                    len = lps[len - 1]
                else:
                    lps[i] = 0
                    i += 1

    def search(self, pat, txt):
        result = []
        m = len(pat)
        n = len(txt)
        lps = [0] * m

        self.computeLPSArray(pat, m, lps)

        i = 0
        j = 0
        while i < n:
            if txt[i] == pat[j]:
                i += 1
                j += 1

            if j == m:
                result.append(i - j)
                j = lps[j - 1]
            elif i < n and txt[i] != pat[j]:
                if j != 0:
                    j = lps[j - 1]
                else:
                    i += 1
        return result
```

## 🎯 **Contribution and Support:**

For discussions, questions, or doubts related to this solution, feel free to connect on LinkedIn: [Any Questions](https://www.linkedin.com/in/patel-hetkumar-sandipbhai-8b110525a/). Let’s make this learning journey more collaborative!

⭐ If you find this helpful, please give this repository a star! ⭐

---

<div align="center">
  <h3><b>📍Visitor Count</b></h3>
</div>

<p align="center">
  <img src="https://profile-counter.glitch.me/Hunterdii/count.svg" />
</p>
