# Divisores

## Descrição 
- gdc: Calcula maior divisor comum em complexidade **O(log a+b)**
- lmc: Calcula o menor múltiplo comum em complexidade **O(log a+b)**
- num_div: Calcula o números de divisores de N em complexidade **O(√n)**

```c++
using ll = long long;

ll gdc(ll a, ll b){
    ll rest;
    do{
        rest = a%b;
        a=b;
        b=rest;
    }while(rest!=0);
    return a;
}
ll lmc(ll a, ll b){
    return a/gdc(a, b)*b;
}
ll num_div(ll n){
    ll ans=0;
    for(ll i=1; i*i<=n; ++i)
        if(n%i == 0)
            ans+=( i==n/i ? 1 : 2);
    return ans;
}
```
<div style="page-break-after: always;"></div>