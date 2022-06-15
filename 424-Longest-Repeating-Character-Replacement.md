424. You are given a string s and an integer k. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most k times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

Example 1:

Input: s = "ABAB", k = 2
Output: 4
Explanation: Replace the two 'A's with two 'B's or vice versa.

Example 2:

>Input: s = "AABABBA", k = 1  
Output: 4  
Explanation: Replace the one 'A' in the middle with 'B' and form "AABBBBA".  
The substring "BBBB" has the longest repeating letters, which is 4.  
 

Constraints:
>1 <= s.length <= 105
>s consists of only uppercase English letters.
>0 <= k <= s.length

### Idea:
Sliding window

Solution: 

```java
public int characterReplacement(String s, int k) {
    int[] arr = new int[26];
    int i = 0, j = 0, res = 1;
    while (j < s.length()) {
        arr[s.charAt(j) - 'A']++;
        if (j - i + 1 - maxCount(arr) <= k) {
            res = Math.max(res, j - i + 1);
        } else {
            while (j - i + 1 - maxCount(arr) > k) {
                arr[s.charAt(i) - 'A']--;
                i++;
            }
        }
        j++;
    }
    return res;
}
public int maxCount(int[] arr) {
    int res = 0;
    for (int a : arr) {
        res = Math.max(res, a);
    }
    return res;
}
```