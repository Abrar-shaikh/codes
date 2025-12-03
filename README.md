# code 5
#include <stdio.h>
int main() {
int data[10];
int datatrec[10];
int c,c1,c2,c3,i;
printf("Enter 4 bits of data: ");
scanf("%d",&data[0]);
scanf("%d",&data[1]);
scanf("%d",&data[2]);
scanf("%d",&data[4]);
data[6]= data[0] ^ data[2] ^ data[4];
data[5]= data[0] ^ data[1] ^ data[4];
data[3]= data[0] ^ data[1] ^ data[2];
printf("\nencoded data is :");
for(i=0;i<7;i++){
printf("%d", data[i]);
}
printf("\nEnter recived 7 bits: ");
for(i=0;i<7;i++){
scanf("%d",&datatrec[i]);
}
c1 = datatrec[6] ^ datatrec[4] ^ datatrec[2] ^ datatrec[0];
c2 = datatrec[5] ^ datatrec[4] ^ datatrec[1] ^ datatrec[0];
c3 = datatrec[3] ^ datatrec[2] ^ datatrec[1] ^ datatrec[0];
c = c3 * 4 + c2 * 2 + c1;
if(c==0){
printf("\nNo error recived in data");
} else{
printf("\nerror recived at bit position: %d ", c);
printf("\nData sent :");
for(i=0;i<7;i++) printf("%d", data[i]);
printf("\nData recived :");
for(i=0;i<7;i++) printf("%d", datatrec[i]);
printf("\nData Corrected :");
int idx = 7 - c;
datatrec[idx] = datatrec[idx] ^ 1;
for(i=0;i<7;i++) printf("%d", datatrec[i]);
printf("\n");
}
return 0;
}

#code 6
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
int n, r;
struct frame {
 char ack;
 int data;
} frm[10];
int sender(void);
void recvack(void);
void resend_sr(void);
void resend_gb(void);
void selective(void);
void goback(void);
/* Data Comm. & Computer Network Lab [DCCN ] CSE Department ASET, AUM. */
void goback() {
 sender();
 recvack();
 resend_gb();
 printf("\n all frames sent succesfully \n");
}
void selective() {
 sender();
 recvack();
 resend_sr();
 printf("\n all frames sent succesfully \n");
}
int sender() {
 int i, data;
 printf("\nEnter the no.of frames to be sent:");
 scanf("%d", &n);
 for (i = 1; i <= n; i++) {
 printf("\nEnter the data for frames [%d]: ", i);
 scanf("%d", &data);
 frm[i].data = data;
 frm[i].ack = 'y';
 }
 return 0;
}
void recvack() {
 int i;
 rand();
 r = rand() % n;
 frm[r].ack = 'n';
 for (i = 1; i <= n; i++) {
 if (frm[i].ack == 'n')
 printf("\nThe frame number %d is not recieved\n", r);
 }
}
void resend_sr() {
 printf("\nresending frame %d", r);
 sleep(2);
 frm[r].ack = 'y';
 printf("\nThe received frame is %d", frm[r].data);
}
void resend_gb() { // Fixed: Changed from int to void
 int i;
 printf("\nGo-Back-N: Resending from frame %d onwards.\n", r);
 for (i = r; i <= n; i++) {
 printf("Resending frame %d with data %d\n", i, frm[i].data);
 frm[i].ack = 'y';
 }
}
int main() {
 int ch;
 printf("1. Go Back N\n2. Selective Repeat\nEnter choice: ");
 scanf("%d", &ch);
 switch (ch) {
 case 1:
 goback();
 break;
 case 2:
 selective();
 break;
 default:
 printf("Invalid choice\n");
 }
 return 0;
}

#code 7
NUL=1000
INF=999

def init(n):
    t=[[[i,None,(0 if i==j else INF)][:] for j in range(n)] for i in range(n)]
    # structure: t[src][dst] = [dst_index, next_hop_index or None, dist]
    for i in range(n):
        for j in range(n):
            t[i][j][0]=j
    return t

def inp(t):
    n=len(t)
    for i in range(n):
        print(f"\nEnter link cost from node {i+1} to others:")
        for j in range(n):
            if i==j: continue
            c=int(input(f"Cost to node {j+1} (999 if no link): "))
            if c!=INF:
                t[i][j][2]=c
                t[i][j][1]=j

def update(t):
    n=len(t)
    for _ in range(n):
        for x in range(n):
            for y in range(n):
                for via in range(n):
                    if t[x][via][2]>=INF: continue
                    d=t[x][via][2]+t[via][y][2]
                    if d < t[x][y][2]:
                        t[x][y][2]=d; t[x][y][1]=via

def print_tables(t,desc):
    print(f"\nRouting Tables {desc}:")
    n=len(t)
    for x in range(n):
        print(f"\nRouting table for node {x+1}:\nDEST\tDIST\tNEXT_HOP")
        for i in range(n):
            dist=t[x][i][2]; hop=t[x][i][1]
            if dist>=INF: print(f"{i+1}\tNO LINK\tNO HOP")
            elif hop is None: print(f"{i+1}\t{dist}\tNO HOP")
            else: print(f"{i+1}\t{dist}\t{hop+1}")

def find_route(t,src,dst):
    path=[]
    i=src-1; j=dst-1
    if t[i][j][2]>=INF:
        print("No route")
        return
    while True:
        path.append(i+1)
        if t[i][j][1]==j: break
        i=t[i][j][1]
    print(" -> ".join(map(str,path+[dst])))
    print("Shortest distance =", t[src-1][dst-1][2])

def main():
    while True:
        no=int(input("Enter the number of nodes (1–10): "))
        if 1<=no<=10: break
    t=init(no); inp(t)
    print_tables(t,"(Initial)")
    update(t)
    print_tables(t,"After Path Computation")
    while True:
        if not int(input("\nEnter 0 to exit, any other number to find shortest path: ")): break
        x,y=map(int,input("Enter the nodes (src dest): ").split())
        print(f"\nBest route from node {x} to {y} is:")
        find_route(t,x,y)

if __name__=="__main__":
    main()

#code 8
########client.py
import socket

# Create UDP socket
client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server_address = ('localhost', 12345)

while True:
    query = input("\nEnter Hostname or IP (or 'exit' to quit'): ")

    if query.lower() == 'exit':
        break

    # Send query to server
    client.sendto(query.encode(), server_address)

    # Receive response
    data, _ = client.recvfrom(1024)
    print("Resolved Address:", data.decode())

client.close()

########server.py
import socket

# DNS mapping (static)
dns_table = {
    "www.google.com": "142.250.190.68",
    "142.250.190.68": "www.google.com"
}

# Create UDP socket
server = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server.bind(('localhost', 12345))

print("DNS Server started... waiting for queries")

while True:
    data, addr = server.recvfrom(1024)
    query = data.decode()
    print(f"Received query: {query}")

    # Resolve query
    response = dns_table.get(query, "No record found")
    server.sendto(response.encode(), addr)
    print(f"Sent response: {response}")
