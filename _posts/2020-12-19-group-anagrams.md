---
title: 字母异位词分组
author: yousabla
date: 2020-12-18 19:36:00 +0800
categories: [Algorithm,ASCII]
tags: [Algorithm,ASCII]
pin: true
---

2020-12-19 Leetcode 

每日一题

49.中等 字母异位词分组

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

```
示例:

输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```


说明：

所有输入均为小写字母。
不考虑答案输出的顺序。

------

当然，不用我多说你也知道我肯定第一次又想错了。

你肯定还记得昨天咱们做的那道找不同，用ASCII码的数字和来解题。

我这么棒，直接举一反三：

```Java
public List<List<String>> groupAnagrams(String[] strs) {
        List<List<String>> ans = new ArrayList<>();
        if (strs.length == 0){
            return ans;
        }
        HashMap<Integer,List<String>> map = new HashMap<>();
        for(String str:strs){
            int sum = 0;
            for(Character c:str.toCharArray()){
                sum += c;
            }
            if(map.containsKey(sum)){
                map.get(sum).add(str);
            }else{
                List<String> temp = new ArrayList<>();
                temp.add(str);
                map.put(sum,temp);
            }
        }
        map.forEach((key, value) -> ans.add(value));
        return ans;
    }
```

当然，这种解法是错的，我觉得这个测试用例相当刁钻：

```
["cab","tin","pew","duh","may","ill","buy","bar","max","doc"]
```

其中“duh”和“ill”的ASCII数字和是相同的。

于是我做了如下修改，把ASCII数字和改成了ASCII数字集：

```java
public List<List<String>> groupAnagrams(String[] strs) {
        List<List<String>> ans = new ArrayList<>();
        if (strs.length == 0){
            return ans;
        }
        HashMap<HashSet<Integer>,List<String>> map = new HashMap<>();
        for(String str:strs){
            HashSet<Integer> set = new HashSet<>();
            for(Character c:str.toCharArray()){
                set.add((int)c);
            }
            if(map.containsKey(set)){
                map.get(set).add(str);
            }else{
                List<String> temp = new ArrayList<>();
                temp.add(str);
                map.put(set,temp);
            }
        }
        map.forEach((key, value) -> ans.add(value));
        return ans;
    }
```

当然还是不对，给我来了个更离谱的测试用例：

```
["ddddddddddg","dgggggggggg"]
```

彳亍。

那这样一来这法子是肯定行不通了啊。

也不是不行，题解给出了一种很牛的法子，这种思想我曾在其他的题解中窥过一丝，即使用长度为26的int数组来保存单词：

```Java
public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        for (String str : strs) {
            int[] counts = new int[26];
            int length = str.length();
            for (int i = 0; i < length; i++) {
                counts[str.charAt(i) - 'a']++;
            }
            // 将每个出现次数大于 0 的字母和出现次数按顺序拼接成字符串，作为哈希表的键
            StringBuffer sb = new StringBuffer();
            for (int i = 0; i < 26; i++) {
                if (counts[i] != 0) {
                    sb.append((char) ('a' + i));
                    sb.append(counts[i]);
                }
            }
            String key = sb.toString();
            List<String> list = map.getOrDefault(key, new ArrayList<String>());
            list.add(str);
            map.put(key, list);
        }
        return new ArrayList<List<String>>(map.values());
    }
```

另一种思路就是排序，字符相同排序过后字符串也一定相同：

```Java
public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        for (String str : strs) {
            char[] array = str.toCharArray();
            Arrays.sort(array);
            String key = new String(array);
            List<String> list = map.getOrDefault(key, new ArrayList<String>());
            list.add(str);
            map.put(key, list);
        }
        return new ArrayList<List<String>>(map.values());
    }
```

