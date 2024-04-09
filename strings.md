<div  style="font-size: 34px">
Strings
</div>

## Hash

Pode ser utilizado para comparar 2 strings em O(1). Os algorítimos de hash são base para outros algorítimos de string

**O valor de p foi inserido considerando um alfabeto de 26 letras, caso esse valor mude ajustes o valor para um primo próximos ao tamanho do alfabeto**

```c++
const long long p = 31, q = 1e9+7;

int f(char c){
        return c-'a'+1;
}

long long h(const string& s){
        long long ans = 0;
        for(auto it = s.rbegin(); it!=s.rend(); ++it)
        {
                ans = (ans * p) % q;
                ans = (ans + f(*it)) % q;
        }
        return ans;
}

```

## Hash duplo

O mesmo princípio do hash simples, porém o hash duplo tem menos chances de colisão.

> Dadas duas strings S e T escolhidas aleatoriamente, a probabilidade de colisão entre ambas é de 1/q, assim, com q = 10^9 + 7, se S for comparado com n=10^6 strings distintas, a probabilidade de acontecer uma colisão é igual a n/q = 10^3

Para diminuir essa probabilidade o hash duplo combina 2 hashes diferentes, diminuindo bastante a probabilidade de colisão

**Os valores de p1 e p2 foram inseridos considerando um alfabeto de 26 letras, caso esse valor mude ajustes os valores para primos próximos ao tamanho do alfabeto**

```c++
int f(char c){
        return c-'a'+1;
}

int hi(long long pi, long long qi, const string& s){
        long long ans = 0;
        for(auto it = s.rbegin(); it!=s.rend(); ++it)
        {
                ans = (ans * pi) % qi;
                ans = (ans + f(*it)) % qi;
        }
        return ans;
}

pair<int, int> h(const string& s){
        const long long p1 = 31, q1 = 1e9+7;
        const long long p2 = 29, q2 = 1e9+9;

        return make_pair(hi(p1, q1, s), hi(p2, q2, s));
}
```

## String Hash e String Double Hash

Pré computação dos hashes da string em O(N log(N)) e acesso ao hash da substring S[i..j] em O(1)

Por padrão o valor de p está em 31, que funciona bem para o alfabeto de 'a' a 'z', caso precise mude o valor para um primo próximo ao tamanho do alfabeto, altere também a função `int f(char c)` e lembre-se de não retornar valores nulos na função.

A função StringDoubleHash funciona bem para strings grandes, por ter um hash duplo a chance de colisão é menor.

```c++
class StringHash{
private:
        ll mod_exp(ll a, ll n, const ll& mod){
                ll res = 1, base = a;
                while(n){
                        if(n&1) res = (res*base)%mod;

                        base = (base*base)%mod;
                        n >>=1;
                }
                return res;
        }

        int f(char c){
                return c-'a'+1;
        }
public:
        ll p, q;
        vector<ll> ps;
        vector<ll> is;

        StringHash(string s = "", ll _p = 31, ll _q = 1e9+7){
                q = _q;
                p = _p;
                auto n = s.size();
                // Calculate prefixes
                ps.assign(n, 0);
                ll res = 1;
                for(int i=0; i<n; ++i){
                        ps[i] = (f(s[i])*res)%q;

                        if(i!=0) ps[i] = (ps[i-1] + ps[i])%q;
                        res = (res*p)%q;
                }

                // Calculate inverses
                is.assign(n, 0);
                ll base = 1;
                for(int i=0; i<n; ++i){
                        is[i] = mod_exp(base, q-2, q);
                        base = (base*p)%q;
                }
        }

        ll hash(int i, int j){
                auto diff = i>0 ? ps[j] - ps[i-1] : ps[j];
                diff = (diff*is[i])%q;

                return (diff + q)%q;
        }
};

class StringDoubleHash{
        private:
                StringHash h1, h2;
        public:
                StringDoubleHash(string s, ll p1 = 31, ll p2 = 29, ll q1 = 1e9+7, ll q2 = 1e9+9){
                        h1 = StringHash(s, p1, q1);
                        h2 = StringHash(s, p2, q2);
                }

                ii hash(int i, int j){
                        return make_pair(h1.hash(i, j), h2.hash(i, j));
                }
};


// Exemplo de uso hash simples:
int unique_substrings(const string& s){
        StringHash sh(s);
        int n = s.size();

        unordered_set<ll> st;
        for(int i=0; i<n; ++i){
                for(int j=i; j<n; ++j){
                        st.insert(sh.hash(i, j));
                }
        }
        return st.size();
}

// Exemplo de uso hash duplo
int unique_substrings(const string& s){
        StringDoubleHash sh(s);
        int n = s.size();

        set<ii> st;
        for(int i=0; i<n; ++i){
                for(int j=i; j<n; ++j){
                        st.insert(sh.hash(i, j));
                }
        }
        return st.size();
}

```

