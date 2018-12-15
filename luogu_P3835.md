```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<cstdlib>
using namespace std;
const int maxn=30000010;
int q,p,op,x,i;
struct FHQ
{
    int rt[maxn],cnt,lc[maxn],rc[maxn],siz[maxn],pri[maxn],val[maxn];
    void copy(int x,int y){lc[x]=lc[y];rc[x]=rc[y];siz[x]=siz[y];pri[x]=pri[y];val[x]=val[y];}
    void print(int o)
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
        int ret;
        if(pri[x]<pri[y])
        {
        	ret=++cnt;copy(ret,x);//只更新要更新的节点
            rc[ret]=merge(rc[ret],y);
            maintain(ret);return ret;
        }
        else
        {
        	ret=++cnt;copy(ret,y);
            lc[ret]=merge(x,lc[ret]);
            maintain(ret);return ret;
        }
    }
    void split_val(int o,int k,int&x,int&y)
    {
        if(!o){x=y=0;return;}
        if(val[o]<=k)
        {
            x=++cnt;copy(x,o);
            split_val(rc[x],k,rc[x],y);
            maintain(x);
        }
        else
        {
        	y=++cnt;copy(y,o);
        	split_val(lc[y],k,x,lc[y]);
        	maintain(y);
        }
    }
    /*
    void split_siz(int o,int k,int&x,int&y)
    {
        if(!o){x=y=0;return;}
        if(siz[lc[o]]>=k)y=o,split_siz(lc[o],k,x,lc[o]);
        else x=o,split_siz(rc[o],k-siz[lc[o]]-1,rc[o],y);
        maintain(o);
    }
    */
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
        split_val(rt[i],v,x,y);
        rt[i]=merge(merge(x,newnode(v)),y);
    }
    void del(int v)
    {
        int x,y,z;
        split_val(rt[i],v,x,z);
        split_val(x,v-1,x,y);
        y=merge(lc[y],rc[y]);
        rt[i]=merge(merge(x,y),z);
    }
    int findrnk(int v)
    {
        int x,y,ret;
        split_val(rt[i],v-1,x,y);
        ret=siz[x];
        rt[i]=merge(x,y);
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
        split_val(rt[i],v-1,x,y);
        if(siz[x]==0)return -2147483647;
        ret=val[kth(x,siz[x])];
        rt[i]=merge(x,y);
        return ret;
    }
    int suf(int v)
    {
        int x,y,ret;
        split_val(rt[i],v,x,y);
        if(siz[y]==0)return 2147483647;
        ret=val[kth(y,1)];
        rt[i]=merge(x,y);
        return ret;
    }
}st;
int main()
{
    scanf("%d",&q);
    srand(19260817);
    for(i=1;i<=q;i++)
    {
        scanf("%d%d%d",&p,&op,&x);
        st.rt[i]=st.rt[p];
        if(op==1)st.insert(x);
        if(op==2)st.del(x);
        if(op==3)printf("%d\n",st.findrnk(x));
        if(op==4)printf("%d\n",st.val[st.kth(st.rt[i],x)]);
        if(op==5)printf("%d\n",st.pre(x));
        if(op==6)printf("%d\n",st.suf(x));
        //printf("FHQ: ");
        //st.print(st.rt);
        //printf("\n");
    }
    return 0;
}
```
