---
title: Ugly numbers
date: 2016-05-13 21:49:56
tags:
- UVa
- STL
- priority queue
---
# Ugly numbers
> 丑数是指不能被2、3、5以外的其他素数整除的数，把丑数从小到大排列起来，结果如下：  
1,2,3,4,5,6,8,9,10,12,15...  
求出第1500个丑数。
<!--more-->
**分析**：
   同一个丑数可以有多种生成方式，通过set判断一个丑数是否已经生成过。

------

```c
#include<iostream>
#include <set>
#include<queue>
typedef long long LL;
using namespace std;
int main() {
    LL x;
    const int a[3]={2,3,5};
    set<LL>s;
    priority_queue<LL,vector<LL>,greater<LL> >pq;
    s.insert(1);
    pq.push(1);
    for(int i=1;;i++){
        x=pq.top();
        pq.pop();
        if (i==1500) {
            cout<<x<<endl;
            break;
        }
        for (int j=0;j<3;j++){
            LL x2=x*(a[j]);
            if (!s.count(x2)) {
                s.insert(x2);
                pq.push(x2);
            }
        }
    }
    return 0;
}
```
