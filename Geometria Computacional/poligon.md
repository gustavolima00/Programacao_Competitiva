# Polígono 2D

O polígono é representado por um vetor de pontos

```c++
using polygon = vector<point>;

double signedArea(polygon &P){
    double result = 0.0;
    int n = P.size();
    for(int i=0; i<n; i++){
        result+=cross(P[i], P[(i+1)%n]);
    }
    return result/2.0;
}

```