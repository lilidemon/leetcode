### 题目:
This keypad can be connected to any electronic device and has 9 buttons, where each button can have up to 3 lowercase English letters. The buyer has the freedom to choose which letters to place on a button while ensuring that the arrangement is valid. A key design is said to be valid if :
- All 26 letters of the English alphabet exist on the keypad.
- Each letter is mapped to exactly one button.
- A button has at most 3 letters mapped to it.

The keypad click count is defined as the number of button presses required to print a given string. In order to send message faster, customers tend to set the keypad design in such a way that the keypad click count is minimized while maintaining its validity.

Given a string text consisting of lowercase English letters only, find the minimum keypad click count.

Example: text = "abacadefghibj"

给一串字符，"abcdefgabc"，然后用手机九宫格打出。手机九宫格可以是任何字母组合，唯一要求是每个键至少有两个字母，最多三个字母。换句话说，这里的键位不一定是我们常见的2=abc，3=def等等，可以是1=agq，2=bhj...任何顺序都可以。要求找出能打出输入字符串的最少按键次数。
————————————————
思路: 统计各字母频数,按频数排序,尽量把频数高的字母放在9个数字的第一个位置

数据结构: 哈希表/数组(统计频数) + 哈希表值排序/优先队列

//Map sort by value
List<Map.Entry<String,String>> list = new ArrayList<>(map.entrySet());
Collections.sort(list, new Comparator<Map.Entry<String,String>>() {
    public int compare(Map.Entry<String, String> o1, Map.Entry<String, String> o2) {
        return o1.getValue().compareTo(o2.getValue());
    }  
});

TreeMap(default: sort by key in increasing order)
//sort by key in decreasing order
Map<String, String> map = new TreeMap<String, String>(new Comparator<String>() {
    public int compare(String obj1, String obj2) {
        return obj2.compareTo(obj1);
    }
});

[leetcode 451. Sort Characters By Frequency] (https://leetcode.com/problems/sort-characters-by-frequency/)

### Algorithm 1: HashMap sorted by value

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

### Algorithm 1: HashMap + PriorityQueue

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