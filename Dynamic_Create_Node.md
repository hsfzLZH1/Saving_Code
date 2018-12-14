```cpp
struct SegTree
{
    struct node{int lc,rc,sum_,max_;}s[maxn*16];
    void Add(int o,int l,int r,int x,int v,int a)
    {
        if(l==r){s[o].sum_=s[o].max_=v;col[x]=a;mrk[x]=v;return;}
        int mid=(l+r)>>1;
        if(x<=mid)
        {
            if(!s[o].lc)s[o].lc=++cur;
            Add(s[o].lc,l,mid,x,v,a);
        }
        else
        {
            if(!s[o].rc)s[o].rc=++cur;
            Add(s[o].rc,mid+1,r,x,v,a);
        }
        s[o].sum_=s[s[o].lc].sum_+s[s[o].rc].sum_;
        s[o].max_=max(s[s[o].lc].max_,s[s[o].rc].max_);
    }
    void Update(int o,int l,int r,int x,int v)
    {
        if(l==r){s[o].sum_=s[o].max_=v;mrk[x]=v;return;}
        int mid=(l+r)>>1;
        if(x<=mid)Update(s[o].lc,l,mid,x,v);
        else Update(s[o].rc,mid+1,r,x,v);
        s[o].sum_=s[s[o].lc].sum_+s[s[o].rc].sum_;
        s[o].max_=max(s[s[o].lc].max_,s[s[o].rc].max_); 
    }
    int QuerySum(int o,int l,int r,int ql,int qr)
    {
        if(!o)return 0;
        if(l>=ql&&r<=qr)return s[o].sum_;
        if(r<ql||l>qr)return 0;
        int mid=(l+r)>>1;
        return QuerySum(s[o].lc,l,mid,ql,qr)+QuerySum(s[o].rc,mid+1,r,ql,qr);
    }
    int QueryMax(int o,int l,int r,int ql,int qr)
    {
        if(!o)return 0;
        if(l>=ql&&r<=qr)return s[o].max_;
        if(r<ql||l>qr)return 0;
        int mid=(l+r)>>1;
        return max(QueryMax(s[o].lc,l,mid,ql,qr),QueryMax(s[o].rc,mid+1,r,ql,qr));
    }
}st;
```
