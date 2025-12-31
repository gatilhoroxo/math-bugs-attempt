# Distâncias e Ângulos

## 🎯 Meta de Aprendizado

Ao completar este tópico, você será capaz de:
- Calcular distâncias entre pontos, retas e planos em 2D e 3D
- Determinar ângulos entre vetores, retas e planos
- Usar projeções vetoriais para resolver problemas geométricos
- Aplicar esses conceitos em navegação, física e gráficos 3D

---

## ⏱️ Tempo Estimado

- **Leitura ativa:** 45-60 min
- **Exercícios relacionados:** 35-50 min (`k2-exercicios/e2-distancias-angulos-exercicios.md`)
- **Implementação:** 60-75 min (`k3-implementacao/i2-distancias-angulos.md`)
- **Total:** ~2h20-3h

---

## 📋 Pré-requisitos

- [x] Pontos, retas e planos (`t1-pontos-retas-planos.md`)
- [x] Produto escalar e vetorial
- [x] Vetores e normalização
- [ ] Trigonometria básica (seno, cosseno)

---

## 🗺️ Mapa Mental

```
DISTÂNCIAS E ÂNGULOS
├── 1. Distâncias
│   ├── Ponto a ponto (Euclidiana)
│   ├── Ponto a reta
│   ├── Ponto a plano
│   ├── Reta a reta (2D e 3D)
│   └── Plano a plano
│
├── 2. Ângulos
│   ├── Entre vetores (produto escalar)
│   ├── Entre retas
│   ├── Entre planos
│   └── Entre reta e plano
│
├── 3. Projeções
│   ├── Projeção escalar
│   ├── Projeção vetorial
│   └── Aplicações (sombras, componentes)
│
└── 4. Aplicações
    ├── Navegação (distâncias GPS)
    ├── Física (trabalho, torque)
    ├── Gráficos (iluminação, sombras)
    └── IA (pathfinding, visibilidade)
```

---

## 📖 Conteúdo

### 1. Por que Distâncias e Ângulos?

Em qualquer aplicação que envolva **espaço**, você precisa medir:
- **Distâncias:** Quão longe? Quanto falta? Colidiu?
- **Ângulos:** Qual direção? Está de frente? Quanto girar?

**Aplicações diretas:**

- **Navegação:** GPS calcula distância ao destino e ângulo de direção
- **Física de Jogos:** Distância para ativar trigger, ângulo de colisão
- **IA:** Pathfinding (A* usa distância euclidiana), campo de visão (ângulo)
- **Gráficos 3D:** Iluminação (ângulo luz-superfície), culling (distância câmera)
- **Robótica:** Sensores de distância, orientação angular

> 💡 **Insight:** Distância e ângulo são as **métricas fundamentais** do espaço.

---

### 2. Distâncias

#### 2.1 Distância Ponto a Ponto

Já vimos anteriormente - é a distância euclidiana:

$$d(P_1, P_2) = \|\vec{P_1P_2}\| = \sqrt{(x_2-x_1)^2 + (y_2-y_1)^2 + (z_2-z_1)^2}$$

**Otimização:** Para comparações (ex: "qual ponto é mais próximo?"), use **distância ao quadrado** para evitar raiz quadrada (operação cara):

```cpp
float distance_squared(Point a, Point b) {
    float dx = b.x - a.x;
    float dy = b.y - a.y;
    float dz = b.z - a.z;
    return dx*dx + dy*dy + dz*dz;  // Sem sqrt!
}

// Uso: encontrar inimigo mais próximo
Enemy* nearest = nullptr;
float min_dist_sq = INFINITY;

for (Enemy* e : enemies) {
    float dist_sq = distance_squared(player.pos, e->pos);
    if (dist_sq < min_dist_sq) {
        min_dist_sq = dist_sq;
        nearest = e;
    }
}
```

---

#### 2.2 Distância Ponto a Reta (2D)

