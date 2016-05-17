---
title: Boxes in a line
date: 2016-05-17 14:20:11
tags: UVa
---
> 你有一行盒子，从左到右依次编号为1, 2, 3,…, n。你可以执行四种指令：
- 1 X Y表示把盒子X移动到盒子Y左边（如果X已经在Y的左边则忽略此指令）。
- 2 X Y表示把盒子X移动到盒子Y右边（如果X已经在Y的右边则忽略此指令）。
- 3 X Y表示交换盒子X和Y的位置。
- 4 表示反转整条链。
盒子个数n和指令条数m（1<=n,m<=100,000)
<!--more-->

```c
#include<cstdio>
#include<algorithm>
#define LOCAL
using namespace std;
const int maxn=100000+50;
int left[maxn],right[maxn];
void link(int L,int R){
    left[R]=L;
    right[L]=R;
}
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt","r",stdin);
#endif
    int n,m,op,x,y,kase=1;
    while(scanf("%d %d",&n,&m)==2){
        int inv=0,ans=0;
        for(int i=1;i<=n;i++){
            right[i]=(i+1)%(n+1);
            left[i]=i-1;
        }
        right[0]=1;left[0]=n;
        while(m--){
            scanf("%d",&op);
            if(op==4) inv=!inv;
            else{
                scanf("%d %d",&x,&y);
                if(op==3 && right[y]==x) swap(x,y);
                if(op!=3 && inv) op=3-op;
                if(op==1 && left[y]==x) continue;
                if(op==2 && right[y]==x) continue;
                
                int lx=left[x],rx=right[x],ry=right[y],ly=left[y];
                if(op==1){
                    link(lx,rx);
                    link(ly,x);
                    link(x,y);
                }
                if(op==2){
                    link(lx,rx);
                    link(y,x);
                    link(x,ry);
                }
                if(op==3){
                    if(right[x]==y){
                        link(lx,y);
                        link(y,x);
                        link(x,ry);
                    }
                    else{
                        link(lx,y);
                        link(y,rx);
                        link(ly,x);
                        link(x,ry);
                    }
                }

            }
        }
        int b=0;
        for(int i=1;i<=n;i++){
            b=right[b];
            if(i%2==1) ans+=b;
        }
        if(inv && n%2==0) ans=(long long)n*(1+n)/2-ans;
        printf("Case %d: %d\n",kase++,ans);
    }
    return 0;
}
```