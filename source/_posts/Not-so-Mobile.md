---
title: Not so Mobile
date: 2016-05-18 21:05:34
tags: UVa
---
> 题意：这是一个类似于树的天平，这个天平的每一端都有可能由一个子天平构成，而每个天平都满足一个公式 WL * DL = WR * DR，其中WL，WR分别代表左边和右边物品的重量，DL，DR分别代表左边和右边物品里天平中心的距离。

> 输入分析：对于每个输入的四个数，如果WL或WR为0时，则代表接下来的输入代表子天平的数据，如果WL和WR同时为0，则输入数据先描述左子天平的状态，其次是右子天平。

> 要求：判断该数据是否可以构成一个平衡的天平。

<!--more-->

```c
#include<iostream>
#define LOCAL
using namespace std;
bool solve(int& W){
    int W1,D1,W2,D2;
    bool b1=true,b2=true;
    cin>>W1>>D1>>W2>>D2;
    if(!W1) b1=solve(W1);
    if(!W2) b2=solve(W2);
    W=W1+W2;
    return b1 && b2 && (W1*D1==W2*D2);
}
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt", "r", stdin);
#endif
    int T,W;
    cin>>T;
    while(T--){
        if(solve(W)) cout<<"YES\n";else cout<<"NO\n";
        if(T) cout<<endl;
    }
    return 0;
}
```
