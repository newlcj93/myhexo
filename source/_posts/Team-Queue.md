---
title: Team Queue
date: 2016-05-13 20:03:38
tags: 
- UVa
- STL
- Queue
---
# UVA540 Team Queue
> 题目：有t个团队的人在排队。每次来了一个新人之后，如果他有队友在排队，那么这个新人会插队到队友的身后。要求支持三种指令：
- ENQUEUE x;
- DEQUEUE（队首出队）; 
- STOP。
模拟这个过程，输出出队顺序。
<!--more-->
思路：设置两个队列，q为总的团队队列，q2为每个团队的队列，每个队列在一个大队列排队。

———
```c
#define LOCAL
#include<cstdio>
#include<queue>
#include<map>
using namespace std;
const int maxn=1000+10;
int main() {
#ifdef LOCAL
    freopen("/Users/jy/input.txt", "r", stdin);
   // freopen("~/output.txt", "w", stdout);
#endif
    int t,kase=0;
    char op[10];
    while(scanf("%d",&t)==1 && t){  //t为队伍数
        printf("Scenerio #%d\n",++kase);
        map<int,int>team;
        int n,x;  //n为每队人数,x为成员编号
        for(int i=0;i<t;i++){
            scanf("%d",&n);
            while(n--){
                scanf("%d",&x);
                team[x]=i;
            }
        }
        queue<int>q,q2[maxn];
        for(;;){
            
            scanf("%s",op);
            if (op[0]=='S') break;
            else if(op[0]=='E'){
                scanf("%d",&x);
                t=team[x];
                if(q2[t].empty()) q.push(t);
                q2[t].push(x);
            }
            else if(op[0]=='D'){
                t=q.front();
                printf("%d\n",q2[t].front());
                q2[t].pop();
                if(q2[t].empty()) q.pop();
            }
        }
        printf("\n");
    }
    return 0;
}

```
