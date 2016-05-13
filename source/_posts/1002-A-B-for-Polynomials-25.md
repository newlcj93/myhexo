---
title: 1002.A+B for Polynomials (25)
date: 2016-05-11 14:58:49
tags: PAT甲级
---
# 1002. A+B for Polynomials (25)
>This time, you are supposed to find A+B where A and B are two polynomials.
<!--more-->
>**Input**

>Each input file contains one test case. Each case occupies 2 lines, and each line contains the information of a polynomial: K N1 aN1 N2 aN2 ... NK aNK, where K is the number of nonzero terms in the polynomial, Ni and aNi (i=1, 2, ..., K) are the exponents and coefficients, respectively. It is given that 1 <= K <= 10，0 <= NK < ... < N2 < N1 <=1000.

>**Output**

>For each test case you should output the sum of A and B in one line, with the same format as the input. Notice that there must be NO extra space at the end of each line. Please be accurate to 1 decimal place.

>**Sample Input**
2 1 2.4 0 3.2
2 2 1.5 1 0.5
**Sample Output**
3 2 1.5 1 2.9 0 3.2

----------
```c
#include <stdio.h>
double s[1010];
int main(){
    double c;
    int n,k,cnt=0,max=0;
    memset(s,0,sizeof(s));
    for (int i=0;i<2;i++){
        scanf("%d",&n);
        while(n--){
            scanf("%d%lf",&k,&c);
            s[k]+=c;
            if (k>max) max=k;
        }
    }
    for(int i=0;i<max+1;i++){
        if(s[i]) cnt++;
    }
    printf("%d",cnt);
    for(int i=max;i>=0;i--){
        if (s[i]){
            printf(" %d %.1lf",i,s[i]);
        }
    }
    printf("\n");

    return 0;
}
```