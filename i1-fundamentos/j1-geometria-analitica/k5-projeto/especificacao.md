# Especificação Técnica: Sistema de Navegação 3D

## 📐 Visão Geral

Sistema completo de navegação 3D que integra conversão de coordenadas GPS, cálculo de rotas, visualização 3D e detecção de colisões para simular navegação autônoma de veículos.

---

## 🎯 Requisitos Funcionais

### RF-01: Representação de Coordenadas GPS
- **Descrição:** Sistema deve trabalhar com coordenadas GPS (latitude, longitude, altitude)
- **Entrada:** Graus decimais (WGS84)
- **Saída:** Estrutura GPSCoord
- **Validação:** -90° ≤ lat ≤ 90°, -180° ≤ lon ≤ 180°
- **Prioridade:** Alta

### RF-02: Conversão GPS ↔ Cartesiano Local
- **Descrição:** Converter entre sistemas de coordenadas
- **Entrada:** GPSCoord + ponto de referência
- **Saída:** Point3D (ENU - East-North-Up)
- **Precisão:** Erro < 1m em raio de 10km
- **Algoritmo:** Projeção plana para pequenas áreas
- **Prioridade:** Alta

### RF-03: Cálculo de Distância GPS
- **Descrição:** Calcular distância entre coordenadas GPS
- **Entrada:** Duas GPSCoord
- **Saída:** Distância em metros
- **Algoritmo:** Fórmula de Haversine
- **Precisão:** Erro < 0.5%
- **Prioridade:** Alta

### RF-04: Carregamento de Rotas
- **Descrição:** Ler rotas de arquivo GPX
- **Entrada:** Arquivo GPX válido
- **Saída:** Lista de waypoints (GPSCoord)
- **Validação:** XML bem formado, waypoints válidos
- **Prioridade:** Alta

### RF-05: Cálculo de Desvio de Rota
- **Descrição:** Calcular distância perpendicular à rota planejada
- **Entrada:** Posição atual + rota
- **Saída:** Distância em metros + ponto mais próximo na rota
- **Algoritmo:** Projeção em segmento de reta
- **Prioridade:** Alta

### RF-06: Cálculo de Ângulo de Correção
- **Descrição:** Determinar direção para retornar à rota
- **Entrada:** Posição atual + rota + heading atual
- **Saída:** Ângulo de correção em graus (-180 a 180)
- **Algoritmo:** Bearing + diferença angular
- **Prioridade:** Média

### RF-07: Simulação de Veículo
- **Descrição:** Simular movimento do veículo
- **Estado:** Posição, velocidade, heading, aceleração
- **Entrada:** Comandos de controle (acelerar, virar)
- **Saída:** Estado atualizado após timestep
- **Física:** Cinemática básica (opcional: dinâmica simplificada)
- **Prioridade:** Alta

### RF-08: Detecção de Waypoint Alcançado
- **Descrição:** Verificar quando veículo chega a waypoint
- **Entrada:** Posição atual + waypoint + tolerância
- **Saída:** Booleano + distância restante
- **Tolerância padrão:** 20 metros
- **Prioridade:** Média

### RF-09: Geometria 3D - Pontos, Retas, Planos
- **Descrição:** Implementar primitivas geométricas
- **Primitivas:**
  - Ponto 3D
  - Reta 3D (paramétrica)
  - Plano (implícito)
  - Esfera (centro + raio)
- **Operações:**
  - Distância ponto-ponto
  - Distância ponto-reta
  - Distância ponto-plano
  - Interseção raio-plano
  - Interseção raio-esfera
- **Prioridade:** Alta

### RF-10: Detecção de Colisão
- **Descrição:** Detectar colisões entre objetos
- **Tipos:**
  - Esfera-Esfera
  - Esfera-Plano
  - Raio-Esfera (ray casting)
  - Raio-AABB (Axis-Aligned Bounding Box)
- **Entrada:** Geometrias dos objetos
- **Saída:** Booleano + informações de colisão (ponto, normal, distância)
- **Prioridade:** Alta

### RF-11: Visualização 3D
- **Descrição:** Renderizar cena em 3D
- **Elementos:**
  - Terreno/grid
  - Rota (linha 3D)
  - Veículo (modelo simplificado)
  - Obstáculos (esferas, caixas)
  - Câmera controlável
