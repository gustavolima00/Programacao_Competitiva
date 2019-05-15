# Estrutura de dados

## Fenwick Tree - SOMA de intervalo em O(log n)
Resolve queries do tipo RSQ de 1 a n (1-indexed) em O(log n).

A arvore é contruida inicialmente zerada e para relizar a inserção dos itens deve ser feita a soma ao indice i do valor x
```c++
template <typename T>
class BITree {
private:
    vector<T> ts;
    size_t N;

    int LSB(int n) { return n&(-n); }

    T RSQ(int i){
        T sum = 0;

        while(i>=1){
            sum+=ts[i];
            i-=LSB(i);
        }
        return sum;
    }
public:
    BITree(size_t n) : ts(n+1, 0), N(n) {};

    T RSQ(int i, int j){
        return RSQ(j) - RSQ(i-1);
    }

    void add(size_t i, const T& x){
        if(i==0) return;
        while(i<=N){
            ts[i]+=x;
            i+=LSB(i);
        }
    }
};
```



