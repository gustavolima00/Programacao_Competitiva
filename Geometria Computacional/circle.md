# Círculo 2D

## Estrutura

### Descrição

- **(double)** area: Calcula área do círculo
- **(double)** chord: Calcula o comprimento da corda com ângulo em radiandos
- **(double)** sector: Calula o comprimento do setor com ângulo radianos
- **(double)** intersects: Verifica se os círculos se interceptem
- **(double)** contains: Verifica se um ponto p está dentro do círculo
- **(double)** tangent_points: Retorna os 2 pontos que formam as retas com p tangentes ao círculo, (A função asin retorna nan se o ponto estiver dentro do círculo)
- **(double)** intersection_points: Retorna os 2 pontos de intersecção entre os círculos

```c++
const double PI = acos(-1);
const double EPS = 1e-9;
struct circle{
    point c;
    double r;

    circle() { c=point(); r=0; }
    circle(point _c, double _r) : c(_c), r(_r) {}

    double area() { return PI*r*r; }
    double chord (double rad){ return 2*r*sin(rad/2.0); } 
    double sector (double rad) { return 0.5*rad*area()/PI; }
    bool intersects(circle other){
        return dist(c, other.c) < r+other.r;
    }
    bool contains(point p) { return dist(c, p) <= r + EPS; }
    pair<point, point> tangent_points( point p ){
        double d1 = dist(p, c), theta = asin(r/d1);
        point p1 = rotate(c-p, -theta);
        point p2 = rotate(c-p, theta);
        p1 = p1*(sqrt(d1*d1-r*r)/d1)+p;
        p2 = p2*(sqrt(d1*d1-r*r)/d1)+p;
        return make_pair(p1, p2);
    }
    pair<point, point> intersection_points(circle other){
        assert(intersects(other));
        double d = dist(c, other.c);
        double u = acos((other.r*other.r + d*d - r*r)/(2*other.r*d));
        point dc = ((other.c - c).normalized()) * r;
        return make_pair(c + rotate(dc, u), c + rotate(dc, -u));
    }
};
```
<div style="page-break-after: always;"></div>

## Funções

### Descrição

- **(double)** inside_circle: Retorna 0 caso o ponto esteja dentro, 1 caso esteja na borda e 2 caso esteja fora do círculo
- **(double)** circumcircle: Círculo circunscrito no triângulo
- **(double)** incircle: Círculo inscrito do triângulo

```c++
int inside_circle(point p, circle c) {
    if (fabs(dist(p, c.c) - c.r)<EPS) return 1;
    else if(dist(p, c.c) < c.r) return 0;
    else return 2;
} // 0 = inside/ 1 = border/ 2 = outside
circle circumcircle(point a, point b, point c) {
    circle ans;
    point u = point((b-a).y, -(b-a).x);
    point v = point((c-a).y, -(c-a).x);
    point n = (c-b)*0.5;
    double t = cross(u, n)/cross(v, u);
    ans.c = ((a+c)*0.5) + (v*t);
    ans.r = dist(ans.c, a);
    return ans;
}
circle incircle( point p1, point p2, point p3){
    double m1 = dist(p2, p3);
    double m2 = dist(p1, p3);
    double m3 = dist(p1, p2);
    point c = (p1*m1+p2*m2+p3*m3)*(1/(m1+m2+m3));
    double s = 0.5*(m1+m2+m3);
    double r = sqrt(s*(s-m1)*(s-m2)*(s-m3))/s;
    return circle(c, r);
}

```
<div style="page-break-after: always;"></div>
