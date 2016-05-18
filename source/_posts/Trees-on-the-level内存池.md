---
title: Trees on the level内存池
date: 2016-05-17 20:46:35
tags:
- UVa
- 内存池
---
在上篇的解决方案中，通过静态数组配合空闲列表增加一个简单的内存池。

<!--more-->

```c
#include<cstdio>
#include<vector>
#include<queue>
#define LOCAL
using namespace std;
const int maxn=300;
char s[maxn];
bool failed;
struct Node{
    bool have_value;
    Node *left,*right;
    int v;
    Node():have_value(false),left(NULL),right(NULL){}
};
Node node[maxn];
queue<Node*>freenodes;//内存池维护空闲指针
Node* root;
void init(){
    for(int i=0;i<maxn;i++){
        freenodes.push(&node[i]);
    }
}
Node* newnode(){
    Node* u=freenodes.front();
    u->left=u->right=NULL;
    u->have_value=false;
    freenodes.pop();
    return u;
}
void addnode(int v,char* s){
    int n=strlen(s);
    Node* u=root;
    for(int i=0;i<n;i++){
        if(s[i]=='L'){
            if(!u->left) u->left=newnode();
            u=u->left;
        }
        else if(s[i]=='R'){
            if(!u->right) u->right=newnode();
            u=u->right;
        }
    }
    if(u->have_value) failed=true;
    u->v=v;
    u->have_value=true;
}
void delete_node(Node* u){
    freenodes.push(u);
}
void remove_tree(Node* u){
    if(!u) return;
    remove_tree(u->left);
    remove_tree(u->right);
    delete_node(u);
}
bool read_input(){
    failed=false;
    remove_tree(root);
    root=newnode();
    for(;;){
        if(scanf("%s",s)!=1) return false;
        if(!strcmp(s,"()")) break;
        int v;
        sscanf(&s[1],"%d",&v);
        addnode(v,strchr(s,',')+1);
    }
    return true;
}
bool bfs(vector<int>& ans){
    queue<Node*>q;
    ans.clear();
    q.push(root);
    while(!q.empty()){
        Node* u=q.front();q.pop();
        if(!u->have_value) return false;
        ans.push_back(u->v);
        if(u->left) q.push(u->left);
        if(u->right) q.push(u->right);
    }
    return true;
}

int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt", "r", stdin);
#endif
    vector<int>ans;
    init();
    while(read_input()){
        if(!bfs(ans)) failed=1;
        if(failed) printf("Not completed");
        else{
            for(int i=0;i<ans.size();i++){
                if(i!=0) printf(" ");
                printf("%d",ans[i]);
            }
        }
        printf("\n");
    }
    return 0;
}
```