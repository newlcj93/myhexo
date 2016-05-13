---
title: Ananagrams
date: 2016-05-13 17:14:19
tags: UVa
---
# 反片语(Ananagrams)
> 输入一些单词，找出所有满足如下条件的单词：该单词不能通过字母重排，得到输入文本中的另外一个单词。在判断是否满足条件时，字母不分大小写，
但在输出时应保留输入中的大小写，按字典进行排序（所有大写字母在所有小写字母的前面）。
<!--more-->
>**样例输入**：
ladder came tape soon leader acme RIDE lone Dreis peat
ScAlE orb eye Rides dealer NotE derail LaCeS drIed
noel dire Disk mace Rob dries
#

>**样例输出**：
Disk
NotE
derail
drIed
eye
ladder
soon

**分析**
    把每个单词“标准化”，即全部转化为小写字母后再进行排序，然后再放到map中进行统计。map映射不含相同的键值，其中map还包含一个count方法：用于判断map中是否含有一个键值key，如果map中含有则返回1，没有则返回0.

---------
```C
 #include<iostream>
 #include<string>
 #include<vector>
 #include<map>
 #include<algorithm>
 using namespace std;
 
 vector<string> words;
 map<string, int> cnt;
 //将单词s进行“标准化”
 string repr(const string s) {
     string ans = s;
     for (int i = 0; i < ans.length(); i++)
     ans[i] = tolower(ans[i]);
     sort(ans.begin(), ans.end());
     return ans;
 }
 
 int main() {
 string s;
 while (cin >> s && s[0] != '#') {
     words.push_back(s);
     string r = repr(s);
     //判断map中是否已含有r这个key，如果没有则将新的键值对加入到map中
     if (!cnt.count(r)) cnt[r] = 0;
        cnt[r]++;
     }
     vector<string> ans;
     for (int i = 0; i < words.size(); i++) {
         if (cnt[repr(words[i])] == 1) ans.push_back(words[i]);
     }
     sort(ans.begin(), ans.end());
     for (int i = 0; i < ans.size(); i++){
         cout << ans[i] << endl;
     }
     return 0;
 }

```