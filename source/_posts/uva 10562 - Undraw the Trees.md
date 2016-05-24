---
title: Undraw the Trees
date: 2016-05-23 17:32:03
tags: UVa
---
**题目**:
> 将多叉树转化为括号表示法
<!--more-->

```c
#include<cstdio>
#include <cstring>
#include <cctype>
#define LOCAL
char str[100][270];
int len[100];
const int maxn=200+10;
int n;
char buf[maxn][maxn];
void dfs(int r,int c){
    printf("%c(",buf[r][c]);
    if(buf[r+1][c]=='|'){
        int i=c;
        while(buf[r+2][i-1]=='-') i--;
        while(buf[r+2][i]=='-'){
           if(!isspace(buf[r+3][i])) dfs(r+3, i);
           i++;
        }
    }
    printf(")");
}
void solve(){
    printf("(");
    n=0;
    for(;;){
        fgets(buf[n],maxn,stdin);
        if(buf[n][0]=='#') break;
        n++;
    }
    if(n){
        int len=strlen(buf[0]);
        for(int i=0;i<len;i++){
            if(buf[0][i]!=' ') {dfs(0,i);break;}
        }
    }
    printf(")\n");
}
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt","r",stdin);
#endif
    int T;
    fgets(buf[0],maxn,stdin);
    sscanf(buf[0],"%d",&T);
    while(T--){
        solve();
    }
    return 0;
}
```