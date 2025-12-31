# Sistemas de Coordenadas e Transformações

## 🎯 Meta de Aprendizado

Ao completar este tópico, você será capaz de:
- Converter entre sistemas de coordenadas (cartesianas, polares, esféricas, cilíndricas)
- Aplicar transformações geométricas (translação, rotação, escala, reflexão)
- Usar coordenadas homogêneas para composição de transformações
- Implementar sistemas de coordenadas para navegação (GPS, bússola)

---

## ⏱️ Tempo Estimado

- **Leitura ativa:** 50-70 min
- **Exercícios relacionados:** 40-55 min (`k2-exercicios/e4-coordenadas-transformacoes-exercicios.md`)
- **Implementação:** 65-85 min (`k3-implementacao/i4-coordenadas-transformacoes.md`)
- **Total:** ~2h35-3h30

---

## 📋 Pré-requisitos

- [x] Vetores e ângulos
- [x] Trigonometria (seno, cosseno, tangente)
- [x] Matrizes (álgebra linear básica)
- [ ] Álgebra Linear: transformações lineares

---

## 🗺️ Mapa Mental

```
COORDENADAS E TRANSFORMAÇÕES
├── 1. Sistemas de Coordenadas 2D
│   ├── Cartesianas (x, y)
│   ├── Polares (r, θ)
│   └── Conversões
│
├── 2. Sistemas de Coordenadas 3D
│   ├── Cartesianas (x, y, z)
│   ├── Cilíndricas (r, θ, z)
│   ├── Esféricas (ρ, θ, φ)
│   └── Geográficas (lat, lon)
│
├── 3. Transformações Geométricas 2D
│   ├── Translação
│   ├── Rotação
│   ├── Escala
│   ├── Reflexão
│   ├── Cisalhamento
│   └── Coordenadas homogêneas
│
└── 4. Transformações 3D
    ├── Rotações (Euler, quaternions)
    ├── Matrizes de transformação
    └── Composição
```

---

## 📖 Conteúdo

### 1. Por que Múltiplos Sistemas de Coordenadas?

**Problema:** Nem todos os problemas se encaixam naturalmente em coordenadas cartesianas $(x, y, z)$.

**Exemplos:**
- **Navegação:** Latitude/longitude são coordenadas esféricas
- **Radar:** Usa coordenadas polares (distância e ângulo)
- **Gráficos 3D:** Câmera tem seu próprio sistema de coordenadas
- **Robótica:** Cada junta do robô tem coordenadas locais

> 💡 **Princípio:** Escolha o sistema de coordenadas que simplifica o problema.

---

### 2. Coordenadas Polares (2D)

#### 2.1 Definição

Um ponto $P$ é descrito por:
- $r$ = distância à origem (raio)
- $\theta$ = ângulo com eixo $x$ positivo (radianos)

```
    y
    |
    |   P(r, θ)
    |  /|
    | / |
    |/θ |
    +---------- x
    O   r
```

#### 2.2 Conversão Cartesiana ↔ Polar

**Polar → Cartesiana:**
$$\begin{cases}
x = r \cos\theta \\
y = r \sin\theta
\end{cases}$$

**Cartesiana → Polar:**
$$\begin{cases}
r = \sqrt{x^2 + y^2} \\
\theta = \arctan2(y, x)
\end{cases}$$

**Nota:** Use `atan2(y, x)` em vez de `atan(y/x)` para obter ângulo correto em todos os quadrantes.

```python
import math

def cartesian_to_polar(x, y):
    r = math.sqrt(x**2 + y**2)
    theta = math.atan2(y, x)
    return r, theta

def polar_to_cartesian(r, theta):
    x = r * math.cos(theta)
    y = r * math.sin(theta)
    return x, y
```

#### 2.3 Aplicações

**Radar/Sonar:**
```python
# Sistema de radar retorna (distância, ângulo)
radar_reading = (1500, radians(45))  # 1500m a 45°

# Converter para coordenadas do mapa
x, y = polar_to_cartesian(radar_reading[0], radar_reading[1])
map_position = (radar_x + x, radar_y + y)
```

**Movimento circular (jogos):**
```cpp
// Objeto orbitando em círculo
float orbit_radius = 5.0;
float angular_speed = 2.0;  // rad/s
float angle = angular_speed * time;

// Posição em coordenadas polares → cartesianas
object.x = center_x + orbit_radius * cos(angle);
object.y = center_y + orbit_radius * sin(angle);
```

---

### 3. Coordenadas Cilíndricas (3D)

#### 3.1 Definição

Extensão de polares para 3D:
- $(r, \theta, z)$
- $r$ = distância ao eixo $z$
- $\theta$ = ângulo no plano $xy$
- $z$ = altura

