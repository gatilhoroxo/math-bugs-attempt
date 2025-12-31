# Cônicas e Superfícies Quádricas

## 🎯 Meta de Aprendizado

Ao completar este tópico, você será capaz de:
- Reconhecer e classificar cônicas (elipse, parábola, hipérbole, circunferência)
- Converter entre formas de equações de cônicas
- Entender superfícies quádricas em 3D (esferas, elipsoides, paraboloides)
- Aplicar cônicas em trajetórias balísticas, órbitas e gráficos

---

## ⏱️ Tempo Estimado

- **Leitura ativa:** 55-75 min
- **Exercícios relacionados:** 45-60 min (`k2-exercicios/e3-conicas-superficies-exercicios.md`)
- **Implementação:** 70-90 min (`k3-implementacao/i3-conicas-superficies.md`)
- **Total:** ~2h50-3h45

---

## 📋 Pré-requisitos

- [x] Pontos, retas e planos
- [x] Distâncias
- [x] Produto escalar
- [ ] Equações do 2º grau

---

## 🗺️ Mapa Mental

```
CÔNICAS E SUPERFÍCIES
├── 1. Cônicas (2D)
│   ├── Definição geométrica (foco e diretriz)
│   ├── Tipos
│   │   ├── Circunferência (e=0)
│   │   ├── Elipse (0<e<1)
│   │   ├── Parábola (e=1)
│   │   └── Hipérbole (e>1)
│   ├── Equações canônicas
│   └── Equação geral: Ax²+Bxy+Cy²+Dx+Ey+F=0
│
├── 2. Aplicações de Cônicas
│   ├── Trajetórias balísticas (parábola)
│   ├── Órbitas planetárias (elipse)
│   ├── Navegação LORAN (hipérbole)
│   └── Antenas parabólicas (parábola)
│
└── 3. Superfícies Quádricas (3D)
    ├── Esfera
    ├── Elipsoide
    ├── Paraboloide (elíptico e hiperbólico)
    ├── Hiperboloide (1 e 2 folhas)
    └── Cone
```

---

## 📖 Conteúdo

### 1. O que são Cônicas?

**Definição geométrica:** Cônicas são curvas obtidas pela **interseção de um cone duplo com um plano**.

```
    /\      Plano vertical → Hipérbole
   /  \     Plano inclinado → Elipse/Circunferência
  /    \    Plano paralelo à geratriz → Parábola
 /------\
 \      /
  \    /
   \  /
    \/
```

**Definição analítica:** Lugar geométrico de pontos que satisfazem uma equação do 2º grau.

**Por que importam para CS?**

- **Física de Jogos:** Trajetórias de projéteis são parábolas
- **Navegação:** GPS usa hipérboles (diferença de distâncias)
- **Astronomia:** Órbitas são elipses (Lei de Kepler)
- **Gráficos:** Ray tracing com esferas, reflexões parabólicas
- **Antenas:** Foco de parábola concentra sinais

---

### 2. Circunferência

#### 2.1 Definição

Conjunto de pontos equidistantes de um centro $C$.

**Equação canônica (centro na origem):**
$$x^2 + y^2 = r^2$$

**Equação geral (centro em $(h, k)$):**
$$(x - h)^2 + (y - k)^2 = r^2$$

Onde:
- $(h, k)$ = centro
- $r$ = raio

**Expandindo:**
$$x^2 - 2hx + h^2 + y^2 - 2ky + k^2 = r^2$$
$$x^2 + y^2 - 2hx - 2ky + (h^2 + k^2 - r^2) = 0$$

#### 2.2 Aplicações

**Colisão circular (jogos):**
```cpp
bool circle_collision(Point c1, float r1, Point c2, float r2) {
    float dx = c2.x - c1.x;
    float dy = c2.y - c1.y;
    float distance_sq = dx*dx + dy*dy;
    float radius_sum = r1 + r2;
    return distance_sq < radius_sum * radius_sum;
}
```

**Detecção de alcance (IA):**
```python
def in_range(unit_pos, target_pos, attack_range):
    """Verifica se alvo está dentro do alcance"""
    distance = sqrt((target_pos.x - unit_pos.x)**2 + 
                    (target_pos.y - unit_pos.y)**2)
    return distance <= attack_range
```