- **Tecnologia:** OpenGL ou similar
- **Prioridade:** Média

### RF-12: Controle de Câmera
- **Descrição:** Permitir visualização de diferentes ângulos
- **Modos:**
  - Vista livre (WASD + mouse)
  - Seguir veículo (3ª pessoa)
  - Vista do veículo (1ª pessoa)
  - Vista superior (ortográfica)
- **Prioridade:** Baixa

### RF-13: Piloto Automático
- **Descrição:** Navegação autônoma seguindo rota
- **Funcionalidades:**
  - Seguir waypoints
  - Corrigir desvios automaticamente
  - Evitar obstáculos (opcional)
  - Ajustar velocidade em curvas
- **Algoritmo:** PID controller ou similar
- **Prioridade:** Média

### RF-14: Interface de Usuário
- **Descrição:** Exibir informações em tempo real
- **Elementos:**
  - Posição atual (GPS + Local)
  - Velocidade
  - Desvio da rota
  - Próximo waypoint (distância)
  - Estatísticas (tempo, distância percorrida)
- **Prioridade:** Baixa

### RF-15: Transformações 3D
- **Descrição:** Matrizes de transformação para câmera e objetos
- **Transformações:**
  - Translação
  - Rotação (eixos X, Y, Z)
  - Escala
  - Look-at (câmera)
  - Perspectiva (projeção)
- **Prioridade:** Média

---

## 🔧 Requisitos Não-Funcionais

### RNF-01: Performance
- Renderização a 30+ FPS com até 1000 obstáculos
- Conversões GPS devem ser instantâneas (< 1ms)
- Colisões devem ser verificadas em < 5ms (otimização espacial)

### RNF-02: Precisão
- Erro de conversão GPS < 1m em 10km de raio
- Cálculo de desvio preciso até 0.1m
- Detecção de colisão precisa até 0.01m

### RNF-03: Usabilidade
- Controles intuitivos (teclado + mouse)
- Feedback visual imediato
- Mensagens de erro claras
- Ajuda em tela (tecla H)

### RNF-04: Manutenibilidade
- Código modular (geometria, navegação, render separados)
- Documentação inline
- Testes unitários para geometria e GPS
- Makefile/CMake para build

### RNF-05: Extensibilidade
- Fácil adicionar novos tipos de obstáculos
- Fácil adicionar novos algoritmos de navegação
- Suporte para diferentes formatos de rota (GPX, KML)

### RNF-06: Portabilidade
- Compilar em Linux, macOS, Windows
- Dependências mínimas (SDL2, OpenGL, GLM)

---

## 📊 Estrutura de Dados

### GPSCoord
```c
typedef struct {
    double latitude;   // Graus decimais [-90, 90]
    double longitude;  // Graus decimais [-180, 180]
    double altitude;   // Metros
} GPSCoord;
```

### Point3D / Vec3
```c
typedef struct {
    double x, y, z;  // Metros (sistema ENU)
} Point3D;
```

### Route
```c
typedef struct {
    GPSCoord* waypoints;      // Array de waypoints
    int num_waypoints;        // Número de waypoints
    double* distances;        // Distâncias acumuladas
    double total_distance;    // Distância total
} Route;
```

### Vehicle
```c
typedef struct {
    Point3D position;         // Posição local (ENU)
    GPSCoord gps_position;    // Posição GPS
    double speed;             // m/s
    double heading;           // Graus (0 = Norte, 90 = Leste)
    double acceleration;      // m/s²
    Sphere collision_volume;  // Para colisões
} Vehicle;
```

### NavigationState
```c
typedef struct {
    Route* route;
    Vehicle* vehicle;
    int current_segment;      // Índice do segmento da rota
    double deviation;         // Metros
    double correction_angle;  // Graus
    GPSCoord closest_point;   // Ponto mais próximo na rota
    double distance_to_next;  // Metros até próximo waypoint
} NavigationState;
```

### Line3D
```c
typedef struct {
    Point3D origin;    // Ponto por onde passa
    Point3D direction; // Vetor direção (não precisa ser unitário)
} Line3D;
```

