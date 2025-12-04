# code 1 — Warshall’s Algorithm (Transitive Closure)

#include <stdio.h>

int main() {
    int n, i, j, k;
    int adj[20][20], reach[20][20];

    printf("Enter number of vertices: ");
    scanf("%d", &n);

    printf("Enter adjacency matrix (0/1):\n");
    for (i = 0; i < n; i++) {
        for (j = 0; j < n; j++) {
            scanf("%d", &adj[i][j]);
            reach[i][j] = adj[i][j];
        }
    }

    for (k = 0; k < n; k++) {
        for (i = 0; i < n; i++) {
            for (j = 0; j < n; j++) {
                reach[i][j] = reach[i][j] || (reach[i][k] && reach[k][j]);
            }
        }
    }

    printf("Transitive closure:\n");
    for (i = 0; i < n; i++) {
        for (j = 0; j < n; j++) printf("%d ", reach[i][j]);
        printf("\n");
    }
    return 0;
}


# code 2 — DFS Reachable Nodes

#include <stdio.h>

int n;
int adj[20][20];
int visited[20];

void dfs(int v) {
    int i;
    visited[v] = 1;
    printf("%d ", v);

    for (i = 0; i < n; i++)
        if (adj[v][i] && !visited[i])
            dfs(i);
}

int main() {
    int i, j, start;

    printf("Enter vertices: ");
    scanf("%d", &n);

    printf("Enter adjacency matrix:\n");
    for (i = 0; i < n; i++)
        for (j = 0; j < n; j++)
            scanf("%d", &adj[i][j]);

    for (i = 0; i < n; i++) visited[i] = 0;

    printf("Enter start vertex: ");
    scanf("%d", &start);

    printf("DFS reachable: ");
    dfs(start);
    return 0;
}


# code 3 — BFS Reachable Nodes

#include <stdio.h>

int main() {
    int n, adj[20][20];
    int visited[20] = {0};
    int queue[20], front = 0, rear = 0;
    int i, j, start;

    printf("Enter vertices: ");
    scanf("%d", &n);

    printf("Enter adjacency matrix:\n");
    for (i = 0; i < n; i++)
        for (j = 0; j < n; j++)
            scanf("%d", &adj[i][j]);

    printf("Enter start vertex: ");
    scanf("%d", &start);

    visited[start] = 1;
    queue[rear++] = start;

    printf("BFS reachable: ");

    while (front < rear) {
        int v = queue[front++];
        printf("%d ", v);

        for (i = 0; i < n; i++) {
            if (adj[v][i] && !visited[i]) {
                visited[i] = 1;
                queue[rear++] = i;
            }
        }
    }
    return 0;
}


# code 4 — Quicksort

#include <stdio.h>

void swap(int *a, int *b) { int t = *a; *a = *b; *b = t; }

int partition(int a[], int low, int high) {
    int pivot = a[high], i = low - 1, j;
    for (j = low; j < high; j++)
        if (a[j] <= pivot) { i++; swap(&a[i], &a[j]); }
    swap(&a[i+1], &a[high]);
    return i+1;
}

void quicksort(int a[], int low, int high) {
    if (low < high) {
        int pi = partition(a, low, high);
        quicksort(a, low, pi - 1);
        quicksort(a, pi + 1, high);
    }
}

int main() {
    int n, i, a[100];
    printf("Enter n: "); scanf("%d", &n);
    for (i = 0; i < n; i++) scanf("%d", &a[i]);
    quicksort(a, 0, n-1);
    for (i = 0; i < n; i++) printf("%d ", a[i]);
    return 0;
}


# code 5 — Quicksort Time Measurement

#include <stdio.h>
#include <time.h>

void swap(int *a, int *b) { int t = *a; *a = *b; *b = t; }

int partition(int a[], int low, int high) {
    int pivot = a[high], i = low - 1, j;
    for (j = low; j < high; j++)
        if (a[j] <= pivot) { i++; swap(&a[i], &a[j]); }
    swap(&a[i+1], &a[high]);
    return i+1;
}

void quicksort(int a[], int low, int high) {
    if (low < high) {
        int pi = partition(a, low, high);
        quicksort(a, low, pi - 1);
        quicksort(a, pi + 1, high);
    }
}

int main() {
    int n, i, a[100];
    clock_t start, end;

    printf("Enter n: "); scanf("%d", &n);
    for (i = 0; i < n; i++) scanf("%d", &a[i]);

    start = clock();
    quicksort(a, 0, n-1);
    end = clock();

    printf("Sorted: ");
    for (i = 0; i < n; i++) printf("%d ", a[i]);

    printf("\nTime: %f sec\n", (double)(end - start)/CLOCKS_PER_SEC);
    return 0;
}


