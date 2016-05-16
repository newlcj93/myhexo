---
title: Matrix Chain Multiplication
date: 2016-05-16 16:32:43
tags: UVa
---
> 输入n个矩阵的维度和一些矩阵链乘表达式，输出乘法的次数。如果乘法无法进行，输出error。
<!--more-->

```c
#include<stack>
#include<cstdio>
using namespace std;
struct Matrix{
    int a,b;
    Matrix(int a=0,int b=0):a(a),b(b) {}
}m[26];
int main(){
    stack<Matrix>s;
    char name[10],e[10];
    int n;
    bool err=false;
    scanf("%d",&n);
    for(int i=0;i<n;i++){
        scanf("%s",name);
        scanf("%d %d",&m[name[0]-'A'].a,&m[name[0]-'A'].b);
    }
    while(scanf("%s",e)!=EOF){
        int len=strlen(e),ans=0;
        for(int i=0;i<len;i++){
            if(isalpha(e[i])) s.push(m[e[i]-'A']);
            else if(e[i]==')'){
                Matrix m2=s.top();s.pop();
                Matrix m1=s.top();s.pop();
                if(m1.b!=m2.a){
                    err=true;
                    break;
                }
                s.push(Matrix(m1.a,m2.b));
                ans+=m1.a*m1.b*m2.b;
            }
            
        }
        if(err) printf("error\n");
        else printf("%d\n",ans);
    }
}
```