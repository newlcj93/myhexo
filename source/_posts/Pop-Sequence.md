---
title: Pop Sequence
date: 2016-05-22 18:04:01
tags: PAT
---
**题目描述**:
> Given a stack which can keep M numbers at most.  Push N numbers in the order of 1, 2, 3, ..., N and pop randomly.  You are supposed to tell if a given sequence of numbers is a possible pop sequence of the stack.  For example, if M is 5 and N is 7, we can obtain 1, 2, 3, 4, 5, 6, 7 from the stack, but not 3, 2, 1, 7, 5, 6, 4.
<!--more-->

**输入描述**:
> Each input file contains one test case.  For each case, the first line contains 3 numbers (all no more than 1000): M (the maximum capacity of the stack), N (the length of push sequence), and K (the number of pop sequences to be checked).  Then K lines follow, each contains a pop sequence of N numbers.  All the numbers in a line are separated by a space.


**输出描述**:
> For each pop sequence, print in one line "YES" if it is indeed a possible pop sequence of the stack, or "NO" if not.

**输入例子**:
> 5 7 5
1 2 3 4 5 6 7
3 2 1 7 5 6 4
7 6 5 4 3 2 1
5 6 4 3 7 2 1
1 7 6 5 4 3 2

**输出例子**:
> YES
NO
NO
YES
NO

```c
#include<cstdio>
#include<stack>
#include <cstring>
//#define LOCAL
using namespace std;
const int maxn=1000+5;
int t[maxn];
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt","r",stdin);
#endif
    int N,M,K;
    scanf("%d%d%d",&M,&N,&K);
    for(int i=0;i<K;i++){
        int A=1,B=1,ok=1;
        stack<int>s;
        memset(t,0,sizeof(t));
        for(int j=1;j<=N;j++){
            scanf("%d",&t[j]);
        }
        while(B<=N){
            if(A==t[B]){A++;B++;}
            else if(!s.empty() && s.top()==t[B]){
                s.pop();
                B++;
            }
            else if (A<=N && s.size()<M-1) {s.push(A++);}
            else {ok=0;break;}
        }
        printf("%s\n",ok?"YES":"NO");
    }
    return 0;
}
```