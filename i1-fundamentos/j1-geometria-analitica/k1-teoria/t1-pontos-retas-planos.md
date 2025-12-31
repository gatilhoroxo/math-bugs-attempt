# Pontos, Retas e Planos

## 🎯 Meta de Aprendizado

Ao completar este tópico, você será capaz de:
- Representar pontos, retas e planos em sistemas de coordenadas 2D e 3D
- Converter entre diferentes formas de equações (paramétrica, vetorial, geral)
- Determinar posições relativas (interseções, paralelismo, perpendicularidade)
- Aplicar esses conceitos em navegação, colisões e gráficos computacionais

---

## ⏱️ Tempo Estimado

- **Leitura ativa:** 50-70 min
- **Exercícios relacionados:** 40-60 min (`k2-exercicios/e1-pontos-retas-planos-exercicios.md`)
- **Implementação:** 60-90 min (`k3-implementacao/i1-pontos-retas-planos.md`)
- **Total:** ~2h30-4h

---

## 📋 Pré-requisitos

- [ ] Vetores (direção e magnitude) - `j1-algebra-linear/k1-teoria/t1-vetores-espacos.md`
- [ ] Produto escalar e vetorial
- [ ] Sistema de coordenadas cartesianas
- [ ] Equações lineares básicas

---

## 🗺️ Mapa Mental

```
PONTOS, RETAS E PLANOS
├── 1. Pontos no Espaço
│   ├── Coordenadas cartesianas (x, y, z)
│   ├── Distância entre pontos
│   └── Ponto médio
│
├── 2. Retas
│   ├── Representações
│   │   ├── Equação vetorial: P = P₀ + tv
│   │   ├── Equações paramétricas
│   │   ├── Equação geral (2D): ax + by = c
│   │   └── Equações simétricas
│   ├── Posições relativas
│   │   ├── Interseção
│   │   ├── Paralelas
│   │   └── Reversas (3D)
│   └── Aplicações
│       ├── Ray casting (gráficos)
│       ├── Trajetórias (navegação)
│       └── Detecção de colisões
│
└── 3. Planos (3D)
    ├── Representações
    │   ├── Equação vetorial: P = P₀ + su + tv
    │   ├── Equação geral: ax + by + cz = d
    │   └── Vetor normal
    ├── Posições relativas
    │   ├── Interseção reta-plano
    │   ├── Interseção plano-plano
    │   └── Paralelismo e perpendicularidade
    └── Aplicações
        ├── Renderização 3D (frustum culling)
        ├── Física (superfícies de contato)
        └── Navegação marítima (horizonte)
```

---

## 📖 Conteúdo

### 1. Por que Geometria Analítica?

Geometria Analítica é a **ponte entre álgebra e geometria**. René Descartes revolucionou a matemática ao perceber que poderia representar formas geométricas com equações algébricas.

**Por que isso importa para CS?**

- **Navegação:** GPS calcula posições usando coordenadas
- **Gráficos 3D:** Toda cena é formada por pontos, retas e planos
- **Física de Jogos:** Colisões são interseções geométricas
- **Robótica:** Planejamento de trajetórias é geometria pura
- **Mapas:** Sistemas de coordenadas (latitude, longitude)

> 💡 **Insight:** Se você trabalha com posições no espaço (2D ou 3D), está fazendo geometria analítica.

**Exemplo em código:**

```python
# Navegação - Calcular rota entre dois pontos
origin = Point(lat=-23.550, lon=-46.633)  # São Paulo
destination = Point(lat=-22.906, lon=-43.172)  # Rio de Janeiro

# A rota mais curta é uma "reta" (ortodromia em esfera)
route = calculate_great_circle(origin, destination)
```

```cpp
// Gráficos - Ray casting para renderização
Ray ray(camera_position, direction);
Plane ground(normal=Vector3(0,1,0), point=Vector3(0,0,0));

if (intersection = ray.intersect(ground)) {
    render_pixel(intersection.point);
}
```

---

### 2. Pontos no Espaço

#### 2.1 Representação

