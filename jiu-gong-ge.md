### Description:
>This keypad can be connected to any electronic device and has 9 buttons, where each button can have up to 3 lowercase English letters. The buyer has the freedom to choose which letters to place on a button while ensuring that the arrangement is valid. A key design is said to be valid if :  
>- All 26 letters of the English alphabet exist on the keypad.
>- Each letter is mapped to exactly one button.
>- A button has at most 3 letters mapped to it.
>The keypad click count is defined as the number of button presses required to print a given string. In order to send message faster, customers tend to set the keypad design in such a way that the keypad click count is minimized while maintaining its validity.  
Given a string text consisting of lowercase English letters only, find the minimum keypad click count.  
Example: text = "abacadefghibj"

### Idea: 
>Count the prequency of each Character, sort Character by prequency in decresing order, put the Character with higher frequency in the first position of 9 numbers as possible

### Data Structure: 
>HashMap/Array(count the prequency of each character) + sort HashMap by value(Collections.sort/PriorityQueue/Arrays.sort)

```diff
+ ### Knowledge related
```
>#### 1. Map sort by value
```java
List<Map.Entry<String,String>> list = new ArrayList<>(map.entrySet());  
Collections.sort(list, new Comparator<Map.Entry<String,String>>() {  
    public int compare(Map.Entry<String, String> o1, Map.Entry<String, String> o2) {  
        return o1.getValue().compareTo(o2.getValue());  
    }   
});  
```
>#### 2. TreeMap(default: sort by key in increasing order)  
```java
//sort by key in decreasing order  
Map<String, String> map = new TreeMap<String, String>(new Comparator<String>() {  
    public int compare(String obj1, String obj2) {  
        return obj2.compareTo(obj1);  
    }  
});  
```
>#### 3. Improve efficiency
StringBuffer is way more faster than String  
String: 2789ms  StringBuffer: 32ms

### [leetcode 451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

### Algorithm 1: HashMap + Collections.sort
```java
public int minCount(String s) {  
    Map<Character, Integer> map = new HashMap<>();  
    for (char c : s.toCharArray()) {  
        map.put(c, map.getOrDefault(c, 0) + 1);  
    }  
    List<Map.Entry<Character, Integer>> list = new ArrayList<>(map.entrySet());  
    Collections.sort(list, new Comparator<Map.Entry<Character, Integer>>() {  
        public int compare(Map.Entry<Character, Integer> o1, Map.Entry<Character, Integer> o2) {  
            return o2.getValue().compareTo(o1.getValue());  
        }  
    });  
    int res = 0;  
    for (int i = 0; i < list.size(); i++) {  
        int pos = i / 9 + 1;  
        res += pos * list.get(i).getValue();  
    }  
    return res;  
}
```
### Algorithm 2: HashMap + PriorityQueue
```java
public int minCount(String s) {  
    Map<Character, Integer> map = new HashMap<>();  
    for (char c : s.toCharArray()) {  
        map.put(c, map.getOrDefault(c, 0) + 1);  
    }  
    PriorityQueue<Character> pq = new PriorityQueue<>(new Comparator<Character>() {  
        public int compare(Character c1, Character c2) {  
            if (c1 != c2) return map.get(c2) - map.get(c1);  
            else return c1 - c2;  
        }  
    });  
    for (char k : map.keySet()) {  
        pq.offer(k);  
    }  
    int res = 0, i = 0;  
    while (!pq.isEmpty()) {  
        int value = map.get(pq.poll());  
        int pos = i / 9 + 1;  
        res += pos * value;  
        i++;  
    }  
    return res;  
}  
```
### Algorithm 3: Array + Arrays.sort
```java
public int minCount(String s) {
    int[][] freq = new int[128][2];
    char[] arr = s.toCharArray();
    for (int i = 0; i < 128; i++) {
        freq[i][0] = i;
    }
    for (char c : arr) {
        freq[c][1]++;
    }
    Arrays.sort(freq, (a, b) -> {
        if (a[1] != b[1]) return b[1] - a[1];
        else return a[0] - b[0];
    });
    int res = 0;                       
    for (int i = 0; i < 128; i++) {    
        int k = freq[i][1];            
        res += (i / 9 + 1) * k;        
    }
    return res;
}
```