---
title: Paintball
date: 2016-05-24 18:18:13
tags: 
- UVa
- DFS
---
**题意**：
> 有n个敌人，每个敌人有一个攻击范围，问你是否存在从西边到东边的路径，如果存在，输出入点和出点最靠北的坐标。
<!--more-->

```C
#include<cstdio>
#include <cstring>
#include <algorithm>
#include <cmath>
//#define LOCAL
using namespace std;
const int  maxn=1000+5;
double W=1000.0,left,right,x[maxn],y[maxn],r[maxn];
int vis[maxn],n;
void check_circle(int u){
    if(x[u]-r[u]<0) left=min(left,y[u]-sqrt(r[u]*r[u]-x[u]*x[u]));
    if(x[u]+r[u]>W) right=min(left,y[u]-sqrt(r[u]*r[u]-(W-x[u])*(W-x[u])));
}
bool intersect(int c1,int c2){
    return sqrt((x[c1]-x[c2])*(x[c1]-x[c2])+(y[c1]-y[c2])*(y[c1]-y[c2]))<r[c1]+r[c2];
}
bool bfs(int u){
    if (vis[u]) return false;
    vis[u]=1;
    if(y[u]-r[u]<=0) return true;
    for(int i=0;i<n;i++){
        if(intersect(u,i) && bfs(i)) return true;
    }
    check_circle(u);
    return false;
}
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt","r",stdin);
#endif
    while(scanf("%d",&n)==1){
        bool ok=true;
        left=right=W;
        memset(vis,0, sizeof(vis));
        for(int i=0;i<n;i++){
            scanf("%lf%lf%lf",&x[i],&y[i],&r[i]);
        }
        for(int i=0;i<n;i++){
            if(y[i]+r[i]>=W && bfs(i)){ok=false;break;}
        }
        if(ok) printf("0.00 %.2lf %.2lf %.2lf\n", left, W, right);
            else printf("IMPOSSIBLE\n");
    }
    return 0;
}
```