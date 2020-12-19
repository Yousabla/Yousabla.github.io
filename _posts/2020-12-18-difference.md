---
title: 找不同
author: yousabla
date: 2020-12-18 17:27:00 +0800
categories: [Algorithm,HashMap]
tags: [Algorithm,HashMap,ASCII]
pin: true
---

2020-12-18 Leetcode 

每日一题

389.简单.找不同

给定两个字符串 s 和 t，它们只包含小写字母。

字符串 t 由字符串 s 随机重排，然后在随机位置添加一个字母。

请找出在 t 中被添加的字母。

```
示例 1：

输入：s = "abcd", t = "abcde"
输出："e"
解释：'e' 是那个被添加的字母。
示例 2：

输入：s = "", t = "y"
输出："y"
示例 3：

输入：s = "a", t = "aa"
输出："a"
示例 4：

输入：s = "ae", t = "aea"
输出："a"
```


提示：

0 <= s.length <= 1000
t.length == s.length + 1
s 和 t 只包含小写字母

PS:审题太不仔细了,甚至没看到第三个示例就开始做.

第一个想法就是HashSet来存,结果当然不对,第三个示例就排除了这个做法.

接下来就是最蠢的解法:

```java
public char findTheDifference(String s, String t) {
        Character res = ' ';
        Map<Character,Integer> map = new HashMap<>();
        for (Character c:s.toCharArray()) {
            map.put(c,map.getOrDefault(c,0)+1);
        }
        for (Character c:t.toCharArray()) {
            if (map.containsKey(c)){
                if (map.get(c) > 1){
                    map.put(c,map.get(c)-1);
                }else{
                    map.remove(c);
                }
            }else{
                res = c;
            }
        }
        return res;
}
```

也就是计数.

看了官方题解,觉得ASCII码的解法很棒!(我本也应该想的到的)

```java
public char findTheDifference(String s, String t) {
        int as = 0,at = 0;
        for (Character c:s.toCharArray()) {
            as += c;
        }
        for (Character c:t.toCharArray()) {
            at += c;
        }
        return (char)(at-as);
}
```

太简单了没啥好说的.