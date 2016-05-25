---
title: UVa1600-Patrol Robot
date: 2016-05-25 10:01:56
tags: 
- UVa
- BFS
---
**题目**：
> 机器人要从一个m*n（1≤m，n≤20）网格的左上角(1,1)走到右下角(m,n)。网格中的一些格子是空地（用0表示），其他格子是障碍（用1表示）。机器人每次可以往4个方向走一格，但最多连续地穿越k（0≤k≤20）个障碍，求最短路长度。起点和终点保证是空地。
<!--more-->

```C
#include<cstdio>
#include <queue>
#include <cstring>
#define LOCAL
using namespace std;
int n,m,K;
const int maxn=20+5;
int G[maxn][maxn],d[maxn][maxn][maxn],vis[maxn][maxn][maxn];
const int dr[]={1,0,-1,0};
const int dc[]={0,-1,0,1};
struct Node{
    int r,c,k;
    Node(int r=0,int c=0,int k=0):r(r),c(c),k(k){}
};
bool legal(int r,int c,int k){
    return r>0 && r<=m && c>0 && c<=n && k>=0;
}
Node walk(const Node& u,int i){
    int r=u.r+dr[i];
    int c=u.c+dc[i];
    int k=G[r][c]?u.k-1:u.k;
    return Node(r,c,k);
}
void bfs(){
    queue<Node>q;
    memset(vis,0,sizeof(vis));
    memset(d,0,sizeof(d));
    Node u(1,1,K);
    q.push(u);
    vis[1][1][K]=1;
    d[1][1][K]=0;
    while(!q.empty()){
        Node u=q.front();q.pop();
        if(u.r==m && u.c==n && u.k>=0) {printf("%d\n",d[u.r][u.c][u.k]);return;}
        for(int i=0;i<4;i++){
            Node v=walk(u,i);
            if(legal(v.r, v.c, v.k) && !vis[v.r][v.c][v.k]){
                d[v.r][v.c][v.k]=d[u.r][u.c][u.k]+1;
                vis[v.r][v.c][v.k]=1;
                q.push(v);
            }
        }
    }
    printf("-1\n");
}
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt","r",stdin);
#endif
    int T;
    scanf("%d",&T);
    while(T--){
        scanf("%d%d%d",&m,&n,&K);
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                scanf("%d",&G[i][j]);
            }
        }
        bfs();
    }
    return 0;
}
```