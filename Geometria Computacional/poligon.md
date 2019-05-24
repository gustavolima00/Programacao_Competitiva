# Polígono 2D

O polígono é representado por um vetor de pontos portanto não tem estrutura.

## Funções

```c++
using polygon = vector<point>;
const double PI = acos(-1);

// Área do polígono, com sinal
double signedArea(polygon &P){
    double result = 0.0;
    int n = P.size();
    for(int i=0; i<n; i++){
        result+=cross(P[i], P[(i+1)%n]);
    }
    return result/2.0;
}

// Área positiva do polígono
double area(polygon &P){
    return fabs(signedArea(P));
}

// Índice do ponto mais a esquerda do vetor de pontos 
int leftmostIndex(vector<point>& P){
    int ans = 0;
    for(int i=1; i<int(P.size()); i++){
        if (P[i]<P[ans]) ans = i;
    }
    return ans;
}

// Retorna o polígono sem crusamento de pontos (Eu acho kkk)
polygon make_polygon(vector<point> P){
    if(signedArea(P)<0.0) 
        reverse(P.begin(), P.end());

    int li = leftmostIndex(P);
    rotate(P.begin(), P.begin()+li, P.end());
    return P;
}

// Perímetro do polígono
double perimeter(polygon &P){
    double result = 0.0;
    int n = P.size();
    for(int i=0; i<n; i++) result+=dist(P[i], P[(i+1)%n]);

    return result; 
}

// Verifica se polígono é convexo
bool isConvex(polygon& P){
    int n = P.size();
    if( n<3 ) return false;
    bool left = ccw(P[0], P[1], P[2]);
    for(int i=1; i<n; i++){
        if(ccw(P[i], P[(i+1)%n], P[(i+2)%n]) != left)
            return false;
    }
    return true;
}

// Verifica se um ponto está dentro do polígono
bool inPolytgon(polygon &P, point p){
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

// Polígono a direita que é formado pelo corte do polígono P pela reta formada pelos pontos a e b

polygon cutPolygon(polygon &P, point a, point b){
    vector<point> R;
    double left1, left2;
    int n = P.size();
    for(int i=0; i<n; i++){
        left1 = cross(b-a, P[i]-a);
        left2 = cross(b-a, P[(i+1)%n]-a);
        if (left1 > -EPS) R.push_back(P[i]);
        if (left1 * left2 < -EPS)
            R.push_back(lineIntersectSeg(P[i], P[(i+1)%n], a, b));
    }
    return make_polygon(R);
}
```