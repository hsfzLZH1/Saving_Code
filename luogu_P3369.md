```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<cstdlib>
using namespace std;
const int maxn=100010;
int q,op,x;
struct FHQ
{
    int rt,cnt,lc[maxn],rc[maxn],siz[maxn],pri[maxn],val[maxn];
    void print(int o)//打印以 $o$ 为根的 Treap，用来查错
    {
        if(!o)return;
        print(lc[o]);
        printf("%d ",val[o]);
        print(rc[o]);
    }
    void maintain(int o){siz[o]=siz[lc[o]]+siz[rc[o]]+1;}
    int merge(int x,int y)
    {
        if(x==0||y==0)return x+y;
        maintain(x);maintain(y);
        if(pri[x]<pri[y])
        {
            rc[x]=merge(rc[x],y);
            maintain(x);return x;
        }
        else
        {
            lc[y]=merge(x,lc[y]);
            maintain(y);return y;
        }
    }
    void split_val(int o,int k,int&x,int&y)
    {
        if(!o){x=y=0;return;}
        if(val[o]<=k)x=o,split_val(rc[o],k,rc[o],y);
        else y=o,split_val(lc[o],k,x,lc[o]);
        maintain(o);
    }
    void split_siz(int o,int k,int&x,int&y)
    {
        if(!o){x=y=0;return;}
        if(siz[lc[o]]>=k)y=o,split_siz(lc[o],k,x,lc[o]);
        else x=o,split_siz(rc[o],k-siz[lc[o]]-1,rc[o],y);
        maintain(o);
    }
    int newnode(int v)
    {
        cnt++;
        lc[cnt]=rc[cnt]=0;
        val[cnt]=v;
        siz[cnt]=1;
        pri[cnt]=rand();
        return cnt;
    }
    void insert(int v)
    {
        int x,y;
        split_val(rt,v,x,y);
        rt=merge(merge(x,newnode(v)),y);
    }
    void del(int v)
    {
        int x,y,z;
        split_val(rt,v,x,z);
        split_val(x,v-1,x,y);
        y=merge(lc[y],rc[y]);
        rt=merge(merge(x,y),z);
    }
    int findrnk(int v)
    {
        int x,y,ret;
        split_val(rt,v-1,x,y);
        ret=siz[x];
        rt=merge(x,y);
        return ret+1;
    }
    int kth(int o,int k)
    {
        if(siz[lc[o]]>=k)return kth(lc[o],k);
        if(k==siz[lc[o]]+1)return o;
        return kth(rc[o],k-siz[lc[o]]-1);
    }
    int pre(int v)
    {
        int x,y,ret;
        split_val(rt,v-1,x,y);
        ret=val[kth(x,siz[x])];
        rt=merge(x,y);
        return ret;
    }
    int suf(int v)
    {
        int x,y,ret;
        split_val(rt,v,x,y);
        ret=val[kth(y,1)];
        rt=merge(x,y);
        return ret;
    }
}st;
int main()
{
    scanf("%d",&q);
    srand(377);
    while(q--)
    {
        scanf("%d%d",&op,&x);
        if(op==1)st.insert(x);
        if(op==2)st.del(x);
        if(op==3)printf("%d\n",st.findrnk(x));
        if(op==4)printf("%d\n",st.val[st.kth(st.rt,x)]);
        if(op==5)printf("%d\n",st.pre(x));
        if(op==6)printf("%d\n",st.suf(x));
        //printf("FHQ: ");
        //st.print(st.rt);
        //printf("\n");
    }
    return 0;
}
```