# code 6 — Mergesort

#include <stdio.h>

void merge(int a[], int l, int m, int r) {
    int L[100], R[100], n1 = m-l+1, n2 = r-m;
    int i, j, k;

    for (i=0;i<n1;i++) L[i]=a[l+i];
    for (j=0;j<n2;j++) R[j]=a[m+1+j];

    i=j=0;k=l;

    while (i<n1 && j<n2)
        a[k++] = (L[i]<=R[j]) ? L[i++] : R[j++];

    while(i<n1) a[k++] = L[i++];
    while(j<n2) a[k++] = R[j++];
}

void mergesort(int a[], int l, int r) {
    if (l < r) {
        int m=(l+r)/2;
        mergesort(a,l,m);
        mergesort(a,m+1,r);
        merge(a,l,m,r);
    }
}

int main() {
    int n,a[100];
    scanf("%d",&n);
    for (int i=0;i<n;i++) scanf("%d",&a[i]);
    mergesort(a,0,n-1);
    for (int i=0;i<n;i++) printf("%d ",a[i]);
    return 0;
}


# code 7 — Mergesort Time Measurement

#include <stdio.h>
#include <time.h>

void merge(int a[], int l, int m, int r) {
    int L[100], R[100], n1 = m-l+1, n2 = r-m;
    int i,j,k;

    for(i=0;i<n1;i++) L[i]=a[l+i];
    for(j=0;j<n2;j++) R[j]=a[m+1+j];

    i=j=0;k=l;
    while(i<n1 && j<n2)
        a[k++]=(L[i]<=R[j])?L[i++]:R[j++];
    while(i<n1) a[k++]=L[i++];
    while(j<n2) a[k++]=R[j++];
}

void mergesort(int a[], int l, int r) {
    if(l<r){
        int m=(l+r)/2;
        mergesort(a,l,m);
        mergesort(a,m+1,r);
        merge(a,l,m,r);
    }
}

int main() {
    int n,a[100]; clock_t s,e;
    scanf("%d",&n);
    for(int i=0;i<n;i++) scanf("%d",&a[i]);

    s=clock();
    mergesort(a,0,n-1);
    e=clock();

    for(int i=0;i<n;i++) printf("%d ",a[i]);
    printf("\nTime = %f sec\n",(double)(e-s)/CLOCKS_PER_SEC);
    return 0;
}


# code 8 — Topological Sort (Kahn)

#include <stdio.h>

int main() {
    int n,adj[20][20],indeg[20]={0},visited[20]={0};
    scanf("%d",&n);

    for(int i=0;i<n;i++)
        for(int j=0;j<n;j++){
            scanf("%d",&adj[i][j]);
            if(adj[i][j]) indeg[j]++;
        }

    for(int c=0;c<n;c++)
        for(int i=0;i<n;i++)
            if(!visited[i] && indeg[i]==0){
                printf("%d ",i);
                visited[i]=1;
                for(int j=0;j<n;j++)
                    if(adj[i][j]) indeg[j]--;
            }
    return 0;
}


# code 9 — Prim’s MST

#include <stdio.h>
#define INF 999999

int main() {
    int n,cost[20][20],visited[20]={0};
    scanf("%d",&n);

    for(int i=0;i<n;i++)
        for(int j=0;j<n;j++){
            scanf("%d",&cost[i][j]);
            if(cost[i][j]==0 && i!=j) cost[i][j]=INF;
        }

    visited[0]=1;
    int edges=0,min_cost=0;

    while(edges<n-1){
        int u=-1,v=-1,min=INF;
        for(int i=0;i<n;i++)
            if(visited[i])
                for(int j=0;j<n;j++)
                    if(!visited[j] && cost[i][j]<min){
                        min=cost[i][j];
                        u=i; v=j;
                    }
        visited[v]=1;
        printf("%d - %d : %d\n",u,v,min);
        min_cost+=min; edges++;
    }
    printf("Cost = %d\n",min_cost);
    return 0;
}


# code 10 — Dijkstra

#include <stdio.h>
#define INF 999999

