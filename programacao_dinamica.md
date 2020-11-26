<div  style="font-size: 34px">
Programação dinâmica
</div>

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