Dado um ponto $P = (x_0, y_0)$ e uma reta $ax + by + c = 0$:

$$d(P, r) = \frac{|ax_0 + by_0 + c|}{\sqrt{a^2 + b^2}}$$

**Interpretação geométrica:** É a **projeção** do vetor $\vec{QP}$ (onde $Q$ é qualquer ponto da reta) sobre o vetor normal da reta.

**Demonstração visual:**
```
        P
       /|
      / | d (distância)
     /  |
    Q---+--- reta
```

**Se você tem forma paramétrica** $\vec{r}(t) = P_0 + t\vec{v}$:

1. Vetor normal: $\vec{n} = (-v_y, v_x)$ (2D) ou $\vec{n} = \vec{v} \times \vec{w}$ (3D)
2. Normalizar: $\hat{n} = \vec{n} / \|\vec{n}\|$
3. Distância: $d = |\vec{P_0P} \cdot \hat{n}|$

**Aplicação - Detecção de colisão círculo-linha:**
```python
def circle_line_collision(circle_center, radius, line_point, line_dir):
    """Círculo colide com linha se distância < raio"""
    # Vetor normal à linha
    normal = Vector(-line_dir.y, line_dir.x).normalize()
    
    # Distância centro do círculo à linha
    to_center = circle_center - line_point
    distance = abs(to_center.dot(normal))
    
    return distance < radius
```

---

#### 2.3 Distância Ponto a Reta (3D)

Dado ponto $P$ e reta $\vec{r}(t) = P_0 + t\vec{v}$:

**Método 1 - Produto vetorial:**

$$d(P, r) = \frac{\|\vec{P_0P} \times \vec{v}\|}{\|\vec{v}\|}$$

**Por quê funciona?** 
- $\|\vec{a} \times \vec{b}\| = \|\vec{a}\| \|\vec{b}\| \sin\theta$
- Isso é literalmente a área do paralelogramo formado por $\vec{P_0P}$ e $\vec{v}$
- Dividir por $\|\vec{v}\|$ dá a "altura" (distância perpendicular)

**Método 2 - Projeção:**

1. Projetar $\vec{P_0P}$ em $\vec{v}$: $\text{proj}_{\vec{v}} \vec{P_0P} = \frac{\vec{P_0P} \cdot \vec{v}}{\|\vec{v}\|^2} \vec{v}$
2. Componente perpendicular: $\vec{perp} = \vec{P_0P} - \text{proj}_{\vec{v}} \vec{P_0P}$
3. Distância: $d = \|\vec{perp}\|$

```cpp
float point_to_line_3d(Point P, Point P0, Vector v) {
    Vector P0P = P - P0;
    Vector cross_product = P0P.cross(v);
    return cross_product.length() / v.length();
}
```

---

#### 2.4 Distância Ponto a Plano

Dado ponto $P = (x_0, y_0, z_0)$ e plano $ax + by + cz + d = 0$:

$$d(P, \pi) = \frac{|ax_0 + by_0 + cz_0 + d|}{\sqrt{a^2 + b^2 + c^2}}$$

Ou, se o vetor normal $\vec{n} = (a, b, c)$ já está normalizado ($\|\vec{n}\| = 1$):

$$d(P, \pi) = |\vec{OP} \cdot \vec{n} + d|$$

**Sinal importa:**
- Positivo: $P$ está do lado da normal
- Negativo: $P$ está do lado oposto
- Zero: $P$ está **no** plano

**Aplicação - Frustum culling:**
```cpp
bool is_inside_frustum(Point object_pos, Plane frustum_planes[6]) {
    for (int i = 0; i < 6; i++) {
        float signed_distance = frustum_planes[i].signed_distance(object_pos);
        if (signed_distance < -object_radius)
            return false;  // Objeto fora deste plano
    }
    return true;
}
```

---

#### 2.5 Distância Reta a Reta