---

### 3. Elipse

#### 3.1 Definição Geométrica

Lugar geométrico dos pontos cuja **soma das distâncias a dois focos** é constante.

$$d(P, F_1) + d(P, F_2) = 2a$$

```
      F1        P        F2
      •--------/|\-------•
               / | \
    d1 ____/    |    \___ d2
                |
         d1 + d2 = 2a (constante)
```

**Elementos:**
- $F_1, F_2$: focos
- $a$: semi-eixo maior
- $b$: semi-eixo menor
- $c$: distância centro-foco, onde $c^2 = a^2 - b^2$
- Excentricidade: $e = c/a$ (quanto mais próximo de 0, mais circular)

#### 3.2 Equações

**Canônica (eixos paralelos aos eixos coordenados):**

Eixo maior horizontal:
$$\frac{x^2}{a^2} + \frac{y^2}{b^2} = 1 \quad (a > b)$$

Eixo maior vertical:
$$\frac{x^2}{b^2} + \frac{y^2}{a^2} = 1 \quad (a > b)$$

**Centro em $(h, k)$:**
$$\frac{(x-h)^2}{a^2} + \frac{(y-k)^2}{b^2} = 1$$

#### 3.3 Aplicações

**Órbitas planetárias (Lei de Kepler):**
```python
# Órbita da Terra ao redor do Sol (simplificado)
a = 1.0  # 1 AU (unidade astronômica)
e = 0.0167  # Excentricidade da órbita terrestre
b = a * sqrt(1 - e**2)

# Posição em função do ângulo θ (anomalia verdadeira)
def orbit_position(theta):
    r = a * (1 - e**2) / (1 + e * cos(theta))
    x = r * cos(theta)
    y = r * sin(theta)
    return Point(x, y)
```

**Iluminação elíptica:**
```cpp
// Propriedade refletiva: luz de F1 reflete para F2
// Usado em litotripsia (quebrar pedras nos rins)
```

---

### 4. Parábola

#### 4.1 Definição Geométrica

Lugar geométrico dos pontos **equidistantes** de um foco $F$ e uma diretriz $d$.

$$d(P, F) = d(P, d)$$

```
        P
       /|\
      / | \
     /  |  \
    F   |   |  (diretriz)
        |   |
```

**Elementos:**
- Foco $F$
- Diretriz (reta)
- Vértice $V$ (ponto médio entre foco e diretriz)
- Parâmetro $p$ = distância vértice-foco

#### 4.2 Equações

**Canônica (vértice na origem, abre para cima):**
$$x^2 = 4py$$

**Outras orientações:**
- Abre para baixo: $x^2 = -4py$
- Abre para direita: $y^2 = 4px$
- Abre para esquerda: $y^2 = -4px$

**Vértice em $(h, k)$:**
$$(x - h)^2 = 4p(y - k)$$

**Forma quadrática:**
$$y = ax^2 + bx + c$$

Onde vértice está em $x_v = -b/(2a)$, $y_v = c - b^2/(4a)$.

#### 4.3 Aplicações

**Trajetória balística (física de jogos):**
```python
# Lançamento de projétil sob gravidade
def projectile_trajectory(v0, angle, g=9.8):
    """Retorna função y(x) da trajetória"""
    v0x = v0 * cos(angle)
    v0y = v0 * sin(angle)
    
    # y = x*tan(θ) - (g*x²)/(2*v0²*cos²(θ))
    def y(x):
        return x * tan(angle) - (g * x**2) / (2 * v0x**2)
    
    return y

# Uso: calcular onde projétil cai
traj = projectile_trajectory(v0=20, angle=radians(45))
# traj(x) retorna altura em cada posição x
```

**Antena parabólica:**
```
Propriedade refletiva: ondas paralelas ao eixo
refletem para o foco (receptor)

  |||  ondas
  |||
  |||
  \|/
   V     ← reflector parabólico
   •F    ← receptor no foco
```

**Faróis automotivos:** Lâmpada no foco, luz reflete paralela.

---

### 5. Hipérbole

#### 5.1 Definição Geométrica

Lugar geométrico dos pontos cuja **diferença das distâncias a dois focos** é constante.