Um **ponto** é uma localização no espaço, definida por coordenadas:

- **2D:** $P = (x, y)$ 
- **3D:** $P = (x, y, z)$

**Exemplos concretos:**
- GPS: $(-23.550, -46.633)$ (latitude, longitude de São Paulo)
- Jogos 2D: $(320, 240)$ (centro de tela 640×480)
- Modelagem 3D: $(1.5, 2.3, -0.8)$ (vértice de um objeto)

#### 2.2 Distância Entre Pontos

A distância euclidiana entre dois pontos $P_1 = (x_1, y_1, z_1)$ e $P_2 = (x_2, y_2, z_2)$ é:

$$d(P_1, P_2) = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2 + (z_2 - z_1)^2}$$

**Em 2D:**
$$d(P_1, P_2) = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$$

**Aplicação - Detecção de colisão circular:**
```python
def circles_collide(c1, r1, c2, r2):
    """Dois círculos colidem se distância < soma dos raios"""
    distance = sqrt((c2.x - c1.x)**2 + (c2.y - c1.y)**2)
    return distance < (r1 + r2)
```

#### 2.3 Ponto Médio

O ponto médio $M$ entre $P_1$ e $P_2$ é:

$$M = \left(\frac{x_1 + x_2}{2}, \frac{y_1 + y_2}{2}, \frac{z_1 + z_2}{2}\right)$$

**Aplicação - Interpolação linear (lerp):**
```cpp
// Animação: mover objeto de A para B
Point lerp(Point A, Point B, float t) {
    // t=0 → A, t=0.5 → meio, t=1 → B
    return A + t * (B - A);
}
```

---

### 3. Retas

#### 3.1 Equação Vetorial (Forma Mais Intuitiva)

Uma reta é definida por um **ponto** $P_0$ e uma **direção** $\vec{v}$:

$$\vec{r}(t) = P_0 + t\vec{v}, \quad t \in \mathbb{R}$$

- $P_0 = (x_0, y_0, z_0)$: ponto conhecido na reta
- $\vec{v} = (a, b, c)$: vetor diretor (direção da reta)
- $t$: parâmetro real (varia de $-\infty$ a $+\infty$)

**Interpretação física:** "Comece em $P_0$ e caminhe na direção $\vec{v}$"

**Exemplo - Trajetória de projétil (simplificado):**
```python
# Projétil começa em origem, se move na direção (10, 5)
P0 = Point(0, 0)
v = Vector(10, 5)  # 10 m/s horizontal, 5 m/s vertical

# Posição em t=2 segundos:
position = P0 + 2 * v  # = (20, 10)
```

#### 3.2 Equações Paramétricas

Expandindo a equação vetorial:

$$\begin{cases}
x = x_0 + ta \\
y = y_0 + tb \\
z = z_0 + tc
\end{cases}$$

**Vantagem:** Fácil calcular pontos na reta para qualquer $t$.

#### 3.3 Equação Geral (2D)

Em 2D, uma reta pode ser escrita como:

$$ax + by = c$$

Ou na forma **implícita**: $ax + by + c = 0$

**Relacionamento com vetor normal:**
- O vetor $\vec{n} = (a, b)$ é **perpendicular** à reta
- Útil para calcular distância ponto-reta

**Conversão:** Se você tem $P_0 = (x_0, y_0)$ e diretor $\vec{v} = (v_x, v_y)$:
- Vetor normal: $\vec{n} = (-v_y, v_x)$ (rotação de 90°)
- Equação: $-v_y(x - x_0) + v_x(y - y_0) = 0$

#### 3.4 Equações Simétricas (3D)

Se $a, b, c \neq 0$, eliminamos $t$ das paramétricas:

$$\frac{x - x_0}{a} = \frac{y - y_0}{b} = \frac{z - z_0}{c}$$

**Quando usar:** Verificar se um ponto pertence à reta.

---

### 4. Interseção de Retas (2D)

Dadas duas retas:
- $r_1: P_1 + t\vec{v_1}$
- $r_2: P_2 + s\vec{v_2}$