**2D:** Retas em 2D ou se cruzam (distância = 0) ou são paralelas.

Para retas **paralelas** $r_1$ e $r_2$:
- Pegue qualquer ponto $P$ de $r_1$
- Calcule distância de $P$ a $r_2$

**3D:** Retas podem ser **reversas** (não se cruzam e não são paralelas).

Dadas $r_1: P_1 + t\vec{v_1}$ e $r_2: P_2 + s\vec{v_2}$:

$$d(r_1, r_2) = \frac{|(\vec{P_1P_2}) \cdot (\vec{v_1} \times \vec{v_2})|}{\|\vec{v_1} \times \vec{v_2}\|}$$

**Interpretação:** Volume do paralelepípedo dividido pela área da base.

**Caso especial:** Se $\vec{v_1} \times \vec{v_2} = \vec{0}$, as retas são **paralelas**.

---

### 3. Ângulos

#### 3.1 Ângulo Entre Vetores

O **produto escalar** relaciona ângulo e vetores:

$$\vec{a} \cdot \vec{b} = \|\vec{a}\| \|\vec{b}\| \cos\theta$$

Isolando $\theta$:

$$\theta = \arccos\left(\frac{\vec{a} \cdot \vec{b}}{\|\vec{a}\| \|\vec{b}\|}\right)$$

**Casos especiais:**
- $\vec{a} \cdot \vec{b} = 0$ ⟹ $\theta = 90°$ (perpendiculares)
- $\vec{a} \cdot \vec{b} > 0$ ⟹ $\theta < 90°$ (ângulo agudo)
- $\vec{a} \cdot \vec{b} < 0$ ⟹ $\theta > 90°$ (ângulo obtuso)

**Aplicação - Campo de visão (FOV):**
```python
def is_in_fov(observer_pos, observer_dir, target_pos, fov_angle):
    """Verifica se alvo está dentro do campo de visão"""
    to_target = (target_pos - observer_pos).normalize()
    
    cos_angle = observer_dir.dot(to_target)
    angle = acos(cos_angle)
    
    return angle < fov_angle / 2  # FOV é ângulo total (dividir por 2)
```

**Aplicação - Iluminação difusa (Lei de Lambert):**
```cpp
float diffuse_lighting(Vector light_dir, Vector surface_normal) {
    // Intensidade = max(0, cos(θ))
    float cos_theta = light_dir.dot(surface_normal);
    return max(0.0f, cos_theta);
}
```

---

#### 3.2 Ângulo Entre Retas

**Retas** $r_1: \vec{v_1}$ e $r_2: \vec{v_2}$ (vetores diretores):

$$\theta = \arccos\left(\frac{|\vec{v_1} \cdot \vec{v_2}|}{\|\vec{v_1}\| \|\vec{v_2}\|}\right)$$

**Nota:** Usamos valor absoluto porque queremos o **menor** ângulo (0° a 90°).

**Casos especiais:**
- Paralelas: $\vec{v_1} \parallel \vec{v_2}$ ⟹ $\theta = 0°$
- Perpendiculares: $\vec{v_1} \cdot \vec{v_2} = 0$ ⟹ $\theta = 90°$

---

#### 3.3 Ângulo Entre Planos

**Planos** $\pi_1: \vec{n_1}$ e $\pi_2: \vec{n_2}$ (vetores normais):

$$\theta = \arccos\left(\frac{|\vec{n_1} \cdot \vec{n_2}|}{\|\vec{n_1}\| \|\vec{n_2}\|}\right)$$

**Interpretação:** Ângulo entre planos = ângulo entre suas normais.

**Aplicação - Dobras em modelagem 3D:**
```python
def calculate_crease(face1_normal, face2_normal):
    """Ângulo de dobra entre duas faces"""
    cos_angle = face1_normal.dot(face2_normal)
    angle_rad = acos(cos_angle)
    return degrees(angle_rad)
```

