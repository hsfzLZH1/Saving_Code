```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<cstdlib>
#include<ctime>
using namespace std;
const int maxn=100010;
int n,q,l,r;
struct FHQ_Treap
{
    int cnt,rt,lc[maxn],rc[maxn],val[maxn],siz[maxn],pri[maxn],tag[maxn];
    void maintain(int o){siz[o]=siz[lc[o]]+siz[rc[o]]+1;}
    void pushdown(int o){if(tag[o])tag[lc[o]]^=1,tag[rc[o]]^=1,swap(lc[o],rc[o]),tag[o]=0;}
    int newnode(int v){cnt++;lc[cnt]=rc[cnt]=0;val[cnt]=v;siz[cnt]=1;pri[cnt]=rand();return cnt;}
    void print(int o)
    {
        if(!o)return;
        pushdown(o);
        print(lc[o]);
        printf("%d ",val[o]);
        print(rc[o]);
    }
    int merge(int x,int y)
    {
        if(x==0||y==0)return x+y;
        if(pri[x]<pri[y])
        {
            pushdown(x);
            rc[x]=merge(rc[x],y);
            maintain(x);
            return x;
        }
        else
        {
            pushdown(y);
            lc[y]=merge(x,lc[y]);
            maintain(y);
            return y;
        }
    }
    /*
    void split(int o,int k,int&x,int&y)
    {
        if(!o){x=y=0;return;}
        pushdown(o);
        if(val[lc[o]]<k)x=o,split(rc[o],k,rc[o],y);
        else y=o,split(lc[o],k,x,lc[o]);
        maintain(o);
    }
    */
    void split(int o,int k,int&x,int&y)//此题中要按子树大小分裂 Treap ，分裂成最小的 k 个和其它的两个 Treap
    {
        if(!o){x=y=0;return;}
        pushdown(o);
        if(siz[lc[o]]>=k)y=o,split(lc[o],k,x,lc[o]);
        else x=o,split(rc[o],k-siz[lc[o]]-1,rc[o],y);
        maintain(o);
    }
}st;
int main()
{
    srand(377);
    scanf("%d%d",&n,&q);
    for(int i=1;i<=n;i++)st.rt=st.merge(st.rt,st.newnode(i));
    while(q--)
    {
        scanf("%d%d",&l,&r);
        int x,y,z;
        st.split(st.rt,l-1,x,y);
        st.split(y,r-l+1,y,z);
        //st.print(x);printf("\n");
        //st.print(y);printf("\n");
        //st.print(z);printf("\n");
        //printf("j %d %d\n",r-l+1,st.siz[y]);
        st.tag[y]^=1;
        st.rt=st.merge(st.merge(x,y),z);
    }
    st.print(st.rt);
    printf("\n");
    return 0;
}
```
