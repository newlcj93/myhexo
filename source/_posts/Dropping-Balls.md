---
title: Dropping Balls
date: 2016-05-17 15:03:27
tags: UVa
---
> 在结点1处放一个小球，它会往下落。每个内结点上都会有一个开关，初始全部关闭，当每次有小球落到一个开关上时，状态都会改变。当小球到达一个结点是，如果结点上的开关关闭，则往左走，否则往右走，直到走到叶子的结点

<!--more-->

```
#include<cstdio>
using namespace std;
int main(){
    int D,I;
    while(scanf("%d%d",&D,&I)==2){
        int k=1;
        for (int i=0;i<D-1;i++){
            if (I%2) {
                I=(I+1)/2;
                k*=2;
            }
            else {
                I/=2;
                k=2*k+1;
            }
        }
        printf("%d\n",k);
    }
    return 0;
}
```
