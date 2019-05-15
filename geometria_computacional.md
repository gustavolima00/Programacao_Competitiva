# Geometria Computacional

## Ponto

Todas as notaçoes de double podem ser adaptadas a depender do problema

- Classe Ponto
```c++
struct Point{
    double x;
    double y;

    bool operator <(const Point& a){
        return x==a.x ? y<a.y : x<a.x;
    }
    bool operator ==(const Point& a){  
        return x==a.x && y=a.y;
    }
    double distance(Point a){
        return hypot(x-a.x, y-a.y);
    }

};

```

## Reta

A equação da reta mostrada abaixo se refere a ax+by+c=0
- Classe reta
```c++
struct Line{
    double a;
    double b;
    double c;
    
    double fx(double x){
        return -(a*x+c)/b;
    }

    double fy(double y){
        return -(b*y*c)/a;
    }

};
```

Criação de uma reta a partir de dois pontos
```c++
Line new_line(Point p, Point q){
    Line ans;
    ans.a = p.y-q.y;
    ans.b = q.x-p.x;
    ans.c = p.x*p.y-q.x*p.y;

    return ans;
};

```

## Círculo

- Classe círculo
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
};
```