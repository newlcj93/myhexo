---
title: Tree
date: 2016-05-18 20:05:37
tags:
- UVa
- Tree
---
> 找出一棵二叉树中最小路径上终端节点（树叶，leaf node）的值。所谓路径乃指从根节点（root）旅行到任一终端节点。路径的值为所经过的节点的值的和（包含根节点及终端节点）。而最小路径就是指路径值最小的那条。
<!--more-->
**Input**
每组测试数据2列，用来描述一棵二叉树。第一列的序列为以中序旅行（inorder traversal）所经过的节点的值。第二列的序列为以后序旅行（postorder traversal）所经过的节点的值。
各列中所有的值都是整数（大于0，小于10000），且值都不同。你可以假设二叉树的节点数最多10000，最少1。输入列的长度最长不会超过1000000。
**Output**
对每组测试数据，请输出二叉树中最小路径上终端节点的值。如果有不止一条最小路径，请输出终端节点值最小的值。
**Sample Input**
3 2 1 4 5 7 6
3 1 2 5 6 7 4
7 8 11 3 5 16 12 18
8 3 11 7 16 18 12 5
255
255
**Sample Output**
1
3
255

```c
#include<iostream>
#include<string>
#include<sstream>
#define LOCAL
using namespace std;
const int maxv=10000+10;
int in_order[maxv],post_order[maxv],lch[maxv],rch[maxv];
int n;
bool read_list(int* a){
    string line;
    if(getline(cin,line)){
        stringstream ss(line);
        n=0;int x;
        while(ss>>x) a[n++]=x;
    }
    else return false;
    return n>0;
}
int build(int L1,int R1,int L2,int R2){
    if(L1>R1) return 0;
    int p=L1;
    int root=post_order[R2];
    while(in_order[p]!=root) p++;
    int cnt=p-L1;
    lch[root]=build(L1,p-1, L2, L2+cnt-1);
    rch[root]=build(p+1,R1,L2+cnt,R2-1);
    return root;
}
int best,best_sum=10000;
void dfs(int u,int sum){
    sum +=u;
    if(!lch[u] && !rch[u]){
        if(sum<best_sum || (sum==best_sum && u<best)){
            best_sum=sum;
            best=u;
        }
    }
    if(lch[u]) dfs(lch[u],sum);
    if(rch[u]) dfs(rch[u],sum);
}
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt", "r", stdin);
#endif
    while(read_list(in_order)){
        read_list(post_order);
        build(0, n-1, 0, n-1);
        int u=post_order[n-1];
        dfs(u,0);
        cout<<best<<endl;
    }
    return 0;
}
```