+++
title = "443. String Compression"
date = "2025-11-11"

[taxonomies]
tags=["leetcode"]

[extra]
repo_view = true
comment = false
+++


## [String Compression](https://leetcode.com/problems/string-compression/description/)

This problem is categorized as **medium** difficulty because it requires solving it in **constant space**. At first glance, it might remind you of **Run-Length Encoding** (RLE), a well-known technique that replaces consecutive identical characters (appearing 2 or more times) with the character itself followed by the count of its occurrences.

The challenge here is to perform this compression directly on the input vector of characters without using any extra space (i.e., in-place modification). The solution can be achieved by maintaining a `write_index` and counting the occurrences of consecutive identical characters. Once the count is determined, the current character is placed at the `write_index`, followed by its count (if greater than 1). This process continues for all characters in the input vector.

---

### C++ Code:

```c++
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

class Solution {
  public:
    int compress(vector<char>& chars) {
      int write_index = 0;
      int i = 0;
      int str_length = chars.size();

      cout << "str_length : " << str_length << endl;

      while (i < str_length) {
        int count = 1;
        char current_char = chars[i];

        // Count consecutive characters
        while (count + i < str_length && current_char == chars[count + i]) {
          count++;
        }

        cout << chars[i] << " count : " << count << "\n";

        // Write the current character to the write_index
        chars[write_index++] = current_char;

        // If count is more than 1, write the count as well
        if (count > 1) {
          for (char digit : to_string(count)) {
            chars[write_index++] = digit;
          }
        }

        // Move i forward by the count of consecutive characters
        i += count;
      }

      return write_index;
    }
};

int main() {
  vector<char> chars = {'a', 'a', 'b', 'b', 'c', 'c', 'c', 'c'};
  
  Solution sol;
  
  int result = sol.compress(chars);
  cout << result << endl;
  return 0;
}
```

### Breakdown of the Approach:

- *Initialize pointers*: We start with write_index at 0 and iterate through the vector using the pointer i.

- *Count consecutive characters*: For each group of consecutive characters, count how many times the current character repeats.

- *Update the vector in-place*: Place the character at the write_index, and if the count is greater than 1, write the digits of the count after the character.

- *Return the new length*: The function returns the write_index, which now represents the new length of the compressed string.

This approach efficiently compresses the string in-place with O(n) time complexity and O(1) extra space complexity, where n is the length of the input vector.