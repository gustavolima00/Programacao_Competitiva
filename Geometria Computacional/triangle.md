# Triângulo 2D 

## Estrutura
### Descrição

- **(double)** perimeter: Calcula perímetro do triângulo
- **(double)** semiPerimeter: Calcula semi-perímetro do triângulo
- **(double)** area: Calcula área do triângulo
- **(double)** rInCircle: Calcula o raio do círculo inscrito no triângulo
- **(double)** inCircle: Retorna o círculo inscrito do triângulo
- **(double)** rCircumCircle: Calcula raio do círculo circunscrito no triângulo
- **(double)** circumCircle: Retorna o círculo circunscrito do triângulo
- **(double/int)** isInside: Retorna 0 caso o ponto esteja dentro do triângulo, 1 caso esteja na borda e 2 caso esteja fora 


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
    double rInCircle() {
        return area()/semiPerimeter();
    }
    circle inCircle() {
        return incircle(a, b, c);
    }
    double rCircumCircle() {
        return dist(a, b)*dist(b, c)*dist(c, a)/(4.0*area());
    }
    circle circumCircle(){
        return circumcircle(a, b, c);
    }
    bool isInside(point p){
        double u = cross(b-a, p-a)*cross(b-a, c-a);
        double v = cross(c-b, p-b)*cross(c-b, a-b);
        double w = cross(a-c, p-c)*cross(a-c, b-c);
        if (u>0.0 and v>0.0 and w>0.0) return 0;
        if (u<0.0 or v<0.0 or w<0.0) return 2;
        else return 1;
    }// 0 = inside/ 1 = border/ 2=outside
};
```