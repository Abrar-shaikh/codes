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


//C Code to Measure Time Taken by Merge Sort
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

// Merge function
void merge(int arr[], int left, int mid, int right) {
    int i, j, k;
    int n1 = mid - left + 1;
    int n2 = right - mid;

    // Temporary arrays
    int *L = (int *)malloc(n1 * sizeof(int));
    int *R = (int *)malloc(n2 * sizeof(int));

    // Copy data
    for (i = 0; i < n1; i++) L[i] = arr[left + i];
    for (j = 0; j < n2; j++) R[j] = arr[mid + 1 + j];

    i = 0; j = 0; k = left;

    // Merge
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) arr[k++] = L[i++];
        else arr[k++] = R[j++];
    }

    // Copy remaining elements
    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];

    free(L);
    free(R);
}

// Merge Sort
void mergeSort(int arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left)/2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid+1, right);
        merge(arr, left, mid, right);
    }
}

int main() {
    int n;
    printf("Enter number of elements: ");
    scanf("%d", &n);

    int *arr = (int *)malloc(n * sizeof(int));
    if (!arr) {
        printf("Memory allocation failed.\n");
        return 1;
    }

   printf("enter %d number of elements: ",n);
   for(int i=0;i<n;i++){
        scanf("%d",&arr[i]);
   }

    // Start timer
    clock_t start = clock();

    mergeSort(arr, 0, n - 1);

    // Stop timer
    clock_t end = clock();

    double time_taken = ((double)(end - start)) / CLOCKS_PER_SEC;
    printf("sorted elements: ");
    for(int i=0;i<n;i++){
        printf("%d ",arr[i]);
    }
    printf("\n");
    printf("Time taken to sort %d elements: %f seconds\n", n, time_taken);

    free(arr);
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
    int n;
    printf("Enter number of vertices: ");
    scanf("%d", &n);
    int cost[n][n];
    printf("Enter the adjacency matrix (0 for no edge):\n");
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            scanf("%d", &cost[i][j]);
            if (cost[i][j] == 0 && i != j) {
                cost[i][j] = INF;  // No edge becomes INF
            }
        }
    }

    int selected[n];      // Mark visited vertices
    for (int i = 0; i < n; i++)
        selected[i] = 0;
    selected[0] = 1;      // Start with vertex 0
    int edges = 0;
    int total_cost = 0;
    printf("\nEdges selected in Minimum Spanning Tree:\n");
    while (edges < n - 1) {
        int min = INF;
        int a = -1, b = -1;
        // Find minimum cost edge connecting selected → unselected
        for (int i = 0; i < n; i++) {
            if (selected[i]) {  // already in MST
                for (int j = 0; j < n; j++) {
                    if (!selected[j] && cost[i][j] < min) {
                        min = cost[i][j];
                        a = i;
                        b = j;
                    }
                }
            }
        }

        printf("%d  %d  | Weight = %d\n", a, b, cost[a][b]);
        total_cost += cost[a][b];
        selected[b] = 1;
        edges++;
    }
    printf("\nTotal Minimum Cost = %d\n", total_cost);
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

//C code on Travelling sales man problem.

#include <stdio.h>
#define INF 9999999
#define N 4 // Number of cities

int dist[N][N] = {
    {0, 10, 15, 20},
    {10, 0, 35, 25},
    {15, 35, 0, 30},
    {20, 25, 30, 0}
};

int dp[1 << N][N]; // dp[mask][i]

int tsp(int mask, int pos) 
{
    if (mask == (1 << N) - 1) 
{
        return dist[pos][0]; // Return to the starting city
    }

    if (dp[mask][pos] != -1) 
{
        return dp[mask][pos];
    }

    int ans = INF;
    for (int city = 0; city < N; city++) 
{
        if (!(mask & (1 << city))) 
{
            int newAns = dist[pos][city] + tsp(mask | (1 << city), city);
            ans = (newAns < ans) ? newAns : ans;
        }
    }
    return dp[mask][pos] = ans;
}

int main() {
    for (int i = 0; i < (1 << N); i++) 
{
        for (int j = 0; j < N; j++) 
{
            dp[i][j] = -1;
        }
    }
    printf("The minimum cost is %d\n", tsp(1, 0));
    return 0;
}