### Plane3D
```c
typedef struct {
    Point3D normal;  // Vetor normal (unitário)
    double d;        // Termo constante: ax + by + cz + d = 0
} Plane3D;
```

### Sphere
```c
typedef struct {
    Point3D center;
    double radius;
} Sphere;
```

### AABB (Axis-Aligned Bounding Box)
```c
typedef struct {
    Point3D min;  // Canto mínimo
    Point3D max;  // Canto máximo
} AABB;
```

### Camera
```c
typedef struct {
    Point3D position;
    Point3D target;      // Look-at
    Point3D up;          // Vetor up
    double fov;          // Campo de visão (graus)
    double aspect;       // Aspect ratio
    double near, far;    // Planos de corte
} Camera;
```

### Matrix4x4
```c
typedef struct {
    float m[16];  // Column-major order (OpenGL)
} Matrix4x4;
```

---

## 🧮 Algoritmos Principais

### A01: Conversão GPS → Local (ENU)
```
Entrada: gps (lat, lon, alt), referência (lat₀, lon₀, alt₀)
Saída: (x, y, z) em metros

1. dlat = lat - lat₀
2. dlon = lon - lon₀
3. lat_media = (lat + lat₀) / 2

4. y = R_TERRA * deg_to_rad(dlat)           # Norte
5. x = R_TERRA * deg_to_rad(dlon) * cos(deg_to_rad(lat_media))  # Leste
6. z = alt - alt₀                            # Up

Retornar (x, y, z)
```

### A02: Haversine (Distância GPS)
```
Entrada: gps1 (lat₁, lon₁), gps2 (lat₂, lon₂)
Saída: distância em metros

1. φ₁ = deg_to_rad(lat₁), φ₂ = deg_to_rad(lat₂)
2. Δφ = φ₂ - φ₁
3. Δλ = deg_to_rad(lon₂ - lon₁)

4. a = sin²(Δφ/2) + cos(φ₁) * cos(φ₂) * sin²(Δλ/2)
5. c = 2 * atan2(√a, √(1-a))
6. d = R_TERRA * c

Retornar d
```

### A03: Desvio de Rota
```
Entrada: posição P, segmento (A, B)
Saída: distância perpendicular, ponto mais próximo

1. AB = B - A
2. AP = P - A

3. t = (AP · AB) / (AB · AB)
4. t_clamped = clamp(t, 0, 1)  # Limitar ao segmento

5. ponto_mais_proximo = A + t_clamped * AB
6. desvio = ||P - ponto_mais_proximo||

Retornar (desvio, ponto_mais_proximo)
```

### A04: Interseção Raio-Esfera
```
Entrada: raio (origem O, direção D), esfera (centro C, raio r)
Saída: (tem_hit, t1, t2)

1. OC = O - C
2. a = D · D
3. b = 2 * (OC · D)
4. c = (OC · OC) - r²

5. Δ = b² - 4ac

6. Se Δ < 0: retornar (falso, ∞, ∞)
7. Se Δ ≥ 0:
   t1 = (-b - √Δ) / (2a)
   t2 = (-b + √Δ) / (2a)
   retornar (verdadeiro, t1, t2)
```

### A05: Bearing (Azimute)
```
Entrada: de (lat₁, lon₁) para (lat₂, lon₂)
Saída: ângulo em graus (0 = Norte, 90 = Leste)

1. φ₁ = deg_to_rad(lat₁), φ₂ = deg_to_rad(lat₂)
2. Δλ = deg_to_rad(lon₂ - lon₁)

3. y = sin(Δλ) * cos(φ₂)
4. x = cos(φ₁)*sin(φ₂) - sin(φ₁)*cos(φ₂)*cos(Δλ)

5. θ = atan2(y, x)
6. bearing = rad_to_deg(θ)

Retornar (bearing + 360) % 360  # Normalizar [0, 360)
```

---

## 🧪 Casos de Teste

### CT-01: Conversão GPS Básica
```
Entrada:
  Referência: (-23.5505, -46.6333, 800)  # Paulista, SP
  Ponto: (-23.5475, -46.6361, 800)       # MASP

Esperado:
  Local ≈ (250 m Oeste, 330 m Norte, 0 m)
  Distância ≈ 413 m
```