```
      z
      |
      |_____ P(r, θ, z)
      |   /|
      |  / |
      | /θ |
      +---------- y
     /
    x
```

#### 3.2 Conversão

**Cilíndrica → Cartesiana:**
$$\begin{cases}
x = r \cos\theta \\
y = r \sin\theta \\
z = z
\end{cases}$$

**Cartesiana → Cilíndrica:**
$$\begin{cases}
r = \sqrt{x^2 + y^2} \\
\theta = \arctan2(y, x) \\
z = z
\end{cases}$$

#### 3.3 Aplicações

**Objetos com simetria cilíndrica:** Tubos, torres, silos.

---

### 4. Coordenadas Esféricas (3D)

#### 4.1 Definição

Um ponto $P$ é descrito por:
- $\rho$ (rho) = distância à origem
- $\theta$ = ângulo azimutal (no plano $xy$)
- $\phi$ (phi) = ângulo polar (com eixo $z$)

```
      z
      |
      |\
      | \ ρ
      |  \
      | φ \__ P(ρ, θ, φ)
      |   /
      +---------- y
     /  θ
    x
```

**Convenção:** $\phi \in [0, \pi]$, $\theta \in [0, 2\pi]$

#### 4.2 Conversão

**Esférica → Cartesiana:**
$$\begin{cases}
x = \rho \sin\phi \cos\theta \\
y = \rho \sin\phi \sin\theta \\
z = \rho \cos\phi
\end{cases}$$

**Cartesiana → Esférica:**
$$\begin{cases}
\rho = \sqrt{x^2 + y^2 + z^2} \\
\theta = \arctan2(y, x) \\
\phi = \arccos(z / \rho)
\end{cases}$$

```python
def spherical_to_cartesian(rho, theta, phi):
    x = rho * math.sin(phi) * math.cos(theta)
    y = rho * math.sin(phi) * math.sin(theta)
    z = rho * math.cos(phi)
    return x, y, z

def cartesian_to_spherical(x, y, z):
    rho = math.sqrt(x**2 + y**2 + z**2)
    theta = math.atan2(y, x)
    phi = math.acos(z / rho) if rho != 0 else 0
    return rho, theta, phi
```

#### 4.3 Aplicações

**Astronomia:** Direção de estrelas (ascensão reta, declinação).

**Gráficos 3D:** Câmera orbitando objeto.

```cpp
// Câmera orbital (visualizadores 3D)
class OrbitCamera {
    float distance = 10.0;   // ρ
    float azimuth = 0.0;     // θ
    float elevation = 0.0;   // π/2 - φ (convenção alternativa)
    
    Vector3 get_position() {
        float phi = M_PI/2 - elevation;
        return spherical_to_cartesian(distance, azimuth, phi);
    }
};
```

---

### 5. Coordenadas Geográficas

#### 5.1 Latitude e Longitude

Sistema esférico adaptado para a Terra:
- **Latitude** (lat): ângulo ao norte/sul do equador ($-90°$ a $+90°$)
- **Longitude** (lon): ângulo leste/oeste do meridiano de Greenwich ($-180°$ a $+180°$)
- **Altitude** (alt): altura acima do nível do mar

```
       North Pole
           |
           |  lat (+)
    ----Equator----
           |  lat (-)
           |
      South Pole
      
    lon (-) ← Greenwich → lon (+)
```

#### 5.2 Conversão para Cartesianas (ECEF)

**Earth-Centered, Earth-Fixed (ECEF):**

Para elipsoide WGS84:
```python
def geodetic_to_ecef(lat, lon, alt):
    """Lat/Lon em graus, alt em metros"""
    a = 6378137.0  # Semi-eixo maior (m)
    e2 = 0.00669437999014  # Excentricidade²
    
    lat_rad = radians(lat)
    lon_rad = radians(lon)
    
    N = a / sqrt(1 - e2 * sin(lat_rad)**2)  # Raio de curvatura
    
    x = (N + alt) * cos(lat_rad) * cos(lon_rad)
    y = (N + alt) * cos(lat_rad) * sin(lon_rad)
    z = (N * (1 - e2) + alt) * sin(lat_rad)
    
    return x, y, z
```

#### 5.3 Distância Entre Coordenadas GPS

**Fórmula Haversine (já vista):**
```python
def haversine_distance(lat1, lon1, lat2, lon2):
    """Distância em km"""
    R = 6371  # Raio da Terra
    
    dlat = radians(lat2 - lat1)
    dlon = radians(lon2 - lon1)
    
    a = sin(dlat/2)**2 + cos(radians(lat1)) * cos(radians(lat2)) * sin(dlon/2)**2
    c = 2 * atan2(sqrt(a), sqrt(1-a))
    
    return R * c
```

