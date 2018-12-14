```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int maxn=100010;
int n,m,op,k,a,b;
struct persistent_seg//主席树，实现可持久化数组
{
    int cur,rt[maxn*2],lc[maxn*64],rc[maxn*64],val[maxn*64];
    void build(int&o,int l,int r,int v)
    {
        if(!o)o=++cur;
        if(l==r){val[o]=v;return;}
        int mid=(l+r)>>1;
        build(lc[o],l,mid,v);
        build(rc[o],mid+1,r,v);
    }
    void print(int o,int l,int r)
    {
        if(!o)return;
        if(l==r){printf("%d ",val[o]);return;}
        int mid=(l+r)>>1;
        print(lc[o],l,mid);
        print(rc[o],mid+1,r);
    }
    void update(int&o,int pre,int l,int r,int x,int v)
    {
        if(!o)o=++cur;
        if(l==r){val[o]=v;return;}
        int mid=(l+r)>>1;
        if(x<=mid)rc[o]=rc[pre],update(lc[o],lc[pre],l,mid,x,v);
        else lc[o]=lc[pre],update(rc[o],rc[pre],mid+1,r,x,v);
    }
    int query(int o,int l,int r,int x)
    {
        if(!o)return 0;
        if(l==r)return val[o];
        int mid=(l+r)>>1;
        if(x<=mid)return query(lc[o],l,mid,x);
        else return query(rc[o],mid+1,r,x);
    }
}f,dep;
int findp(int x,int vs)//查找根节点
{
    int fx=f.query(f.rt[vs],1,n,x);
    if(fx)return findp(fx,vs);
    return x;
}
int main()
{
    scanf("%d%d",&n,&m);
    f.build(f.rt[0],1,n,0);dep.build(dep.rt[0],1,n,1);
    for(int i=1;i<=m;i++)
    {
        scanf("%d",&op);
        if(op==1)
        {
            scanf("%d%d",&a,&b);
            int u=findp(a,i-1),v=findp(b,i-1);
            if(u!=v)//合并
            {
                int du=dep.query(dep.rt[i-1],1,n,u),dv=dep.query(dep.rt[i-1],1,n,v);
                if(du>dv)f.update(f.rt[i],f.rt[i-1],1,n,v,u),dep.update(dep.rt[i],dep.rt[i-1],1,n,u,max(du,dv+1));
                else f.update(f.rt[i],f.rt[i-1],1,n,u,v),dep.update(dep.rt[i],dep.rt[i-1],1,n,v,max(dv,du+1));
            }
            else f.rt[i]=f.rt[i-1],dep.rt[i]=dep.rt[i-1];
        }
        if(op==2)
        {
            scanf("%d",&k);
            f.rt[i]=f.rt[k];dep.rt[i]=dep.rt[k];//回到第 $k$ 次操作后的状态
        }
        if(op==3)
        {
            scanf("%d%d",&a,&b);
            f.rt[i]=f.rt[i-1];dep.rt[i]=dep.rt[i-1];//查询操作，注意要将该次操作的根节点指针赋值为上一次操作所代表的的根节点
            int u=findp(a,i),v=findp(b,i);
            if(u==v)printf("1\n");
            else printf("0\n");
        }
        //f.print(f.rt[i],1,n);printf("\n");
        //dep.print(dep.rt[i],1,n);printf("\n");
    }
    return 0;
}
```
