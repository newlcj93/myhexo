---
title: Database
date: 2016-05-15 18:29:04
tags: UVa
---
> 一个m(<10000)行n(<10)列的数据库问是否存在两个不同行r1, r2和两个不同列c1,c2。(r1, c2)和(r2, c2)相等。
<!--more-->

--------
```
#include<iostream>
#include<cstdio>
#include <string>
#include<map>
#include<vector>
using namespace std;
const int maxr=1000+50;
const int maxc=10+5;
typedef pair<int, int>p2;
map<string,int>id;
int cnt=0,db[maxr][maxc],m,n;
int ID(const string& s){
    if(!id.count(s)) id[s]=++cnt;
    return id[s];
}
void find(){
    map<p2,int>d;
    for(int c1=0;c1<m;c1++){
        for(int c2=c1+1;c2<m;c2++){
            for(int r=0;r<n;r++){
                p2 p=make_pair(db[r][c1], db[r][c2]);
                if(d.count(p)) {
                    printf("NO\n");
                    printf("%d %d\n",d[p]+1,r+1);
                    printf("%d %d\n",c1+1,c2+1);
                }
                d[p]=r;
            }
        }
    }
    printf("YES\n");
}
int main(){
    string s;
    while(scanf("%d%d",&n,&m)!=EOF,n,m){
        for(int i=0;i<n;i++){
            cin>>s;
            int lastpos=-1;
            for(int j=0;j<m;j++){
                int p=s.find(',',lastpos+1);
                if(p==string::npos) p=s.length();
                db[i][j]=ID(s.substr(lastpos+1,p-lastpos-1));
                lastpos=p;
            }
        }
        find();
    }
    return 0;
}
```