**Casos possíveis:**

1. **Concorrentes** (se cruzam em 1 ponto)
2. **Paralelas** ($\vec{v_1} \parallel \vec{v_2}$, nunca se cruzam)
3. **Coincidentes** (mesma reta)

**Método de resolução:**

Igualamos: $P_1 + t\vec{v_1} = P_2 + s\vec{v_2}$

Sistema 2×2:
$$\begin{cases}
x_1 + tv_{1x} = x_2 + sv_{2x} \\
y_1 + tv_{1y} = y_2 + sv_{2y}
\end{cases}$$

**Implementação prática:**
```python
def line_intersection_2d(P1, v1, P2, v2):
    """Retorna ponto de interseção ou None se paralelas"""
    # Resolver sistema linear 2x2
    det = v1.x * v2.y - v1.y * v2.x
    
    if abs(det) < epsilon:  # Paralelas
        return None
    
    t = ((P2.x - P1.x) * v2.y - (P2.y - P1.y) * v2.x) / det
    return P1 + t * v1
```

**Aplicação - Colisão de segmentos:**
```cpp
// Jogos 2D: verificar se bala (segmento) acerta parede
bool segment_intersection(Segment s1, Segment s2) {
    Point inter = line_intersection(s1.start, s1.direction, 
                                     s2.start, s2.direction);
    if (!inter) return false;
    
    // Verificar se interseção está dentro dos segmentos
    return in_range(inter, s1) && in_range(inter, s2);
}
```

---

### 5. Planos (3D)

#### 5.1 Equação Vetorial

Um plano é definido por um **ponto** $P_0$ e dois **vetores diretores** $\vec{u}$ e $\vec{v}$ (não paralelos):

$$\vec{r}(s, t) = P_0 + s\vec{u} + t\vec{v}, \quad s, t \in \mathbb{R}$$

**Interpretação:** "Comece em $P_0$ e caminhe em duas direções independentes"

#### 5.2 Equação Geral (Mais Usado)

$$ax + by + cz = d$$

Ou forma **implícita**: $ax + by + cz + d = 0$

**Vetor normal:** $\vec{n} = (a, b, c)$ é perpendicular ao plano.

**Como obter de dois vetores diretores:**
$$\vec{n} = \vec{u} \times \vec{v}$$

O produto vetorial dá o vetor perpendicular aos dois!

**Exemplo - Plano do chão em um jogo 3D:**
```cpp
// Chão plano em y=0 (plano xz)
// Vetor normal aponta para cima: n = (0, 1, 0)
Plane ground(normal=Vector3(0, 1, 0), d=0);
// Equação: 0x + 1y + 0z = 0  →  y = 0
```

#### 5.3 Distância Ponto-Plano

Dado plano $ax + by + cz = d$ e ponto $P = (x_0, y_0, z_0)$:

$$\text{distância} = \frac{|ax_0 + by_0 + cz_0 - d|}{\sqrt{a^2 + b^2 + c^2}}$$

**Aplicação - Frustum culling (não renderizar objetos fora da câmera):**
```python
def is_visible(object_position, frustum_planes):
    """Objeto está visível se dentro de todos os 6 planos do frustum"""
    for plane in frustum_planes:
        distance = plane.distance_to(object_position)
        if distance < -object_radius:
            return False  # Fora do frustum
    return True
```

---

### 6. Interseção Reta-Plano

Dada reta $\vec{r}(t) = P_0 + t\vec{v}$ e plano $ax + by + cz = d$:

**Método:**
1. Substituir equações paramétricas da reta no plano
2. Resolver para $t$
3. Calcular ponto: $P = P_0 + t\vec{v}$

**Fórmula direta:**

$$t = \frac{d - (aP_{0x} + bP_{0y} + cP_{0z})}{a v_x + b v_y + c v_z}$$

**Casos especiais:**
- Se denominador = 0: reta **paralela** ao plano
  - Se numerador ≠ 0: sem interseção
  - Se numerador = 0: reta **contida** no plano

