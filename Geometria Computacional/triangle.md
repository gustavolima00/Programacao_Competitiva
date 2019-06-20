# Triângulo 2D 

## Estrutura
### Descrição

- **(double)** perimeter: Calcula perímetro do triângulo
- **(double)** semiPerimeter: Calcula semi-perímetro do triângulo
- **(double)** area: Calcula área do triângulo
- **(double)** r_incircle: Calcula o raio do círculo inscrito no triângulo
- **(double)** in_circle: Retorna o círculo inscrito do triângulo
- **(double)** r_circumcircle: Calcula raio do círculo circunscrito no triângulo
- **(double)** circum_circle: Retorna o círculo circunscrito do triângulo
- **(double/int)** is_inside: Retorna 0 caso o ponto esteja dentro do triângulo, 1 caso esteja na borda e 2 caso esteja fora 


```c++
struct triangle{
    point a, b, c;

    triangle() { a=b=c=point(); }
    triangle(point _a, point _b, point _c) : a(_a), b(_b), c(_c) {}

    double perimeter() { return dist(a, b) + dist(b, c) + dist(c, a); }
    double semiPerimeter() { return perimeter()/2.0; }
    double area(){
        double s = semiPerimeter(), ab = dist(a, b), bc = dist(b, c), ca = dist(c, a);
        return sqrt(s*(s-ab)*(s-bc)*(s-ca));
    }
    double r_incircle() {
        return area()/semiPerimeter();
    }
    circle in_circle() {
        return incircle(a, b, c);
    }
    double r_circumcircle() {
        return dist(a, b)*dist(b, c)*dist(c, a)/(4.0*area());
    }
    circle circum_circle(){
        return circumcircle(a, b, c);
    }
    int is_inside(point p){
        double u = cross(b-a, p-a)*cross(b-a, c-a);
        double v = cross(c-b, p-b)*cross(c-b, a-b);
        double w = cross(a-c, p-c)*cross(a-c, b-c);
        if (u>0.0 and v>0.0 and w>0.0) return 0;
        if (u<0.0 or v<0.0 or w<0.0) return 2;
        else return 1;
    }// 0 = inside/ 1 = border/ 2=outside
};
```
<div style="page-break-after: always;"></div>