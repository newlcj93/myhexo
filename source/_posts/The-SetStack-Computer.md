---
title: The SetStack Computer
date: 2016-05-13 16:57:37
tags: UVa
---
# The SetStack Computer
> 有一个专门为了集合运算而设计的“集合栈”计算机。该机器有一个初始为空的栈，并且支持以下操作：
- PUSH：空集入栈
- DUP：把当前栈顶元素复制一份后再入栈
- UNION：出栈两个集合，然后把两者的并集入栈
- INTERSECT：出栈两个集合，然后把二者的交集入栈
- ADD：出栈两个集合，然后把先出栈的集合加入到后出栈的集合中，把结果入栈
输入：先输入测试次数，再输入操作次数，再输入具体操作
<!--more-->

---------
```c
#include<iostream>
#include<string>
#include<vector>
#include<map>
#include<set>
#include<stack>
#include<algorithm>
#include<iterator>
#define ALL(x) x.begin(),x.end()
#define INS(x) inserter(x,x.begin())
using namespace std;
typedef set<int> Set;
map<Set,int> IDcache;
vector<Set> Setcache;

int ID(Set x){
    if (IDcache.count(x)) return IDcache[x];
    Setcache.push_back(x);
    return IDcache[x]=Setcache.size()-1;
}

int main() {
    stack<int> s;
    int n;
    scanf("%d",&n);
    for(int i=0;i<n;i++){
        string op;
        cin>>op;
        if(op[0]=='P') s.push(ID(Set()));
        else if(op[0]=='D') s.push(s.top());
        else{
            Set x1=Setcache[s.top()];s.pop();
            Set x2=Setcache[s.top()];s.pop();
            Set x;
            if(op[0]=='U') set_union (ALL(x1), ALL(x2), INS(x));
            if(op[0]=='I') set_intersection (ALL(x1), ALL(x2), INS(x));
            if(op[0]=='A') x=x2;x.insert(ID(x1));
            s.push(ID(x));
        }
    cout<<Setcache[s.top()].size()<<endl;
    }
return 0;
}
```