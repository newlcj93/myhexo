---
title: Quadtrees
date: 2016-05-19 00:25:13
tags:
- UVa
- Trees
---
> 有一个32×32像素的黑白图片，用四分树来表示。树的四个节点从左到右分别对应右上、左上、左下、右下的四个小正方区域。然后用递归的形式给出一个字符串代表一个图像，f(full)代表该节点是黑色的，e(empty)代表该节点是白色的，p表示灰色节点，即它还有子节点。求两幅图黑色像素合并以后的黑色像素的个数。

<!--more-->

```c
#include<cstdio>
#include<algorithm>
#define LOCAL
using namespace std;
const int maxn=1024+100;
const int len=32;
int buf[len][len];
char s[maxn];
int cnt;
void draw(char* s,int& p,int r,int c,int w){
    char ch=s[p++];
    if(ch=='p'){
        draw(s,p,r,c+w/2,w/2);
        draw(s,p,r,c,w/2);
        draw(s,p,r+w/2,c+w/2,w/2);
        draw(s,p,r+w/2,c,w/2);
    }
    else if(ch=='f'){
        for(int i=r;i<r+w;i++){
            for (int j=c;j<c+w;j++)
                if (buf[i][j]==0){
                    buf[i][j]=1;
                    cnt++;
                }
        }
    }
}
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt", "r", stdin);
#endif
    int T;
    scanf("%d",&T);
    while(T--){
        memset(buf, 0, sizeof(buf));
        cnt=0;
        for(int i=0;i<2;i++){
            scanf("%s",s);
            int p=0;
            draw(s, p, 0, 0, len);
        }
        printf("There are %d black pixels.\n",cnt);
    }
    return 0;
}

```