---
title: Rails
date: 2016-05-15 22:42:33
tags: UVa
---
> 给定一个不变的入栈的顺序1～n，然后再给出一个序列问是否是一个满足条件的出栈顺序。
<!--more-->

------------

```c
#include<stack>
#include<cstdio>
using namespace std;
const int maxn=1000;
int target[maxn];
int main(){
    int n;
    while(scanf("%d",&n)!=EOF){
        int A=1,B=1,ok=1;
        stack<int>s;
        for(int i=1;i<n+1;i++){
            scanf("%d",&target[i]);
        }
        while(B <= n){
            if(A==target[B]){
                A++;B++;
            }
            else if(!s.empty() && s.top()==target[B]){
                s.pop();
                B++;
            }
            else if(A<target[B]){
                s.push(A);
                A++;
            }
            else{
                ok=0;
                break;
            }
        
        }
        printf("%s\n",ok?"YES":"NO");
    }
    return 0;
}
```