---

#### 3.4 Ângulo Entre Reta e Plano

Reta $\vec{v}$ (diretor) e plano $\vec{n}$ (normal):

$$\theta = \arcsin\left(\frac{|\vec{v} \cdot \vec{n}|}{\|\vec{v}\| \|\vec{n}\|}\right)$$

Ou: $\theta = 90° - \arccos\left(\frac{|\vec{v} \cdot \vec{n}|}{\|\vec{v}\| \|\vec{n}\|}\right)$

**Por quê seno?** Porque queremos o ângulo com o **plano**, não com a **normal** (que seria cosseno).

**Aplicação - Ângulo de impacto (física):**
```cpp
float impact_angle(Vector projectile_velocity, Vector surface_normal) {
    float cos_theta = projectile_velocity.dot(surface_normal) / 
                      (projectile_velocity.length() * surface_normal.length());
    return 90 - degrees(acos(cos_theta));
}
```

---

### 4. Projeções Vetoriais

#### 4.1 Projeção Escalar

A **projeção escalar** de $\vec{a}$ sobre $\vec{b}$ é:

$$\text{comp}_{\vec{b}} \vec{a} = \frac{\vec{a} \cdot \vec{b}}{\|\vec{b}\|}$$

**Interpretação:** "Quanto de $\vec{a}$ aponta na direção de $\vec{b}$"

```
    a
   /|
  / |
 /  | componente perpendicular
/__θ|
  proj (projeção)
    b →
```

**Aplicação - Velocidade paralela/perpendicular:**
```python
def decompose_velocity(velocity, surface_normal):
    """Separa velocidade em componentes paralela e perpendicular"""
    # Componente perpendicular (normal)
    normal_component = velocity.dot(surface_normal) * surface_normal
    
    # Componente paralela (tangente)
    tangent_component = velocity - normal_component
    
    return tangent_component, normal_component
```

---

#### 4.2 Projeção Vetorial

A **projeção vetorial** de $\vec{a}$ sobre $\vec{b}$ é:

$$\text{proj}_{\vec{b}} \vec{a} = \left(\frac{\vec{a} \cdot \vec{b}}{\vec{b} \cdot \vec{b}}\right) \vec{b}$$

Ou, se $\vec{b}$ é unitário ($\|\vec{b}\| = 1$):

$$\text{proj}_{\vec{b}} \vec{a} = (\vec{a} \cdot \vec{b}) \vec{b}$$

**Uso:** Encontrar componente de um vetor em uma direção.

```cpp
Vector project(Vector a, Vector b) {
    float scalar = a.dot(b) / b.dot(b);
    return scalar * b;
}
```

**Aplicação - Movimento em superfície inclinada:**
```python
# Gravidade é sempre para baixo
gravity = Vector(0, -9.8, 0)

# Superfície tem normal
surface_normal = Vector(0.6, 0.8, 0).normalize()  # Rampa

# Força que faz objeto deslizar rampa abaixo
gravity_tangent = gravity - project(gravity, surface_normal)
```

---

#### 4.3 Componente Perpendicular

$$\vec{a}_{\perp} = \vec{a} - \text{proj}_{\vec{b}} \vec{a}$$

**Aplicação - Reflexão:**
```cpp
Vector reflect(Vector incident, Vector normal) {
    // Reflexão = incidente - 2 * (componente normal)
    Vector normal_component = project(incident, normal);
    return incident - 2 * normal_component;
}
```

---

### 5. Fórmulas de Distância (Resumo)

