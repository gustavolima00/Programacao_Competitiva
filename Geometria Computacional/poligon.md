# Polígono 2D

O polígono é representado por um vetor de pontos portanto não tem estrutura.

## Funções
### Descrição
- **(double/int)** signed_area: Retorna área do polígono (Pode dar valor negativo)
- **(double/int)** area: Retorna área positiva do polígono
- **(double/int)** left_index: Índice do ponto mais a esquerda
- **(double/int)** make_polygon: Coloca o índice do ponto mais a esquerda como 0 (Necessário para as funções que chamam polygon)
- **(double)** perimeter: Calcula o perímetro do polígono
- **(double/int)** is_convex: Verifica se polígono é convexo
- **(double)** in_polytgon: Verifica se um ponto está no polígono
- **(double)** cut_polygon: Polígono a direita que é formado pelo corte do polígono P pela reta formada pelos pontos a e b
```c++
using polygon = vector<point>;
const double PI = acos(-1);

double signed_area(polygon &P){
    double result = 0.0;
    int n = P.size();
    for(int i=0; i<n; i++){
        result+=cross(P[i], P[(i+1)%n]);
    }
    return result/2.0;
}
double area(polygon &P){
    return fabs(signed_area(P));
}
int left_index(vector<point>& P){
    int ans = 0;
    for(int i=1; i<int(P.size()); i++){
        if (P[i]<P[ans]) ans = i;
    }
    return ans;
}
polygon make_polygon(vector<point> P){
    if(signed_area(P)<0.0) 
        reverse(P.begin(), P.end());

    int li = left_index(P);
    rotate(P.begin(), P.begin()+li, P.end());
    return P;
}
double perimeter(polygon &P){
    double result = 0.0;
    int n = P.size();
    for(int i=0; i<n; i++) result+=dist(P[i], P[(i+1)%n]);
    return result; 
}
bool is_convex(polygon& P){
    int n = P.size();
    if( n<3 ) return false;
    bool left = ccw(P[0], P[1], P[2]);
    for(int i=1; i<n; i++){
        if(ccw(P[i], P[(i+1)%n], P[(i+2)%n]) != left)
            return false;
    }
    return true;
}
bool in_polytgon(polygon &P, point p){
    if (P.size() == 0u) return false;
    double sum = 0.0;
    int n = P.size();
    for(int i=0; i<n; i++){
        if(P[i] == p || between(P[i], p, P[(i+1)%n]))
            return true;
        if(ccw(p, P[i], P[(i+1)%n])) 
            sum+=angle(P[i], p, P[(i+1)%n]);
        else 
            sum-=angle(P[i], p, P[(i+1)%n]);
    }
    return fabs(fabs(sum)-2*PI) < EPS; 
}
polygon cut_polygon(polygon &P, point a, point b){
    vector<point> R;
    double left1, left2;
    int n = P.size();
    for(int i=0; i<n; i++){
        left1 = cross(b-a, P[i]-a);
        left2 = cross(b-a, P[(i+1)%n]-a);
        if (left1 > -EPS) R.push_back(P[i]);
        if (left1 * left2 < -EPS)
            R.push_back(line_intersect(P[i], P[(i+1)%n], a, b));
    }
    return make_polygon(R);
}
```

<div style="page-break-after: always;"></div>

## Convex Hull
Dado um conjunto de pontos retorna o polígono que contém todos os pontos em O(n log n). Caso presice consederar os pontos no meio de uma aresta trocar ccw para >=0. CUIDADO: Se todos os pontos forem colineares, vai dar RTE.


```c++
point pivot(0, 0);

bool angle_cmp(point a, point b){
    if(collinear(pivot, a, b))
        return inner(pivot-a, pivot-a) < inner(pivot-b, pivot-b);
    return cross(a-pivot, b-pivot) >=0;
}

polygon convex_hull(vector<point> P){
    int i, j, n = P.size();
    if( n<=2 ) return P;
    int P0 = left_index(P);
    swap(P[0], P[P0]);
    pivot = P[0];
    sort(++P.begin(), P.end(), angle_cmp);
    vector<point> S;
    S.push_back(P[n-1]);
    S.push_back(P[0]);
    S.push_back(P[1]);
    for(i=2; i<n;){
        j=int(S.size()-1);
        if(ccw(S[j-1], S[j], P[i]))
            S.push_back(P[i++]);
        else S.pop_back();
    }
    reverse(S.begin(), S.end());
    S.pop_back();
    reverse(S.begin(), S.end());
    return S;
}
```

<div style="page-break-after: always;"></div>


## Monotone chain

- Com esse algorítimo é possível conseguir o polígono que contém todos os pontos em O(n log n) com todos os pontos consistindo como inteiros e não ponto flutuante. **Caso presice consederar os pontos no meio de uma aresta trocar ccw para >=0** 
- Nessa rotina é preciso usar a struct point como **long long.**

```c++
using polygon = vector<point>;

polygon convex_hull(const vector<point> points){
    vector<point> P(points);
    sort(P.begin(), P.end());
    vector<point> lower, upper;
    for(const auto& p: P){
        int n = int(lower.size());
        while(n>=2 and ccw(lower[n-2], lower[n-1], p)){
            lower.pop_back();
            n = int(lower.size());
        }
        lower.push_back(p);
    }
    reverse(P.begin(), P.end());
    for(const auto& p: P){
        int n = int(upper.size());
        while(n>=2 and ccw(upper[n-2], upper[n-1], p)){
            upper.pop_back();
            n = int(upper.size());
        }
        upper.push_back(p);
    }
    lower.pop_back();
    lower.insert(lower.end(), upper.begin(), upper.end()-1);
    return lower;
}
```

<div style="page-break-after: always;"></div>