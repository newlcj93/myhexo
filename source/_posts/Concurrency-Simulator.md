---
title: Concurrency Simulator
date: 2016-05-16 15:32:49
tags: UVa
---
> 模拟n个程序（按输入顺序编号为1～n）的并行执行。每个程序包含不超过25条语句，格式一共有5种：
- var = constant（赋值）
- print var（打印）
- lock
- unlock
- end
<!--more-->
> 变量用单个小写字母表示，初始为0，为所有程序公有（因此在一个程序里对某个变量赋值可能会影响另一个程序）。常数是小于100的非负整数。

> 每个时刻只能有一个程序处于运行态，其他程序均处于等待态。上述5种语句分别需要t1、t2、t3、t4、t5单位时间。运行态的程序每次最多运行Q个单位时间（称为配额）。当一个程序的配额用完之后，把当前语句（如果存在）执行完之后该程序会被插入一个等待队列中，然后处理器从队首取出一个程序继续执行。初始等待队列包含按输入顺序排列的各个程序，但由于lock/unlock语句的出现，这个顺序可能会改变。 

> lock的作用是申请对所有变量的独占访问。lock和unlock总是成对出现，并且不会嵌套。lock总是在unlock的前面。当一个程序成功执行完lock指令之后，其他程序一旦试图执行lock指令，就会马上被放到一个所谓的阻止队列的尾部（没有用完的配额就浪费了）。当unlock执行完毕后，阻止队列的第一个程序进入等待队列的首部。 

> 输入n, t1, t2, t3, t4, t5, Q以及n个程序，按照时间顺序输出所有print语句的程序编号和结果。

```c
#include<cstdio>
#include<queue>
#include<cstring>
#include<cstdlib>
#include<cctype>
#include<iostream>
#define LOCAL
using namespace std;

const int maxn = 1000;

deque<int> readyQ;
queue<int> blockQ;
int n, quantum, c[5], var[26], ip[maxn]; // ip[pid]是程序pid的当前行号。所有程序都存在prog数组，更类似真实的情况，代码也更短
bool locked=false;
char prog[maxn][10];
void run(int pid){
    int q=quantum;
    while(q){
        char *p=prog[ip[pid]];
        switch (p[2]) {
            case '=':
                var[p[0]-'a']=isdigit(p[5])?(p[4]-'0')*10+p[5]-'0':p[4]-'0';
                q-=c[0];
                break;
            case 'i':
                printf("%d: %d\n",pid+1,var[p[6]-'a']);
                q-=c[1];
                break;
            case 'c':
                if(locked){
                    blockQ.push(pid);
                    return;
                }
                locked=true;
                q-=c[2];
                break;
            case 'l':
                locked=false;
                if(!blockQ.empty()){
                    int pid2=blockQ.front();
                    readyQ.push_front(pid2);
                    blockQ.pop();
                }
                q-=c[3];
                break;
            case 'd':
                return;
        }
        ip[pid]++;
    }
    readyQ.push_back(pid);
}
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt", "r", stdin);
#endif
    int T;
    scanf("%d",&T);
    while(T--){
        scanf("%d",&n);
        int pid,line=0;
        for(int i=0;i<5;i++){
            scanf("%d",&c[i++]);
        }
        scanf("%d",&quantum);

        for(int i=0;i<n;i++){
            fgets(prog[line++], maxn, stdin);
            ip[i]=line-1;
            while(prog[line-1][2]!='d'){
               fgets(prog[line++], maxn, stdin);
            }
            pid=i;
            readyQ.push_back(pid);
        }
        while(!readyQ.empty()) {
            pid=readyQ.front();
            readyQ.pop_front();
            run(pid);
        }
        printf("\n");
    }
    return 0;
}
```
