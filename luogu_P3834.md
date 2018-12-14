```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<map>
#include<set>
using namespace std;
const int maxn=200010;
int n,m,l,r,k,a[maxn],cur,v[maxn];
int cnt,rt[maxn*20],lc[maxn*20],rc[maxn*20],sum[maxn*20];
set<int>st;
map<int,int>mp;
void print(int o,int l,int r)
{
    if(!o)return;
    int mid=(l+r)>>1;
    printf("%d ",sum[o]);
    print(lc[o],l,mid);
    print(rc[o],mid+1,r);
}
void build(int&o,int l,int r)
{
    if(!o)o=++cnt;
    if(l==r){sum[o]=0;return;}
    int mid=(l+r)>>1;
    build(lc[o],l,mid);
    build(rc[o],mid+1,r);
}
void update(int&o,int pre,int l,int r,int x)
{
    if(!o)o=++cnt;
    sum[o]=sum[pre]+1;
    if(l==r)return;
    int mid=(l+r)>>1;
    if(x<=mid)rc[o]=rc[pre],update(lc[o],lc[pre],l,mid,x);
    else lc[o]=lc[pre],update(rc[o],rc[pre],mid+1,r,x);
}
int query(int o1,int o2,int l,int r,int k)//o1,o2 表示两颗统计区间线段树，做差即为区间 [l,r] 所对应的线段树
{
    if(l==r)return l;
    int mid=(l+r)>>1; 
    if(sum[lc[o2]]-sum[lc[o1]]>=k)return query(lc[o1],lc[o2],l,mid,k);
    else return query(rc[o1],rc[o2],mid+1,r,k-sum[lc[o2]]+sum[lc[o1]]);//减小等于左子树中数的个数的值
}
int main()
{
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++)scanf("%d",a+i),st.insert(a[i]);
    for(set<int>::iterator it=st.begin();it!=st.end();it++)mp[*it]=++cur,v[cur]=*it;
    for(int i=1;i<=n;i++)a[i]=mp[a[i]];//离散化
    build(rt[0],1,cur);
    for(int i=1;i<=n;i++)update(rt[i],rt[i-1],1,cur,a[i]);
    //for(int i=0;i<=n;i++)print(rt[i],1,cur),printf("\n");
    while(m--)
    {
        scanf("%d%d%d",&l,&r,&k);
        printf("%d\n",v[query(rt[l-1],rt[r],1,cur,k)]);//查询区间即为 [1,r]-[1,l-1]
    }
    return 0;
} 
```
