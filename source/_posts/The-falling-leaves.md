---
title: The falling leaves
date: 2016-05-18 23:16:47
tags: 
- UVa
- Tree
---
> 给一颗二叉树，每个节点都有一个水平位置：左子结点在它的左边一个单位，右子节点在右边一个单位。从左向右输出每个水平位置的所有节点的权值之和。按照递归（先序）方式输入，用-1 表示空树。
**样例输入**：
5 7 -1 6 -1 -1 3 -1 -1
8 2 9 -1 -1 6 5 -1 -1 12 -1 -1 3 7 -1 -1 -1 -1
**样例输出**：
Case 1:
7 11 3
Case 2:
9 7 21 15

<!--more-->

```c
#include<cstdio>
#include<algorithm>
#define LOCAL
using namespace std;
const int maxn=100;
int sum[maxn],v;
void build(int p){
    scanf("%d",&v);
    if(v==-1) return;
    sum[p]+=v;
    build(p-1);
    build(p+1);
}
bool init(){
    memset(sum,0,sizeof(sum));
    scanf("%d",&v);
    if(v==-1) return false;
    int p=maxn/2;
    sum[p]+=v;
    build(p-1);
    build(p+1);
    return true;
}
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt", "r", stdin);
#endif
    int kase=0;
    while(init()){
        int first=1;
        printf("Case %d:\n",++kase);
        for(int i=0;i<maxn;i++){
            if(sum[i]==0) continue;
            else{
                if(!first) printf(" ");
                first=0;
                printf("%d",sum[i]);
            }
            
        }
        printf("\n");
    }
    return 0;
}
```