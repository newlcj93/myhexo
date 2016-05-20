---
title: Oil Deposits
date: 2016-05-20 18:23:58
tags:
- UVa
- 图
---
> 输入m行n列的字符矩阵，统计字符“@”组成多少个八连块。如果两个字符“@”所在的格子相邻（横，竖或者对角线方向），就说它们属于同一个八连块。
<!--more-->

```c
#include <cstdio>
#include <cstring>
#define LOCAL
const int maxn=100+5;
char pic[maxn][maxn];
int m,n,idx[maxn][maxn];
void dfs(int r,int c,int id){
    if( r<0 || r>=m || c<0 || c>=n) return;
    if(idx[r][c]==id || pic[r][c]!='@') return;
    idx[r][c]=id;
    for(int dr=-1;dr<=1;dr++){
        for(int dc=-1;dc<=1;dc++){
            if(dr!=0 || dc!=0){
                dfs(r+dr,c+dc,id);
            }
        }
    }
}
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt","r",stdin);
#endif
    while(scanf("%d%d",&m,&n)==2 && m && n){
        for(int i=0;i<m;i++) scanf("%s",pic[i]);
        memset(idx, 0, sizeof(idx));
        int cnt=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(idx[i][j]==0 && pic[i][j]=='@'){
                    dfs(i,j,++cnt);
                }
            }
        }
        printf("%d\n",cnt);
    }
    return 0;
}
```