**Aplicação - Ray tracing:**
```cpp
// Lançar raio da câmera e ver onde acerta o plano
Ray camera_ray(camera_pos, pixel_direction);
Plane object_surface(normal, distance);

float t = object_surface.intersect(camera_ray);
if (t > 0) {  // Interseção à frente
    Point hit = camera_ray.at(t);
    Color pixel_color = shade(hit, object_surface);
}
```

---

### 7. Aplicações em Ciência da Computação

#### 7.1 Navegação e GPS

**Coordenadas geográficas:** Latitude/longitude são coordenadas esféricas, mas em distâncias curtas podem ser aproximadas por plano.

```python
# Calcular distância entre duas cidades (aproximação plana)
# 1 grau ≈ 111 km (na linha do equador)
def distance_km(lat1, lon1, lat2, lon2):
    dlat = (lat2 - lat1) * 111
    dlon = (lon2 - lon1) * 111 * cos(radians(lat1))
    return sqrt(dlat**2 + dlon**2)
```

#### 7.2 Detecção de Colisões

**Ponto vs Polígono (ray casting):**
```cpp
bool point_in_polygon(Point p, Polygon poly) {
    // Lançar raio horizontal da direita
    Ray ray(p, Vector(1, 0));
    int intersections = 0;
    
    for (Edge edge : poly.edges) {
        if (ray.intersects(edge))
            intersections++;
    }
    
    return (intersections % 2) == 1;  // Ímpar = dentro
}
```

#### 7.3 Renderização 3D

**Clipping (cortar geometria fora da câmera):**
```python
# Câmera tem 6 planos: near, far, left, right, top, bottom
frustum = [near_plane, far_plane, left_plane, 
           right_plane, top_plane, bottom_plane]

for triangle in scene:
    clipped = triangle
    for plane in frustum:
        clipped = clip_triangle_by_plane(clipped, plane)
    if clipped:
        render(clipped)
```

---

## 🔗 Conexões

### Com Álgebra Linear
- Retas e planos são **subespaços vetoriais**
- Produto vetorial ($\vec{u} \times \vec{v}$) para normal do plano
- Sistemas lineares para interseções

### Com Cálculo
- Retas tangentes (derivadas)
- Planos tangentes (derivadas parciais)
- Otimização (distâncias mínimas)

### Com Física
- Trajetórias de objetos (retas paramétricas)
- Superfícies de contato (planos)
- Reflexão/refração (ângulos com planos)

---

## 🎯 Checklist de Domínio

Antes de seguir, você deve conseguir:

- [ ] Converter entre equação vetorial, paramétrica e geral de reta
- [ ] Calcular interseção de duas retas (2D)
- [ ] Determinar se retas são paralelas, concorrentes ou coincidentes
- [ ] Encontrar vetor normal de um plano dados dois vetores diretores
- [ ] Calcular interseção reta-plano
- [ ] Aplicar distância ponto-plano em problemas práticos
- [ ] Implementar ray-plane intersection em código

---

## 📚 Próximos Passos

1. **Exercícios:** `k2-exercicios/e1-pontos-retas-planos-exercicios.md`
2. **Implementação:** `k3-implementacao/i1-pontos-retas-planos.md`
3. **Próximo tópico:** `t2-distancias-angulos.md`

---

## 💡 Dicas de Estudo

### Visualize!
Use **GeoGebra 3D** para plotar retas e planos:
```
// GeoGebra syntax
Linha1 = Linha((0,0,0), (1,2,3))  // Ponto + direção
Plano1 = Plano((1,0,0), (0,1,0), (0,0,1))  // 3 pontos
```

### Pense em aplicações reais
- Retas = trajetórias, raios de luz, segmentos
- Planos = superfícies, horizontes, clipping planes

### Pratique conversões
Exercite converter entre todas as formas de equações. É chato, mas essencial!

---

**Última atualização:** 30 de dezembro de 2025  
**Tempo de leitura:** ~60 minutos  
**Pré-requisito para:** Distâncias e Ângulos, Cônicas