int main() {
    int n,cost[20][20],dist[20],visited[20]={0},src;
    scanf("%d",&n);

    for(int i=0;i<n;i++)
        for(int j=0;j<n;j++){
            scanf("%d",&cost[i][j]);
            if(cost[i][j]==0 && i!=j) cost[i][j]=INF;
        }

    scanf("%d",&src);

    for(int i=0;i<n;i++) dist[i]=cost[src][i];
    dist[src]=0; visited[src]=1;

    for(int c=1;c<n;c++){
        int u=-1,min=INF;
        for(int i=0;i<n;i++)
            if(!visited[i] && dist[i]<min){
                min=dist[i]; u=i;
            }
        if(u==-1) break;

        visited[u]=1;

        for(int v=0;v<n;v++)
            if(!visited[v] && dist[u]+cost[u][v] < dist[v])
                dist[v]=dist[u]+cost[u][v];
    }

    for(int i=0;i<n;i++)
        printf("To %d: %d\n",i,dist[i]);
    return 0;
}


# code 11 — Kruskal

#include <stdio.h>
#define INF 999999

struct Edge { int u,v,w; };

int find(int parent[], int i){
    if(parent[i]==i) return i;
    return parent[i]=find(parent,parent[i]);
}

void union_set(int parent[], int rank[], int x, int y){
    int rx=find(parent,x), ry=find(parent,y);
    if(rx==ry) return;
    if(rank[rx]<rank[ry]) parent[rx]=ry;
    else if(rank[rx]>rank[ry]) parent[ry]=rx;
    else { parent[ry]=rx; rank[rx]++; }
}

int main() {
    int n,cost[20][20];
    scanf("%d",&n);

    for(int i=0;i<n;i++)
        for(int j=0;j<n;j++){
            scanf("%d",&cost[i][j]);
            if(cost[i][j]==0) cost[i][j]=INF;
        }

    struct Edge edges[200];
    int e=0;
    for(int i=0;i<n;i++)
        for(int j=i+1;j<n;j++)
            if(cost[i][j]!=INF){
                edges[e].u=i; edges[e].v=j; edges[e].w=cost[i][j];
                e++;
            }

    for(int i=0;i<e-1;i++)
        for(int j=0;j<e-i-1;j++)
            if(edges[j].w > edges[j+1].w){
                struct Edge t=edges[j]; edges[j]=edges[j+1]; edges[j+1]=t;
            }

    int parent[20], rank[20];
    for(int i=0;i<n;i++){ parent[i]=i; rank[i]=0; }

    int mst_cost=0, count=0;

    for(int i=0;i<e && count<n-1;i++){
        int u=edges[i].u, v=edges[i].v, w=edges[i].w;
        int ru=find(parent,u), rv=find(parent,v);
        if(ru!=rv){
            printf("%d - %d : %d\n",u,v,w);
            mst_cost+=w; count++;
            union_set(parent,rank,ru,rv);
        }
    }
    printf("Cost = %d\n",mst_cost);
    return 0;
}


# code 12 — 0-1 Knapsack DP

#include <stdio.h>

int max(int a,int b){ return a>b?a:b; }

int main() {
    int n,W,wt[50],val[50],dp[51][51];
    scanf("%d",&n);

    for(int i=0;i<n;i++) scanf("%d",&wt[i]);
    for(int i=0;i<n;i++) scanf("%d",&val[i]);
    scanf("%d",&W);

    for(int i=0;i<=n;i++)
        for(int w=0;w<=W;w++)
            if(i==0 || w==0) dp[i][w]=0;
            else if(wt[i-1]<=w)
                dp[i][w]=max(val[i-1]+dp[i-1][w-wt[i-1]],dp[i-1][w]);
            else dp[i][w]=dp[i-1][w];

    printf("Max value = %d\n",dp[n][W]);
    return 0;
}


# code 13 — TSP Backtracking

#include <stdio.h>
#define INF 999999

int n,cost[20][20],visited[20],best=INF;

void tsp(int curr,int count,int c){
    if(count==n && cost[curr][0]!=0){
        int total=c+cost[curr][0];
        if(total<best) best=total;
        return;
    }

    for(int i=0;i<n;i++)
        if(!visited[i] && cost[curr][i]!=0){
            visited[i]=1;
            tsp(i,count+1,c+cost[curr][i]);
            visited[i]=0;
        }
}

int main(){
    scanf("%d",&n);
    for(int i=0;i<n;i++)
        for(int j=0;j<n;j++)
            scanf("%d",&cost[i][j]);

    for(int i=0;i<n;i++) visited[i]=0;

    visited[0]=1;
    tsp(0,1,0);

    printf("TSP Min Cost = %d\n",best);
    return 0;
}