**Bearing (ângulo de direção):**
```python
def bearing(lat1, lon1, lat2, lon2):
    """Ângulo de navegação (0° = Norte, 90° = Leste)"""
    lat1, lon1, lat2, lon2 = map(radians, [lat1, lon1, lat2, lon2])
    
    dlon = lon2 - lon1
    
    y = sin(dlon) * cos(lat2)
    x = cos(lat1) * sin(lat2) - sin(lat1) * cos(lat2) * cos(dlon)
    
    bearing_rad = atan2(y, x)
    bearing_deg = (degrees(bearing_rad) + 360) % 360
    
    return bearing_deg
```

---

### 6. Transformações Geométricas 2D

#### 6.1 Translação

Deslocar ponto por $(t_x, t_y)$:

$$\begin{pmatrix} x' \\ y' \end{pmatrix} = \begin{pmatrix} x \\ y \end{pmatrix} + \begin{pmatrix} t_x \\ t_y \end{pmatrix}$$

**Não é linear!** (não pode ser representada por matriz 2×2)

```python
def translate(point, tx, ty):
    return Point(point.x + tx, point.y + ty)
```

#### 6.2 Rotação

Rotação de ângulo $\theta$ (anti-horário) ao redor da origem:

$$\begin{pmatrix} x' \\ y' \end{pmatrix} = \begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix} \begin{pmatrix} x \\ y \end{pmatrix}$$

```python
def rotate(point, angle):
    """Ângulo em radianos"""
    cos_a = cos(angle)
    sin_a = sin(angle)
    
    x_new = point.x * cos_a - point.y * sin_a
    y_new = point.x * sin_a + point.y * cos_a
    
    return Point(x_new, y_new)
```

**Rotação ao redor de ponto arbitrário $(c_x, c_y)$:**
1. Transladar para mover centro para origem: $P' = P - C$
2. Rotacionar: $P'' = R \cdot P'$
3. Transladar de volta: $P_{final} = P'' + C$

#### 6.3 Escala

Multiplicar coordenadas por fatores $(s_x, s_y)$:

$$\begin{pmatrix} x' \\ y' \end{pmatrix} = \begin{pmatrix} s_x & 0 \\ 0 & s_y \end{pmatrix} \begin{pmatrix} x \\ y \end{pmatrix}$$

**Escala uniforme:** $s_x = s_y$

```cpp
Point scale(Point p, float sx, float sy) {
    return Point(p.x * sx, p.y * sy);
}
```

#### 6.4 Reflexão

**Sobre eixo $x$:**
$$\begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}$$

**Sobre eixo $y$:**
$$\begin{pmatrix} -1 & 0 \\ 0 & 1 \end{pmatrix}$$

**Sobre linha $y = x$:**
$$\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}$$

#### 6.5 Cisalhamento (Shear)

**Horizontal:**
$$\begin{pmatrix} 1 & k \\ 0 & 1 \end{pmatrix}$$

```python
# Efeito de vento em sprite
def shear_horizontal(point, k):
    return Point(point.x + k * point.y, point.y)
```

---

### 7. Coordenadas Homogêneas

**Problema:** Translação não é linear - não pode ser representada por matriz.

**Solução:** Adicionar dimensão extra!

**2D:** $(x, y) \rightarrow (x, y, 1)$

Agora translação pode ser matriz 3×3:

$$\begin{pmatrix} x' \\ y' \\ 1 \end{pmatrix} = \begin{pmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1 \end{pmatrix} \begin{pmatrix} x \\ y \\ 1 \end{pmatrix}$$

**Vantagem:** Podemos **compor** transformações multiplicando matrizes!

#### 7.1 Transformações em Coordenadas Homogêneas

**Translação:**
$$T(t_x, t_y) = \begin{pmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1 \end{pmatrix}$$

**Rotação:**
$$R(\theta) = \begin{pmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{pmatrix}$$

**Escala:**
$$S(s_x, s_y) = \begin{pmatrix} s_x & 0 & 0 \\ 0 & s_y & 0 \\ 0 & 0 & 1 \end{pmatrix}$$

#### 7.2 Composição de Transformações

**Ordem importa!**

```python
# Rodar sprite ao redor de seu centro
center = Point(100, 100)
angle = radians(45)

# Composição: T(center) · R(angle) · T(-center)
M = translate(center) @ rotate(angle) @ translate(-center)

# Aplicar a todos os vértices
for vertex in sprite.vertices:
    vertex.pos = M @ vertex.pos
```

**Exemplo - Hierarquia de transformações (braço robótico):**
```cpp
// Transformação global = Ombro · Cotovelo · Pulso
Matrix4 shoulder_transform = translate(0, 0, 0) * rotate_z(theta1);
Matrix4 elbow_transform = translate(L1, 0, 0) * rotate_z(theta2);
Matrix4 wrist_transform = translate(L2, 0, 0) * rotate_z(theta3);

Matrix4 hand_global = shoulder_transform * elbow_transform * wrist_transform;
```

---

### 8. Transformações 3D

#### 8.1 Matrizes de Rotação 3D

**Rotação ao redor do eixo $x$:**
$$R_x(\theta) = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & \cos\theta & -\sin\theta & 0 \\ 0 & \sin\theta & \cos\theta & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}$$

**Rotação ao redor do eixo $y$:**
$$R_y(\theta) = \begin{pmatrix} \cos\theta & 0 & \sin\theta & 0 \\ 0 & 1 & 0 & 0 \\ -\sin\theta & 0 & \cos\theta & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}$$

**Rotação ao redor do eixo $z$:**
$$R_z(\theta) = \begin{pmatrix} \cos\theta & -\sin\theta & 0 & 0 \\ \sin\theta & \cos\theta & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}$$

