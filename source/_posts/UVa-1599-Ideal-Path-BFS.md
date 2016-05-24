---
title: UVa 1599 - Ideal Path(BFS)
date: 2016-05-24 11:36:07
tags: 
- UVa
- BFS
---
**题目**:
> 给出n个点，m条边的无向图，每条边有一种颜色，求从结点1到结点n颜色字典序最小的最短路径。
<!--more-->

```C
#include<cstdio>
#include <cstring>
#include <vector>
#include<queue>
#define LOCAL
using namespace std;
const int maxn=100000+5;
const int INF=100000000;
struct Edge{
    int u,v,c;
    Edge(int u=0,int v=0,int c=0):u(u),v(v),c(c){}
};
vector<Edge>edges;
vector<int>G[maxn];
void AddEdge(int u,int v,int c){
    edges.push_back(Edge(u,v,c));
    int idx=edges.size()-1;
    G[u].push_back(idx);
}
int n,vis[maxn];
int d[maxn];
void rev_bfs(){
    memset(vis,0,sizeof(vis));
    d[n-1]=0;
    vis[n-1]=true;
    queue<int>q;
    q.push(n-1);
    while(!q.empty()){
        int v=q.front();q.pop();
        for(int i=0;i<G[v].size();i++){
            int e=G[v][i];
            int u=edges[e].v;
            if(!vis[u]){
                vis[u]=true;
                d[u]=d[v]+1;
                q.push(u);
            }
        }
    }
}
vector<int> ans;
void bfs(){
    memset(vis, 0, sizeof(vis));
    vis[0]=true;
    ans.clear();
    vector<int>next;
    next.push_back(0);
    for(int i=0;i<d[0];i++){
        int min_color=INF;
        for(int j=0;j<next.size();j++){
            int u=next[j];
            for (int k=0;k<G[u].size();k++){
                int e=G[u][k];
                int v=edges[e].v;
                if(d[u]==d[v]+1)
                    min_color=min(min_color,edges[e].c);
            }
        }
        ans.push_back(min_color);
        vector<int>next2;
        for(int j=0;j<next.size();j++){
            int u=next[j];
            for(int k=0;k<G[u].size();k++){
                int e=G[u][k];
                int v=edges[e].v;
                if (!vis[v] && d[u]==d[v]+1 && edges[e].c==min_color){
                    vis[v]=true;
                    next2.push_back(v);
                }
            }
        }
        next=next2;
    }
    printf("%d\n",ans.size());
    printf("%d",ans[0]);
    for(int i=1;i<ans.size();i++) printf(" %d",ans[i]);
    printf("\n");
}
int main(){
#ifdef LOCAL
    freopen("/Users/jy/input.txt","r",stdin);
#endif
    int u,v,c,m;
    while(scanf("%d%d",&n,&m)==2){
        for(int i=0;i<n;i++) G[i].clear();
        edges.clear();
        while(m--){
            scanf("%d%d%d",&u,&v,&c);
            AddEdge(u-1, v-1, c);
            AddEdge(v-1, u-1, c);
        }
        rev_bfs();
        bfs();
    }

    return 0;
}
```