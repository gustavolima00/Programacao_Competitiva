<div  style="font-size: 34px">
Matemática
</div>

# Primos e divisores

## Descrição 

- sieve: Crivo de Eristótenes, armazena no vetor primes os primos até **MAXN** em complexidade **O(N)**;
- get_pots: Retorna fatores primoes e suas respectivas potências
- gdc: Calcula maior divisor comum em complexidade **O(log a + log b)**
- lmc: Calcula o menor múltiplo comum em complexidade **O(log a + log b)**
- num_div: Calcula o números de divisores de N em complexidade **O(√n)**

```c++
using ii = pair<int, int>;
using ll = long long;
#define MAXN 10000009
int lp[N+1];
vector<int> pr;

void sieve(){
    for (int i=2; i<N; ++i) {
        if (lp[i] == 0) {
            lp[i] = i;
            pr.push_back (i);
        }
        for (int j=0; j<(int)pr.size() && pr[j]<lp[i] && i*pr[j]<N; ++j)
            lp[i * pr[j]] = pr[j];
    }
}

vector<ii> get_pots(int n){
        vector<ii> res;
        int i = 0, p = pr[i];
        while(p*p<=n){
                int q = 0;
                while(n%pr[i] == 0) n/=pr[i], ++q;
                if(q) res.emplace_back(p, q);
                p = pr[++i];
        }
        if(n!=1) res.emplace_back(n, 1);
        return res;
}

int gcd(int a, int b){
    return b==0 ? a : gcd(b, a%b);
}
int lmc(int a, int b){
    return a*(b/gcd(a, b));
}
int num_div(int n){
    int ans=0;
    for(int i=1; i*i<=n; ++i)
        if(n%i == 0)
            ans+=( i==n/i ? 1 : 2);
    return ans;
}
```

<div style="page-break-after: always;"></div>

# Aritimética modular

## Descrição
- ext_gcd: Algorítimo estendido de euclides complexidade:**O(log a + log b)**
- mod_inv: Calcula inverso modular de a em módulo m complexidade:**O(log a+b)**
- mod_exp: Calcula a elevado à b em módulo m em complexidade:**O(log b)** Pode ser usado sem o segundo argumento para calcular o inverso de a se mod for primo
- mod_mul: Calcula (a*b)%m sem overflow em complexidade:**O(log a + log b)**
- diophantine: Acha uma solução na equação no formato ax+by=c, sendo x e y as incognitas; Após a divisão de a, b e c pelo mdc(a, b) as outras soluções são x=x0+bt, y=y0-at; complexidade:**O(log a + log b)**
- preprocess_fat: Pré-processa os fatorias em módulo m até MAXN em complexidade **O(MAXN)**

É necessário definir a variável **mod** globalmente 
Se necessário use a macro

```c++
#define int long long

int32_t main(){

}
```

```c++
int mod = 1e9+7;

int ext_gcd(int a, int b, int& x, int& y){
    if(b==0){
        x=1; y=0; return a;
    }
    else{
        int g = ext_gcd(b, a%b, y, x);
        y-=a/b*x; return g;
    }
}
int mod_inv(int a){
    int x, y;
    ext_gcd(a, mod, x, y);
    return (x%mod+mod)%mod;
}
int mod_mul(int a, int b){
    int x=0, y=a%mod;
    while(b>0){
        if(b%2==1) x=(x+y)%mod;
        y=(y*2)%mod; b/=2;
    }
    return x%mod;
}
int mod_exp(int a, int b=mod-2){
    int ans = 1;
    while(b){
        if (b&1) ans = (ans*a)%mod;
        a = (a*a)%mod;
        b = b/2;
    }
    return ans;
}

int mod_exp(int a, int b=mod-2){
    if(b == 0) return 1;
    int c = mod_exp(a, b/2);
    c = (c*c)%mod;
    if(b%2 != 0) c=(c*a)%mod;
    return c;
}
void diophantine(int a, int b, int c, int& x, int& y){
    int d = ext_gcd(a, b, x, y);
    x *= c/d; y*= c/d;
}

#define MAXN 1000009
int fat[MAXN];
void preprocess_fat(){
    fat[0] = 1;
    for(int i=1; i<MAXN; ++i)
        fat[i]=(i*fat[i-1])%mod;
}
```
<div style="page-break-after: always;"></div>

<div style="page-break-after: always;"></div>

# Transformada rápida de Fourier (FFT)

## Descrição

- fft: Transforma o vetor xs para o campo das frequências(invert=false) ou do tempo(invert=true) em complexidade: **O(N log N)** sendo N o tamanho do vetor
- conv: Armazena em res a convolução dos polinômios a e b (multiplicação entre eles) em complexidade: **O(N log N)** sendo N o maior grau entre os 2 polinômios

```c++
#include <complex>
using base = complex<double>;
const double PI = acos(-1);

void fft(vector<base>& a, bool invert){
	int n = a.size();
	if(n == 1) return;

	vector<base> a0(n/2), a1(n/2);
	for(int i=0; 2*i<n; ++i){
		a0[i] = a[2*i];
		a1[i] = a[2*i+1];
	}
	fft(a0, invert);
	fft(a1, invert);

	double ang = 2*PI/n*(invert ? -1:1);
	base w{1}, wn{cos(ang), sin(ang)};
	for(int i=0; 2*i<n; ++i){
		a[i] = a0[i]+w*a1[i];
		a[i+n/2] = a0[i]-w*a1[i];
		if(invert){
			a[i] /= 2;
			a[i+n/2] /=2;
		}
		w *= wn;
	}
}

void conv(vector<base> a, vector<base> b, vector<base> &res){
	int n = 1;
	while(n<(int)max(a.size(), b.size())) n <<= 1;
	n <<= 1;
	a.resize(n), b.resize(n);
	fft(a, false); fft(b, false);
	res.resize(n);
	for(int i=0; i<n; ++i) res[i] = a[i]*b[i];
	fft(res, true);
}
```