#### 8.2 Ângulos de Euler

Rotação 3D como sequência de 3 rotações (ex: yaw-pitch-roll):

$$R = R_z(\text{yaw}) \cdot R_y(\text{pitch}) \cdot R_x(\text{roll})$$

**Problema:** Gimbal lock (perda de grau de liberdade em certas orientações).

**Solução:** Quaternions (mais avançado).

```cpp
// Aplicação - Câmera FPS
class FPSCamera {
    float yaw = 0.0;    // Rotação horizontal
    float pitch = 0.0;  // Rotação vertical (limitada)
    
    Vector3 get_forward() {
        Vector3 forward;
        forward.x = cos(pitch) * cos(yaw);
        forward.y = sin(pitch);
        forward.z = cos(pitch) * sin(yaw);
        return forward.normalize();
    }
};
```

---

### 9. Mudança de Base (Sistemas de Coordenadas Locais)

**Problema:** Objeto tem sistema de coordenadas local, mas precisa ser renderizado no sistema global.

**Solução:** Matriz de mudança de base.

**Exemplo - Câmera (view matrix):**
```cpp
// Câmera em posição eye, olhando para target, com up definindo "cima"
Matrix4 look_at(Vector3 eye, Vector3 target, Vector3 up) {
    Vector3 z = (eye - target).normalize();  // Câmera olha para -z
    Vector3 x = up.cross(z).normalize();
    Vector3 y = z.cross(x);
    
    // Matriz de rotação (mudança de base) + translação
    return Matrix4(
        x.x, x.y, x.z, -x.dot(eye),
        y.x, y.y, y.z, -y.dot(eye),
        z.x, z.y, z.z, -z.dot(eye),
        0,   0,   0,   1
    );
}
```

---

## 🔗 Conexões

### Com Álgebra Linear
- Transformações lineares = matrizes
- Composição = multiplicação matricial
- Mudança de base

### Com Cálculo
- Jacobiano para conversão de coordenadas (integrais)
- Derivada de rotação = velocidade angular

### Com Física
- Sistemas de referência inerciais vs não-inerciais
- Transformações de Lorentz (relatividade)

---

## 🎯 Checklist de Domínio

- [ ] Converter entre cartesianas e polares/esféricas
- [ ] Calcular distância e bearing entre coordenadas GPS
- [ ] Implementar rotação 2D ao redor de ponto arbitrário
- [ ] Usar coordenadas homogêneas para compor transformações
- [ ] Aplicar matriz look-at para câmera 3D
- [ ] Entender ordem de transformações (TRS: Translate-Rotate-Scale)

---

## 📚 Próximos Passos

1. **Exercícios:** `k2-exercicios/e4-coordenadas-transformacoes-exercicios.md`
2. **Implementação:** `k3-implementacao/i4-coordenadas-transformacoes.md`
3. **Projeto final:** `k5-projeto/` - Sistema de Navegação

---

## 💡 Dicas Práticas

### Depuração de Transformações

```python
# Sempre desenhe os eixos para visualizar sistema de coordenadas
def draw_axes(transform):
    origin = transform @ Point(0, 0, 0)
    x_axis = transform @ Point(1, 0, 0)
    y_axis = transform @ Point(0, 1, 0)
    z_axis = transform @ Point(0, 0, 1)
    
    draw_line(origin, x_axis, color=RED)
    draw_line(origin, y_axis, color=GREEN)
    draw_line(origin, z_axis, color=BLUE)
```

### Ordem de Transformações

**Lembrete:** Matrizes multiplicam **da direita para esquerda**.

```cpp
// Código: escalar → rodar → transladar
M = T * R * S

// Aplicação: transformações são aplicadas na ordem S, R, T
vertex' = M * vertex = T * (R * (S * vertex))
```

---

**Última atualização:** 30 de dezembro de 2025  
**Tempo de leitura:** ~60 minutos