$$|d(P, F_1) - d(P, F_2)| = 2a$$

```
    F1         F2
    •           •
     \\       //     Ramo 1
      \\     //
       \\   //
        \\ //
         \/   Vértices
         /\
        // \\
       //   \\      Ramo 2
      //     \\
     //       \\
```

**Elementos:**
- $F_1, F_2$: focos
- $a$: semi-eixo transverso (distância centro-vértice)
- $b$: semi-eixo conjugado
- $c$: distância centro-foco, onde $c^2 = a^2 + b^2$ (note o +)
- Excentricidade: $e = c/a > 1$
- Assíntotas: $y = \pm (b/a)x$

#### 5.2 Equações

**Canônica (eixo transverso horizontal):**
$$\frac{x^2}{a^2} - \frac{y^2}{b^2} = 1$$

**Eixo transverso vertical:**
$$\frac{y^2}{a^2} - \frac{x^2}{b^2} = 1$$

**Centro em $(h, k)$:**
$$\frac{(x-h)^2}{a^2} - \frac{(y-k)^2}{b^2} = 1$$

#### 5.3 Aplicações

**Navegação LORAN (Long Range Navigation):**
```python
# Sistema usa diferença de tempo de chegada de sinais
# de duas estações (focos) para determinar posição

# Se sinal de A chega 0.001s antes de B:
time_diff = 0.001  # segundos
c = 299792  # velocidade da luz (km/s)
distance_diff = c * time_diff  # = 2a da hipérbole

# Embarcação está em uma hipérbole com focos em A e B
```

**Órbitas hiperbólicas (cometas):**
```python
# Cometas com energia > 0 têm órbitas hiperbólicas
# (passam uma vez e vão embora)
```

---

### 6. Equação Geral das Cônicas

**Forma geral de 2º grau:**
$$Ax^2 + Bxy + Cy^2 + Dx + Ey + F = 0$$

**Classificação pelo discriminante $\Delta = B^2 - 4AC$:**

| $\Delta$ | Tipo |
|----------|------|
| $< 0$ | Elipse (se $A = C$ e $B = 0$: circunferência) |
| $= 0$ | Parábola |
| $> 0$ | Hipérbole |

**Termo $Bxy$:** Indica rotação dos eixos.

---

### 7. Superfícies Quádricas (3D)

#### 7.1 Esfera

$$x^2 + y^2 + z^2 = r^2$$

**Centro em $(h, k, l)$:**
$$(x-h)^2 + (y-k)^2 + (z-l)^2 = r^2$$

**Ray-sphere intersection:**
```cpp
bool ray_sphere_intersection(Ray ray, Sphere sphere, float& t) {
    Vector oc = ray.origin - sphere.center;
    float a = ray.direction.dot(ray.direction);
    float b = 2.0 * oc.dot(ray.direction);
    float c = oc.dot(oc) - sphere.radius * sphere.radius;
    
    float discriminant = b*b - 4*a*c;
    if (discriminant < 0) return false;
    
    t = (-b - sqrt(discriminant)) / (2*a);
    return t > 0;
}
```

---

#### 7.2 Elipsoide

$$\frac{x^2}{a^2} + \frac{y^2}{b^2} + \frac{z^2}{c^2} = 1$$

**Aplicação:** Terra é aproximadamente um elipsoide (achatada nos polos).

```python
# WGS84 (sistema de coordenadas GPS)
a = 6378.137  # km (raio equatorial)
b = 6356.752  # km (raio polar)

# Fator de achatamento
f = (a - b) / a  # ≈ 1/298.257
```

---

#### 7.3 Paraboloide Elíptico

$$z = \frac{x^2}{a^2} + \frac{y^2}{b^2}$$

**Forma:** Tigela/bacia.

**Aplicação:** Telescópios refletores, antenas de rádio.

---

#### 7.4 Paraboloide Hiperbólico (Sela)

$$z = \frac{x^2}{a^2} - \frac{y^2}{b^2}$$

**Forma:** Ponto de sela (sobe em uma direção, desce em outra).

**Aplicação:** Arquitetura (estruturas Pringle), otimização (pontos de sela em gradiente).

