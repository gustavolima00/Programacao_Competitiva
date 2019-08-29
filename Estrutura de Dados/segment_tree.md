# Segment Tree

## Segment Tree 1D

Construção em O(n). Queries e updates em O(log n). memória O(n), Indexado em 0. 

```c++
const int neutral = 0
#define comp(a, b) ((a)+(b))

class SegTree{
    vi a; //using vi = vector<int>
    int n;
public:
    SegTree(vi st){
        int sz = int(st.size());
        for(n = 1; n<sz; n<<=1 );
        a.assign(n<<1, neutral);
        for(int i=0; i<sz; ++i) a[i+n] = st[i];
        for(int i=n+sz-1; i>1; --i)
            a[i>>1] = comp(a[i>>1], a[i]);
    }
    void update(int i, int x){
        a[i+=n] = x; //substitui
        for( i>>=1; i ; i>>=1)
            a[i] = comp(a[i<<1], a[1+(i<<1)]);
    }
    int query(int l, int r){
        int ans = neutral;
        for(l+=n, r+=n+1; l<r; l>>=1, r>>=1){
            if(l&1) ans=comp(ans, a[l++]);
            if(r&1) ans=comp(ans, a[--r]);
        }
        return ans;
    }
};
```
<div style="page-break-after: always;"></div>
