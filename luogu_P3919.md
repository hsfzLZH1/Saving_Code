```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int maxn=1000010;
int n,m,a[maxn],vs,op,loc,val;
int cur,rt[maxn*60],lc[maxn*60],rc[maxn*60],v[maxn*60];
void build(int&o,int l,int r)
{
    if(!o)o=++cur;
    if(l==r){v[o]=a[l];return;}
    int mid=(l+r)>>1;
    build(lc[o],l,mid);
    build(rc[o],mid+1,r);
}
void update(int&o,int lst,int l,int r,int x,int vv)
{
    if(!o)o=++cur;
    if(l==r){v[o]=vv;return;}
    int mid=(l+r)>>1;
    if(x<=mid)rc[o]=rc[lst],update(lc[o],lc[lst],l,mid,x,vv);
    else lc[o]=lc[lst],update(rc[o],rc[lst],mid+1,r,x,vv); 
}
int query(int o,int l,int r,int x)//在 o 版本查找节点编号为 x 的值
{
    if(l==r)return v[o];
    int mid=(l+r)>>1;
    if(x<=mid)return query(lc[o],l,mid,x);
    else return query(rc[o],mid+1,r,x);
}
int main()
{
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++)scanf("%d",a+i);
    build(rt[0],1,n);
    for(int i=1;i<=m;i++)
    {
        scanf("%d%d%d",&vs,&op,&loc);
        if(op==1)scanf("%d",&val),update(rt[i],rt[vs],1,n,loc,val);
        else printf("%d\n",query(rt[vs],1,n,loc)),rt[i]=rt[vs];
    }
    return 0;
}
```
