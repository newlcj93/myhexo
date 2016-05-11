---
title: 1001. A+B Format (20)
date: 2016-05-11 12:03:45
tags: PAT甲级
---
# 1001. A+B Format (20)

> Calculate a + b and output the sum in standard format -- that is, the digits must be separated into groups of three by commas (unless there are less than four digits).
<!--more-->
> **Input**

> Each input file contains one test case. Each case contains a pair of integers a and b where -1000000 <= a, b <= 1000000. The numbers are separated by a space.

> **Output**

> For each test case, you should output the sum of a and b in one line. The sum must be written in the standard format.
> **Sample Input**
-1000000 9
**Sample Output**
-999,991

------
这道题难度不大，答案考虑了输入的数字也采用“,”隔开的情况，比原题稍复杂些

```c
#include <stdio.h>
int str2n(char *s){
    int i=0,ans=0;
    int sign=1;
    if(s[0]=='-') {
        sign = -1;
        i++;
    }
    while(s[i]){
        if (s[i]!=','){
            ans=ans*10+s[i]-'0';
        }
        i++;
    }
    return sign*ans;
}
int main(){
    char a[20],b[20],s[20];
    int c,d,sum,tot=0;
    scanf("%s%s",a,b);
    c=str2n(a);
    d=str2n(b);
    sum=c+d;
    if (sum<0){
        printf("-");
        sum=-sum;
    }
    else if(!sum) printf("0");
    while (sum) {
        s[tot++]=sum%10;
        sum/=10;
    }
    for(int i=tot-1;i>=0;i--){
        printf("%d",s[i]);
        if (i%3==0 && i) printf(",");
    }
    printf("\n");
    return 0;
}
```
