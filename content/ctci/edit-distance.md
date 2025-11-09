+++
title = "72. Edit Distance"
date = "2025-11-09"

[taxonomies]
tags=["leetcode"]

[extra]
repo_view = true
comment = false
+++


## Edit Distance

The problem given in this question can be solved by [levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance) using full matrix approach.
Make a matrix of size (n+1, m+1), where n & m are the sizes of both words respectively and fill the first column and first row their indices while keeping other 
positions 0.

```bash
|    | "" | r | o | s |
| -- | -- | - | - | - |
| "" | 0  | 1 | 2 | 3 |
| h  | 1  | ? | ? | ? |
| o  | 2  | ? | ? | ? |
| r  | 3  | ? | ? | ? |
| s  | 4  | ? | ? | ? |
| e  | 5  | ? | ? | ? |
```

Each (i, j) position (for i, j >= 1) will represent the levenshtein distance between words s1 and s2 where by calculating the cost of transforming the first i characters of s1 into the first j characters of s2. The value of each cell depends on the cost of insertion, deletion and substitution.
 The value in the bottom-right corner of the matrix (`matrix[s1.size()][s2.size()]`)

```bash
|    | "" | r | o | s |
| -- | -- | - | - | - |
| "" | 0  | 1 | 2 | 3 |
| h  | 1  | 1 | 2 | 3 |
| o  | 2  | 2 | 1 | 2 |
| r  | 3  | 2 | 2 | 2 |
| s  | 4  | 3 | 3 | 2 |
| e  | 5  | 4 | 4 | 3 |
```


## C++ code

```c++
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

class Solution{
  public:
    int minDistance(string word1,  string word2){
      
      int len_1 = word1.size();
      int len_2 = word2.size();

      vector<vector<int>> mat(len_1+1, vector<int>(len_2 + 1, 0));

      for(int i=0; i<=len_1; i++){
        mat[i][0] = i;
      }

      for(int i=0; i<=len_2; i++){
        mat[0][i] = i;
      }

      for (int i=1; i<=len_1; i++){
        for (int j=1; j<=len_2; j++){
          int cost = word1[i-1] == word2[j-1] ? 0 : 1;
          mat[i][j] = min({
              mat[i-1][j] + 1,
              mat[i][j-1] + 1,
              mat[i-1][j-1] + cost
              });
        }
      }
      return mat[len_1][len_2];
    }
};

int main(){
  string word1 = "horse";
  string word2 = "ros";
  
  Solution sol;

  int result = sol.minDistance(word1, word2);
  cout<<result<<endl;
  return 0;
}

```

Other test cases : 

```bash
word1 = "intention"
word2 = "execution"
```

```bash
word1 = "a"
word2 = "aa"
```