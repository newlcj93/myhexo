---
title: Trees on the level
date: 2016-05-17 20:18:49
tags: 
- UVa 
- BFS
---
> 输入二叉树的每一个节点的信息，建树完毕后，按照层次顺序遍历这棵树，然后将每一个节点的权值给输出来！
**注意**：如果从根到某个叶节点的路径上有的节点没有在输入中给出或者给出超过一次，应该输出“not complete”.节点数不超过256个！

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
Node* root;
Node* newnode(){
    return new Node();
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
void remove_tree(Node* u){
    if(!u) return;
    remove_tree(u->left);
    remove_tree(u->right);
    delete u;
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