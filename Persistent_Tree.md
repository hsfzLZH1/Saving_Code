```cpp
void print(int o,int l,int r)//前序遍历输出版本编号为 o 的线段树
{
    if(!o)return;// 0 代表该节点不存在
    int mid=(l+r)>>1;
    printf("%d ",sum[o]);//sum 是维护的值：区间和
    print(lc[o],l,mid);//左子树
    print(rc[o],mid+1,r);//右子树
}
void build(int&o,int l,int r)//以 o 为根， [l,r] 为区间建树，o 传引用是为了方便传参和赋值
{
    if(!o)o=++cnt;//如果该节点原来是空的（这里肯定是空的）
    if(l==r){sum[o]=0;return;}//递归边界，区间只有一个元素，初始全部赋值为 0 
    int mid=(l+r)>>1;
    build(lc[o],l,mid);
    build(rc[o],mid+1,r);
}
void update(int&o,int pre,int l,int r,int x)//以 pre 为上一个版本， o 为当前版本，使位置 x 上的值自增 1 ，并维护区间的和
{
    if(!o)o=++cnt;//如果新节点为空
    sum[o]=sum[pre]+1;//在上一个版本的基础上计算当前版本的值
    if(l==r)return;
    int mid=(l+r)>>1;
    if(x<=mid)rc[o]=rc[pre],update(lc[o],lc[pre],l,mid,x);//如果 x 在当前区间的左侧，则右侧区间和原来相等，并递归更新左侧区间
    else lc[o]=lc[pre],update(rc[o],rc[pre],mid+1,r,x);//同上
}
```
