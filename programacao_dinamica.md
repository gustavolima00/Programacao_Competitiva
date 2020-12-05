<div  style="font-size: 34px">
Programação dinâmica
</div>

## Coin change

- **dp**: Considerando **N** como o número de moedas e **N** o valor do troco a dp retorna em complexidade <img src="https://latex.codecogs.com/gif.latexs?O(N%20%5Ctimes%20V)"> qual o número de maneiras possíveis de se entregar um troco de valor **V** com as **N** moedas disponíveis usando **x** moedas com **x>=0**

- **solve**: É possível resolver o problema com um limite de moedas, sendo **lmt[i]** o número de modedas **i** disponíveis em complexidade <img src="https://latex.codecogs.com/gif.latexs?O((N%20%5Ctimes%20V)%20%2B%20(N%20%5Ctimes%202%5EN))">

```c++
#define MAXN 5
#define MAXC 100001

int n;
ll mem[MAXC][MAXN];
bool visited[MAXC][MAXN];
int coins[MAXN];

ll dp(int v, int i=0){
	if(v<0) return 0;
	if(i==n){
		if(v == 0) return 1;
		else return 0;
	}
	auto &ans = mem[v][i];
	if(!visited[v][i]){
		visited[v][i] = true;
		ans  = dp(v, i+1);
		ans += dp(v-coins[i], i);
	}
	return ans;
}


int lmt[MAXN];
ll inline solve(int v){
	ll ans = 0;
	for(int msk=0; msk<(1<<n); ++msk){
		ll acc = 0;
		ll signal = 1;
		for(int i=0; i<n; ++i)
			if(msk&(1<<i)) acc += (lmt[i]+1)*coins[i], signal*=-1;
		ans = (ans + signal*dp(v-acc) + mod)%mod;
	}	
	return ans;
}
```
<div style="page-break-after: always;"></div>

## Knapsack

Problema clássico da mochila com pesos e valores dos itens, no código abaixo w é o peso que a pessoa pode carregar, wt é um array de pesos de tamanho n, val é um array de valores de tamanho n, e n é o tamanho dos arrays.

- A matriz do knapSack foi inicializada globalmente para poupar tempo de processamento inicializando os valores zerados
- Com esse código é necessário declarar os valores máximos que a matriz pode assumir como constantes
```c++
const int MAXN = 20000; // Número máximo de ítens
const int MAXW = 20000; // Número máximo de peso carregado 
int mt[MAXN+1][MAXW+1]; // Matríz global é iniciada com zeros

int knapSack(int w, int wt[], int val[], int n){
    for(int i=1; i<=n; i++){
        for(int j=1; j<=w; j++){
            if(wt[i-1]<=j)
                mt[i][j] = max(val[i-1]+mt[i-1][j-wt[i-1]], mt[i-1][j]);
            else
                mt[i][j] = mt[i-1][j];
        }
    }
    return mt[n][w];
}
```

<div style="page-break-after: always;"></div>


## LIS 

- Cáculculo da lis em complexidade **O(N log N)** 
- A recuperação do caminho pode ser feita pelo array pred começando do pred com o maior valor de lis

```c++


```

