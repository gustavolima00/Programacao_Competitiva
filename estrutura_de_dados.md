<div  style="font-size: 34px">
Estrutura de dados
</div>

# Fenwick Tree
Todas as implementações abaixo são indexadas em 1 portanto **não deve ser feito a consulta do índice 0**

## Range Sum Query em O(log n) e atualização com soma pontual

```c++
const int neutral = 0;
#define comp(a, b) ((a)+(b))

class BITree {
private:
    vector<int> ft;
public:
    BITree(int n) { ft.assign(n+1, 0); }
    int rsq(int i){
        int sum = neutral;
        for(; i; i-= (i&-i))
            sum = comp(sum, ft[i]);
        return sum;
    }
    int rsq(int i, int j){
        return rsq(j)-rsq(i-1);
    }
    void update(int i, int v){
        for(; i<(int) ft.size(); i+= (i&-i))
            ft[i] = comp(v, ft[i]);
    }
};
```
<div style="page-break-after: always;"></div>

## Range update em O(log n) query pontual

Construção com todos os valores zerados indexada em 0.
 
```c++
class BITree{
private:
	vector<ll> a;
	int n;
public:
	BITree(int _n){
		n = _n;
		a.assign(n+2, 0);
	}
	ll at(int i) const {
		ll sum = 0;
		for(++i; i; i-=i&-i) sum += a[i];
		return sum;
	}
	ll operator [](int i) const{
		return at(i);
	}
	void add(int i, int j, ll x){
		for(++i; i<=n; i+=i&-i) a[i] += x;
		for(j+=2; j<=n; j+=j&-j) a[j] -= x;
	}
	void add(int i, ll x){
		add(i, i, x);
	}
};
```
<div style="page-break-after: always;"></div>

## Query bidimencional em O(log n.m) update com soma pontual 

```c++
class BITree2D{
private:
    size_t rows;
    size_t columns;
    vector<vector<long long>> ft;

    int LSB(long long n) { return n&(-n); }

    long long RSQ(int y, int x){
        long long sum = 0;

        for(int i=y; i>0; i-=LSB(i))
            for(int j=x; j>0; j-=LSB(j))
                sum+=ft[i][j];
        
        return sum;
    }
public:
    BITree2D(size_t y, size_t x) : ft(y+1, vector<long long>(x+1, 0)), rows(y), columns(x) {}

    // Y e X sao as coordenadas do ponto superior
    // x e y sao as coordenadas do ponto inferior 
    // RSQ vai retornar a soma do quadrado formado pelos pontos
    long long RSQ(int y, int x, int Y, int X){ 
        return RSQ(Y, X) - RSQ(Y, x-1) - RSQ(y-1, X) + RSQ(y-1, x-1);
    }
    
    void add(int y, int x, long long v){
        for(int i=y; i<=rows; i+=LSB(i))
            for(int j=x; j<=columns; j+=LSB(j))
                ft[i][j] += v;
    }
    
};

```
<div style="page-break-after: always;"></div>

## Range Product Query em O(log n) e update com multiplicação pontual

```c++
class BITree{
private:
    size_t N;
    vector<long long> ft;
    vector<int> sz; // 1 se o indice i for 0

    int LSB(const long long& n) { return n&(-n); }

    long long RPQ(int i){
        long long prod = 1;
        while(i){
            prod*=ft[i];
            i-=LSB(i);
        }
        return prod;
    }

    // Funções rsq para o vetor com zeros
    int RSQ(int i){
        int sum = 0;
        while(i){
            sum+=sz[i];
            i-=LSB(i);
        }
        return sum;
    }

    int RSQ(int i, int j){
        return RSQ(j)-RSQ(i-1);
    }

    void multiply(int i, long long k){
        while(i <= N){
            ft[i] *= k;
            i+=LSB(i);
        }
    }

    void add(int i, int k){
        while(i <= N){
            sz[i] += k;
            i+=LSB(i);
        }
    }

public:

    BITree(int n) : N(n), ft(n+1, 1), sz(n+1, 0) { }

    long long RPQ(int i, int j){
        long long p = RPQ(j)/RPQ(i-1);
        int z = RSQ(i, j);

        if(z) return 0;
        else return p;
    }

    void update(int i, const long long& k){
        if(k)
            multiply(i, k);
        else if (RSQ(i, i)==0)
            add(i, 1);
    }
    
};
```

