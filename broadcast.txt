#include<stdio.h>
#include<limits.h>
#include<string.h>
#define max 10
int n,adj_mat[max][max],finalPath[max][max];
struct node{
int predecessor;
int sucessor;
int length;
enum {permanent,tentative} label;
};
void shortestPath(int root,int path[])
{
int i,j,min,k,count=0,prev;
struct node ele[n];
for(i=0;i<n;i++){
ele[i].predecessor=-1;
ele[i].sucessor=-1;
ele[i].length=INT_MAX;
ele[i].label=tentative;
}
ele[root].length=0;
ele[root].label=permanent;
k=root;
do
{
prev=k;
for(i=0;i<n;i++){
if((adj_mat[k][i]!=0)&&(ele[i].label==tentative))
{
if(adj_mat[k][i]+ele[k].length<ele[i].length)
{
ele[i].predecessor=k;
ele[i].length=ele[k].length+adj_mat[k][i];
}
}
}
k=0;
min=INT_MAX;
for(i=0;i<n;i++){
if(ele[i].label==tentative&&ele[i].length<min)
{
min=ele[i].length;
k=i;
}
}
ele[k].label=permanent;//making node as permanent
ele[ele[k].predecessor].sucessor=1;
count++;
}while(count<n);//computhing shortest to all node
for(i=0;i<n;i++)
{
j=0;
k=i;
if(ele[i].sucessor==-1)
{
do
{
finalPath[i][j++]=k;
k=ele[k].predecessor;
}while(k>=0);
}
path[i]=j;
}
return;
}
int main(){
printf("Enter number of nodes..:\n");
scanf("%d",&n);
printf("Enter graph in adjacency matrix form..:\n");
for(int i=0;i<n;i++){
for(int j=0;j<n;j++){
scanf("%d",&adj_mat[i][j]);
}
}
int root,j,path[max];
printf("Enter root node..:\n");
scanf("%d",&root);
shortestPath(root,path);
printf("The paths are : \n");
for(int i=0;i<n;i++)//printing broadcast tree
{
if(path[i]>0)
{
for(int j=path[i]-1;j>=0;j--)
{
if(j==0)
{
printf("%d",finalPath[i][j]);
}
else
{
printf("%d-->",finalPath[i][j]);
}
}
printf("\n");
}
}
return 0;
}