### CT-02: Desvio de Rota
```
Entrada:
  Segmento: A(0,0,0) → B(100,0,0)
  Posição: P(50, 20, 0)

Esperado:
  Desvio: 20 m
  Ponto mais próximo: (50, 0, 0)
```

### CT-03: Interseção Raio-Esfera
```
Entrada:
  Raio: origem (0,0,0), direção (1,0,0)
  Esfera: centro (5,0,0), raio 2

Esperado:
  Hit: verdadeiro
  t1: 3.0 (ponto (3,0,0))
  t2: 7.0 (ponto (7,0,0))
```

### CT-04: Bearing
```
Entrada:
  De: (0°, 0°)
  Para: (1°, 0°)

Esperado:
  Bearing ≈ 0° (Norte)

Entrada:
  De: (0°, 0°)
  Para: (0°, 1°)

Esperado:
  Bearing ≈ 90° (Leste)
```

---

## 📁 Estrutura de Arquivos Detalhada

```
src/
├── main.c                        # Ponto de entrada, loop principal
├── geometry/
│   ├── point3d.h / .c            # Pontos 3D
│   ├── vec3.h / .c               # Vetores 3D (operações)
│   ├── line3d.h / .c             # Retas 3D
│   ├── plane3d.h / .c            # Planos
│   ├── sphere.h / .c             # Esferas
│   ├── aabb.h / .c               # Bounding boxes
│   └── transforms.h / .c         # Matrizes 4×4
├── navigation/
│   ├── gps.h / .c                # Conversões GPS
│   ├── route.h / .c              # Rotas e waypoints
│   ├── vehicle.h / .c            # Estado do veículo
│   ├── navigation.h / .c         # Lógica de navegação
│   └── autopilot.h / .c          # Piloto automático
├── collision/
│   ├── collision.h / .c          # Detecção de colisão
│   ├── raycast.h / .c            # Ray casting
│   └── bvh.h / .c                # Otimização espacial
├── render/
│   ├── camera.h / .c             # Câmera 3D
│   ├── renderer.h / .c           # Renderização OpenGL
│   ├── shader.h / .c             # Carregamento de shaders
│   └── ui.h / .c                 # Interface (texto/ImGui)
└── utils/
    ├── config.h / .c             # Configuração
    ├── logger.h / .c             # Logging
    └── math_utils.h / .c         # Utilidades matemáticas
```

---

## 🔌 Dependências Externas

### Obrigatórias
- **SDL2** (>= 2.0.0): Janela, input, contexto OpenGL
- **GLEW** (>= 2.0): Extensões OpenGL
- **GLM** (>= 0.9.9): Matemática 3D (opcional, pode implementar próprio)

### Opcionais
- **ImGui**: Interface gráfica avançada
- **libxml2**: Parser GPX robusto
- **stb_image**: Texturas (para terreno)

---

## 🎯 Métricas de Sucesso

### Funcionalidade
- [ ] Carrega e visualiza rota GPX corretamente
- [ ] Veículo navega autonomamente com desvio < 5m
- [ ] Colisões detectadas com precisão > 99%
- [ ] Renderização estável a 30+ FPS

### Código
- [ ] Cobertura de testes > 80% (geometria e GPS)
- [ ] Sem memory leaks (Valgrind)
- [ ] Sem warnings de compilação

### Documentação
- [ ] README completo com build instructions
- [ ] Comentários inline em funções complexas
- [ ] Diagramas de arquitetura

---

## 🚀 Roadmap de Implementação

1. **Semana 1:** Geometria básica + GPS
2. **Semana 2:** Roteamento + Navegação
3. **Semana 3:** Visualização 3D
4. **Semana 4:** Colisões + Integração
5. **Semana 5+:** Polimento + Features avançadas

---

## 📖 Referências Técnicas

- **GPS Math:** https://www.movable-type.co.uk/scripts/latlong.html
- **GPX Format:** https://www.topografix.com/gpx.asp
- **OpenGL:** https://learnopengl.com/
- **Collision Detection:** "Real-Time Collision Detection" (Christer Ericson)
- **Game Physics:** "Game Physics Engine Development" (Ian Millington)
