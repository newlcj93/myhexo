---
title: UVa 400 Unix ls
date: 2016-05-14 22:09:02
tags: UVa
---
# UVa 400 Unix ls
> 模拟Unix下ls命令。给出一些列文件名，按字典序排序之后，以列优先的方式输出。除了最后一列之外，其余各列所占的字符数为最长文件名长度加2，最后一列所占数目为最长文件名长度。每行字符数不能超过60，要求最终的行数最少。
<!--more-->
```c
#include<algorithm>
#include <string>
#include<iostream>
using namespace std;
const int maxn=100;
const int maxcol=60;
string fn[maxn];
void print(const string& s,int len,char extra){
    cout<<s;
    for(int i=0;i<len-s.length();i++){
        cout<<extra;
    }
}
int main(){
    int n,m=0;
    cin>>n;
    for(int i=0;i<n;i++){
        cin>>fn[i];
        m=max(m,(int)fn[i].length());
    }
    int cols=(maxcol-m)/(m+2)+1;
    int rows=(n-1)/cols+1;
    print("",60,'-');
    cout<<endl;
    sort(fn,fn+n);
    for(int i=0;i<rows;i++){
        for (int j=0;j<cols;j++){
            int index=i*cols+j;
            if(index<n) print(fn[index],j==cols-1?m:m+2,' ');
        }
        cout<<endl;
    }
    return 0;
}
```