<div style="page-break-after: always;"></div>

# Segment Tree

## Segment Tree 1D

Construção em O(n). Queries e updates em O(log n). memória O(n), Indexado em 0. 

```c++
const int neutral = 0
#define comp(a, b) ((a)+(b))

class SegTree{
    vector<int> a;
    int n;
public:
    SegTree(int _n){
        int sz = _n;
        for(n = 1; n<sz; n<<=1 );
        a.assign(n<<1, neutral);
    }
    void update(int i, int x){
        a[i+=n] = x;
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

## SegmentTree com Lazy Propagation

```c++
const int neutral = -1e9;
#define comp(a, b) max((a), (b))
 
class SegmentTree {
private:
	vector<int> st, lazy;
	int size;
#define left(p) (p<<1)
#define right(p) ((p<<1) + 1) 
	void build(int p, int l, int r, int* A){
		if(l == r) { st[p] = A[l]; return; }
		int m = (l + r)/2;
		build(left(p), l, m, A);
		build(right(p), m+1, r, A);
		st[p] = comp(st[left(p)], st[right(p)]);
	}
	void push(int p, int l, int r){
		//st[p] += (r-l+1)*lazy[p]; // Caso RSQ
		st[p] += lazy[p]; // Caso RMQ
		if(l != r){
			lazy[right(p)] += lazy[p];
			lazy[left(p)] += lazy[p];
		}
		lazy[p] = 0;
	}
	void update(int p, int l, int r, int a, int b, int k){
		push(p, l, r);
		if(a>r || b<l) return;
		else if(l>=a && r<=b){
			lazy[p] = k;  push(p, l, r); return;
		}
		update(left(p), l, (l + r) / 2, a, b, k);
		update(right(p), (l + r) / 2 + 1, r, a, b, k); 
		st[p] = comp(st[left(p)], st[right(p)]);
	}
	int query(int p, int l, int r, int a, int b){
		push(p, l, r);
		if(a > r || b < l) return neutral;
		if(l >= a && r <= b) return st[p];
		int m = (l + r)/2;
		int p1 = query(left(p), l, m, a, b);
		int p2 = query(right(p), m+1, r, a, b);
		return comp(p1, p2);
	}
public:
	SegmentTree(int* bg, int* en){
		size = (int)(en - bg);
		st.assign(4*size, neutral);
		lazy.assign(4*size, 0);
		build(1, 0, size-1, bg);
	}
	int query(int a, int b){
		return query(1, 0, size-1, a, b);
	}
	void update(int a, int b, int k){
		return update(1, 0, size-1, a, b, k);
	}
};
```

<div style="page-break-after: always;"></div>

## SparseTable

Construção em O(N log N) consulta em O(1)

```c++
#define comp(a, b) min((a), (b))
int st[MAXN][MAXLOGN];
 
void init(int* bg, int*en){
	int sz = int(en-bg);
	for(int i=0; i<sz; ++i) st[i][0] = bg[i];
	for(int j=1; 1 << j <= sz; ++j)
		for(int i=0; i + (1<<j) <= sz; ++i)
			st[i][j] = comp(st[i][j-1], st[i + (1<<(j-1))][j-1]);
}
int query(int l, int r){
	int k = (int) floor(log((double)r-l+1) / log(2.0));
	return comp(st[l][k], st[r-(1<<k)+1][k]);
}
```

## Union Find (DSU)

Construção em O(N) find em ~O(1)
```c++
#define MAXN 300010
int p[MAXN];
int tam[MAXN];

void init(){
	for(int i=0; i<MAXN; ++i) p[i] = i, tam[i]=1;
}
int find(int i){
	return p[i]==i ? i : p[i] = find(p[i]);
}
void join(int i, int j){
	int x = find(i), y = find(j);
	if(x!=y){
		if(tam[x]>tam[y]) swap(x, y);
		p[x] = y;
		tam[y]+=tam[x];
	}
}
```