| De | Para | Fórmula |
|----|------|---------|
| Ponto | Ponto | $\sqrt{(x_2-x_1)^2 + (y_2-y_1)^2 + (z_2-z_1)^2}$ |
| Ponto | Reta (2D) | $\frac{\|ax_0 + by_0 + c\|}{\sqrt{a^2+b^2}}$ |
| Ponto | Reta (3D) | $\frac{\|\vec{P_0P} \times \vec{v}\|}{\|\vec{v}\|}$ |
| Ponto | Plano | $\frac{\|ax_0 + by_0 + cz_0 + d\|}{\sqrt{a^2+b^2+c^2}}$ |
| Reta | Reta (3D) | $\frac{\|(\vec{P_1P_2}) \cdot (\vec{v_1} \times \vec{v_2})\|}{\|\vec{v_1} \times \vec{v_2}\|}$ |

---

### 6. Aplicações Práticas

#### 6.1 Navegação - Distância Ortodrômica (Great Circle)

Para distâncias curtas, aproximação plana funciona. Para longas distâncias na Terra (esfera):

$$d = R \cdot \arccos(\sin\phi_1 \sin\phi_2 + \cos\phi_1 \cos\phi_2 \cos(\Delta\lambda))$$

Onde:
- $R$ = raio da Terra (≈ 6371 km)
- $\phi_1, \phi_2$ = latitudes em radianos
- $\Delta\lambda$ = diferença de longitude

**Fórmula Haversine (mais estável numericamente):**
```python
def haversine(lat1, lon1, lat2, lon2):
    """Distância em km entre coordenadas GPS"""
    R = 6371  # Raio da Terra em km
    
    dlat = radians(lat2 - lat1)
    dlon = radians(lon2 - lon1)
    
    a = sin(dlat/2)**2 + cos(radians(lat1)) * cos(radians(lat2)) * sin(dlon/2)**2
    c = 2 * atan2(sqrt(a), sqrt(1-a))
    
    return R * c
```

---

#### 6.2 IA - Pathfinding

**A* usa heurística de distância:**
```python
def a_star(start, goal, graph):
    def heuristic(node):
        # Distância euclidiana como estimativa
        return distance(node.pos, goal.pos)
    
    open_set = PriorityQueue()
    open_set.put((0, start))
    
    while not open_set.empty():
        current = open_set.get()[1]
        
        if current == goal:
            return reconstruct_path(current)
        
        for neighbor in graph.neighbors(current):
            g = g_score[current] + distance(current, neighbor)
            f = g + heuristic(neighbor)
            # ... resto do A*
```

---

#### 6.3 Física - Trabalho

**Trabalho** é força na direção do deslocamento:

$$W = \vec{F} \cdot \vec{d}$$

Isso é literalmente **projeção** de $\vec{F}$ em $\vec{d}$!

```python
force = Vector(10, 5, 0)  # 10N horizontal, 5N vertical
displacement = Vector(3, 0, 0)  # Move 3m horizontalmente

work = force.dot(displacement)  # = 30 J (só componente horizontal trabalha)
```

---

## 🔗 Conexões

### Com Álgebra Linear
- Produto escalar define ângulo
- Produto vetorial define normal (perpendicular)
- Projeções são operadores lineares

### Com Cálculo
- Gradiente aponta na direção de maior crescimento (perpendicular às curvas de nível)
- Vetor tangente vs vetor normal

### Próximo Tópico
- Cônicas usam distâncias (elipse, hipérbole, parábola)

---

## 🎯 Checklist de Domínio

- [ ] Calcular distância ponto-reta em 2D e 3D
- [ ] Calcular distância ponto-plano
- [ ] Determinar ângulo entre vetores usando produto escalar
- [ ] Aplicar projeção vetorial para decompor movimento
- [ ] Implementar campo de visão (FOV) com ângulos
- [ ] Usar distância ao quadrado para otimização

---

## 📚 Próximos Passos

1. **Exercícios:** `k2-exercicios/e2-distancias-angulos-exercicios.md`
2. **Implementação:** `k3-implementacao/i2-distancias-angulos.md`
3. **Próximo tópico:** `t3-conicas-superficies.md`

---

**Última atualização:** 30 de dezembro de 2025  
**Tempo de leitura:** ~55 minutos
