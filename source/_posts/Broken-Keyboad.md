---
title: Broken Keyboad
date: 2016-05-16 22:45:47
tags: UVa
---
> 在输入文章的时候，键盘上的Home键和End键出了问题，会不定时的按下。给你一段按键的文本，其中’[‘表示Home键，’]’表示End键，输出这段悲剧的文本。
<!--more-->

**思路**：使用链表来模拟，遇到Home键，就将后边的文本插入到这段文本的最前边，遇到End键，就插入到这段文本的最后边。

-----------

```c
#include<cstdio>
#include<cstring>
using namespace std;
const int maxn=100000+50;
int next[maxn],cur,last;
char s[maxn];
int main(){
    while(scanf("%s",s+1)==1){
        memset(next,0, sizeof(next));
        cur=last=0;
        next[0]=0;
        int len=strlen(s+1);
        for(int i=1;i<=len;i++){
            if (s[i]=='[') cur=0;
            else if (s[i]==']') cur=last;
            else{
                next[i]=next[cur];
                next[cur]=i;
                if(last==cur) last=i;
                cur=i;
            }
        }
        for(int i=next[0];i!=0;i=next[i]){
            printf("%c",s[i]);
        }
        printf("\n");;

    }
    return 0;
}
```
