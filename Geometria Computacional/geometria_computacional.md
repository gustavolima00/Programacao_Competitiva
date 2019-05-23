# Geometria Computacional

## Reta

### Classe
A equação da reta mostrada abaixo se refere a ax+by+c=0
- Classe reta
```c++
struct Line{
    double a;
    double b;
    double c;

    Line() : a(1), b(1), c(1) {};
    //Apenas se a classe ponto estiver emplementada
    Line(Point p, Point q){
        a = p.y-q.y;
        b = q.x-p.x;
        c = p.x*p.y-q.x*p.y;
    }
    
    double fx(double x){
        return -(a*x+c)/b;
    }

    double fy(double y){
        return -(b*y*c)/a;
    }

};
```

### Ponto a partir de uma reta e distância ponto e reta

- A função a seguir retorna o ponto Q pertencente a reta R com base no ponto P passado.
- Para calcular a distancia entre o ponto e a reta , basta calcular a distância entre o ponto P e o ponto Q

<img src="img/ponto_reta.png" alt="img" width="" height="150">

- A formula é a seguinte:

<img src="img/ponto_q.png" alt="img" width="" height="">

```c++
//Para usar a função é preciso da classe Point
Point q_point(Point p, Line r){
    Point q;
    q.x = ( r.b*( r.b*p.x - r.a*p.x ) - r.a*r.c )/( r.a*r.a + r.b*r.b );
    q.y = ( r.a*( -r.b*p.x + r.a*p.y ) - r.b*r.c )/( r.a*r.a + r.b*r.b )

    return q;
}
```
## Segmento de reta

### Classe
```c++
const double EPS = 1e-9;

struct Segment{
    Point a;
    Point b;

    Segment(Point x, Point y) : a(x), b(y) {};
    // Verifica se um ponto se encontra no segmento de reta
    bool is_in(Point p){
        if(p==a or p==b) return true;

        auto xmin = min(a.x, b.x);
        auto xmax = max(a.x, b.x);
        auto ymin = min(a.y, b.y);
        auto ymax = max(a.y, b.y);

        if(p.x<xmin or p.x>xmax or p.y<ymin or p.y>ymax)
            return false;

        return fabs(( (p.y-a.y)*(b.x-a.x) ) - ( (p.x-a.x)*(b.y-a.y) ))<EPS;
    }


};
```
## Círculo

- Classe círculo

As funções definidas abaixo se referem ao argo, corda, setor e segmento do círculo respectivamente.
```c++
const double PI = acos(-1);

struct Circle{
    double r;
    double x;
    double y;
    
    double arc(double ang){
        return ang*r;
    }

    double chord(double ang){
        return 2*PI*sin(ang/2);
    }

    double sector(double ang){
        return ang*r*r/2;
    }

    double segment(double ang){
        double s = sector(ang);
        double c = chord(ang);
        return sqrt((s-r)*(s-r)*(s-c));
    }

};
```

### Polígono