```python
# Visualização em malha 3D
def hyperbolic_paraboloid(x, y, a=1, b=1):
    return (x**2 / a**2) - (y**2 / b**2)
```

---

#### 7.5 Hiperboloide de Uma Folha

$$\frac{x^2}{a^2} + \frac{y^2}{b^2} - \frac{z^2}{c^2} = 1$$

**Forma:** Torre de resfriamento nuclear.

**Propriedade:** Superfície regrada (pode ser construída com retas).

---

#### 7.6 Hiperboloide de Duas Folhas

$$\frac{z^2}{c^2} - \frac{x^2}{a^2} - \frac{y^2}{b^2} = 1$$

**Forma:** Dois "funis" separados.

---

#### 7.7 Cone

$$\frac{x^2}{a^2} + \frac{y^2}{b^2} = \frac{z^2}{c^2}$$

**Aplicação:** Ray tracing de cones, campo de visão (FOV).

```cpp
// Cone de visão de uma luz spot
bool in_spotlight_cone(Point p, Point light_pos, 
                        Vector light_dir, float cone_angle) {
    Vector to_point = (p - light_pos).normalize();
    float cos_angle = to_point.dot(light_dir);
    return acos(cos_angle) < cone_angle;
}
```

---

### 8. Parametrizações

Cônicas e superfícies podem ser parametrizadas (útil para renderização):

**Circunferência:**
```python
x = r * cos(t)
y = r * sin(t)
# t ∈ [0, 2π]
```

**Elipse:**
```python
x = a * cos(t)
y = b * sin(t)
```

**Parábola:**
```python
x = t
y = a * t**2
# t ∈ ℝ
```

**Esfera:**
```python
x = r * sin(θ) * cos(φ)
y = r * sin(θ) * sin(φ)
z = r * cos(θ)
# θ ∈ [0, π], φ ∈ [0, 2π]
```

---

### 9. Interseção Ray-Quadric

**Método geral:**
1. Substituir $\vec{r}(t) = \vec{o} + t\vec{d}$ na equação da quádrica
2. Obter equação do 2º grau em $t$: $at^2 + bt + c = 0$
3. Resolver usando fórmula de Bhaskara
4. Verificar se $t > 0$ (interseção à frente)

**Exemplo - Ray-Sphere:**
```python
def ray_sphere_intersect(ray_origin, ray_dir, sphere_center, radius):
    oc = ray_origin - sphere_center
    
    a = ray_dir.dot(ray_dir)
    b = 2 * oc.dot(ray_dir)
    c = oc.dot(oc) - radius**2
    
    discriminant = b**2 - 4*a*c
    
    if discriminant < 0:
        return None  # Sem interseção
    
    t1 = (-b - sqrt(discriminant)) / (2*a)
    t2 = (-b + sqrt(discriminant)) / (2*a)
    
    # Retorna a interseção mais próxima (à frente)
    if t1 > 0:
        return ray_origin + t1 * ray_dir
    elif t2 > 0:
        return ray_origin + t2 * ray_dir
    else:
        return None
```

---

## 🔗 Conexões

### Com Física
- Trajetórias sob gravidade (parábola)
- Órbitas (elipse, hipérbole)
- Reflexão (parábola, elipse)

### Com Álgebra Linear
- Diagonalização de matrizes para rotação de cônicas
- Formas quadráticas: $x^TAx$

### Com Cálculo
- Comprimento de arco de elipse (integrais elípticas)
- Otimização: mínima distância a uma cônica

---

## 🎯 Checklist de Domínio

- [ ] Identificar tipo de cônica pelo discriminante
- [ ] Escrever equação de circunferência/esfera dado centro e raio
- [ ] Calcular focos de elipse/hipérbole
- [ ] Modelar trajetória de projétil como parábola
- [ ] Implementar ray-sphere intersection
- [ ] Parametrizar cônicas para renderização

---

## 📚 Próximos Passos

1. **Exercícios:** `k2-exercicios/e3-conicas-superficies-exercicios.md`
2. **Implementação:** `k3-implementacao/i3-conicas-superficies.md`
3. **Próximo tópico:** `t4-coordenadas-transformacoes.md`

---

**Última atualização:** 30 de dezembro de 2025  
**Tempo de leitura:** ~65 minutos
