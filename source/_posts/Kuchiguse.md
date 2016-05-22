---
title: Kuchiguse
date: 2016-05-22 20:00:21
tags: PAT
---
**题目描述**
> The Japanese language is notorious for its sentence ending particles. Personal preference of such particles can be considered as a reflection of the speaker's personality. Such a preference is called "Kuchiguse" and is often exaggerated artistically in Anime and Manga. For example, the artificial sentence ending particle "nyan~" is often used as a stereotype for characters with a cat-like personality:
Itai nyan~ (It hurts, nyan~)
Ninjin wa iyada nyan~ (I hate carrots, nyan~)
Now given a few lines spoken by the same character, can you find her Kuchiguse?
<!--more-->
**输入描述**:
> Each input file contains one test case.  For each case, the first line is an integer N (2<=N<=100). Following are N file lines of 0~256 (inclusive) characters in length, each representing a character's spoken line. The spoken lines are case sensitive.


**输出描述**:
> For each test case, print in one line the kuchiguse of the character, i.e., the longest common suffix of all N lines. If there is no such suffix, write "nai".

**输入例子**:
> 3
Itai nyan~
Ninjin wa iyadanyan~
uhhh nyan~

**输出例子**:
> nyan~

c语言版
```c
#include<cstdio>
#include <cstring>
char str[100][270];
int len[100];
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt","r",stdin);
#endif
    int n;
    scanf("%d",&n);
    getchar();
    for(int i=0;i<n;i++){
        gets(str[i]);
        len[i]=strlen(str[i]);
    }
    bool flag=true;
    int min=len[0]-1;
    for(int i=1;i<n && flag;i++){
        for(int j=len[0]-1,k=len[i]-1;j>0 && k>0;j--,k--){
            if(str[0][j]!=str[i][k]){
                flag=false;
                break;
            }
            else{
                min=j;
            }
        }
    }
    if(min!=len[0]-1){
        for(int i=min;i<len[0];i++){
            printf("%c",str[0][i]);
        }
        printf("\n");
    }
    else printf("nai\n");
    return 0;
}
```
C++版
```c++
#include<iostream>
#include<algorithm>
#include <string>
#include <vector>
#define LOCAL
using namespace std;
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt","r",stdin);
#endif
    string s,rs;
    vector<string>v;
    int n;
    cin>>n;
    getline(cin,s);
    while(n--){
        getline(cin,s);
        reverse(s.begin(), s.end());
        v.push_back(s);
    }
    int loc=-1;
    bool flag=true;
    while(flag && ++loc<v[0].size()){
        char ch=v[0][loc];
        for(int i=1;i<v.size();i++){
            if(ch!=v[i][loc]){
                flag=false;
                break;
            }
        }
        if(flag) rs+= v[0][loc];
    }
    if (rs.empty()) cout<<"nai"<<endl;
    else{
        reverse(rs.begin(), rs.end());
        cout<<rs<<endl;
    }
    return 0;
}
```