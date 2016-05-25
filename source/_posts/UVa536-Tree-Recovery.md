---
title: UVa536-Tree Recovery
date: 2016-05-25 15:40:57
tags: 
- UVa
---
**题目**:
> 输入二叉树的前序遍历和中序遍历序列后，输出后序遍历序列。
<!--more-->

```C
#include<cstdio>
#include <cstring>
#define LOCAL
using namespace std;
const int maxn=100;
char lch[maxn],rch[maxn],in_order[maxn],pre_order[maxn];
char build(int L1,int R1,int L2,int R2){
    if(L1>R1) return 0;
    char root=pre_order[L1];
    int p=L2;
    while(in_order[p]!=root) p++;
    int cnt=p-L2;
    lch[root]=build(L1+1,L1+cnt,L2,p-1);
    rch[root]=build(L1+cnt+1,R1,p+1,R2);
    return root;
}
void post_order(char root){
    if(lch[root]) post_order(lch[root]);
    if(rch[root]) post_order(rch[root]);
    printf("%c",root);
}
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt","r",stdin);
#endif
    while(scanf("%s%s",pre_order,in_order)==2){
        build(0,strlen(pre_order),0,strlen(in_order));
        post_order(pre_order[0]);
        printf("\n");
    }
    return 0;
}
```