---
title: UVa506-System Dependencies
date: 2016-05-24 14:46:25
tags: UVa
---
**题目**:
> 模拟系统安装组件和卸载组件的过程，安装一个组件的时候要先安装该组件的依赖，如果某些依赖已经安装，则无需重复安装。卸载某个组件时，先卸载该组件，然后卸载该组件的依赖，如果某些依赖还被其他已安装的组件使用，则不能卸载。组件的安装分为用户安装和系统安装，卸载也分用户卸载和系统卸载。
<!--more-->

```C
#include<iostream>
#include <string>
#include <vector>
#include<sstream>
#include <map>
#define LOCAL
using namespace std;
const int  maxn=10000;
string name[maxn];
vector<int>depend[maxn],depend2[maxn];
vector<int>installed;
map<string,int>name2id;
int status[maxn],cnt=0;
int ID(const string& item){
    if(!name2id.count(item)){
        name2id[item]=++cnt;
        name[cnt]=item;
    }
    return name2id[item];
}
void list(){
    int len=installed.size();
    for(int i=0;i<len;i++){
        cout<<"   "<<name[installed[i]]<<endl;
    }
}
bool needed(int item){
    int len=depend2[item].size();
    for(int i=0;i<len;i++){
        if(status[depend2[item][i]]) return true;
    }
    return false;
}
void remove(int item,bool toplevel){
    if((toplevel || status[item]==2) && !needed(item)){
        status[item]=0;
        cout << "   Removing " << name[item] << "\n";
        installed.erase(remove(installed.begin(),installed.end(),item),installed.end());
        int len=depend[item].size();
        for(int i=0;i<len;i++){
            remove(depend[item][i],false);
        }
    }
}
void install(int item,bool toplevel){
    if(!status[item]){
        status[item]=toplevel?1:2;
        cout << "   Installing " << name[item] << "\n";
        installed.push_back(item);
        int len=depend[item].size();
        for(int i=0;i<len;i++){
            install(depend[item][i],false);
        }
    }
}
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt","r",stdin);
#endif
    string s,cmd;
    while(getline(cin,s)){
        cout << s << "\n";
        stringstream ss(s);
        ss>>cmd;
        if(cmd[0]=='E') break;
        else if(cmd[0]=='L') list();
        else {
            string item1;
            ss>>item1;
            int id1=ID(item1);
            if(cmd[0]=='D'){
                string item1,item2;
                while(ss>>item2){
                    int id2=ID(item2);
                    depend[id1].push_back(id2);
                    depend2[id2].push_back(id1);
                }
            }
            else if(cmd[0]=='I'){
                if(status[id1]) cout << "   " << item1 << " is already installed.\n";
                else install(id1, 1);
            }
            else if(cmd[0]=='R'){
                if(!status[id1]) cout << "   " << item1 << " is not installed.\n";
                else if(needed(id1)) cout << "   " << item1 << " is still needed.\n";
                else remove(id1,1);
            }
        }
    }
    return 0;
}
```