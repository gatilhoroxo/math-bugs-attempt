# Projeto Âncora: Sistema de Navegação 3D com Colisões

## 🎯 Objetivo do Projeto

Criar um sistema completo de navegação 3D que integra GPS, geometria analítica, transformações e detecção de colisões. Este projeto consolida **todos** os conceitos de geometria analítica em uma aplicação prática.

**O que você vai aprender:**
- Conversão entre sistemas de coordenadas (GPS ↔ Cartesiano)
- Cálculo de rotas e desvios em tempo real
- Ray casting para navegação e visibilidade
- Detecção de colisões com geometria 3D
- Visualização e debugging de sistemas geométricos complexos

---

## ⏱️ Tempo Estimado

- **Total:** 10-15 horas (distribuídas ao longo de 3-4 semanas)
- **Etapa 0:** 45 min (setup e estrutura)
- **Etapas 1-2:** 3-4h (conversões e roteamento)
- **Etapas 3-4:** 4-6h (visualização 3D e colisões)
- **Etapa 5:** 2-3h (integração completa)
- **Etapa 6:** 3-5h (opcional, features avançadas)

---

## 🎚️ Dificuldade

⭐⭐⭐⭐⭐ Muito Avançado

**Nível de complexidade:**
- Programação: Avançado
- Matemática: Intermediário-Avançado
- Gráficos 3D: Intermediário

---

## 💪 Sistema de XP

| Etapa | Descrição | XP | Status |
|-------|-----------|-----|--------|
| **0** | Setup e estruturas base | 30 XP | ⬜ |
| **1** | Sistema de coordenadas GPS | 60 XP | ⬜ |
| **2** | Roteamento e navegação | 70 XP | ⬜ |
| **3** | Visualização 3D básica | 60 XP | ⬜ |
| **4** | Detecção de colisões | 80 XP | ⬜ |
| **5** | Integração completa | 70 XP | ⬜ |
| **6.1** | Múltiplos veículos | +50 XP | ⬜ |
| **6.2** | Obstáculos dinâmicos | +60 XP | ⬜ |
| **6.3** | Otimização espacial (BVH) | +70 XP | ⬜ |
| **6.4** | Exportação para visualizadores | +40 XP | ⬜ |
| **6.5** | Replay de trajetória | +50 XP | ⬜ |

**XP Total Base:** 370 XP  
**XP Total com Bônus:** 640 XP

---

## 📊 Rastreamento de Progresso

- [ ] Etapa 0 completa (Setup) - 30 XP
- [ ] Etapa 1 completa (GPS) - 60 XP
- [ ] Etapa 2 completa (Navegação) - 70 XP
- [ ] Etapa 3 completa (Visualização) - 60 XP
- [ ] Etapa 4 completa (Colisões) - 80 XP
- [ ] Etapa 5 completa (Integração) - 70 XP
- [ ] Projeto Base Completo (0-5) - 370 XP
- [ ] Features Avançadas (6.1-6.5) - até +270 XP

**XP Conquistado:** ___ / 370 XP (base) ou ___ / 640 XP (completo)

---

## ⌨️ Guia Rápido de Controles

### Visualização
- **W A S D** - Mover câmera (forward/left/back/right)
- **Q E** - Mover câmera (up/down)
- **Mouse** - Rotacionar câmera (clique + arraste)
- **Scroll** - Zoom in/out
- **Espaço** - Pausar/resumir simulação

### Navegação
- **↑ ↓** - Aumentar/diminuir velocidade do veículo
- **← →** - Virar esquerda/direita (manual override)
- **R** - Reset para início da rota
- **N** - Próximo waypoint
- **A** - Auto-navegação (piloto automático)

### Visualização de Dados
- **1** - Vista superior (ortográfica)
- **2** - Vista lateral
- **3** - Vista de seguir veículo (3ª pessoa)
- **4** - Vista do veículo (1ª pessoa)
- **G** - Mostrar/esconder grid
- **L** - Mostrar/esconder linha da rota
- **C** - Mostrar/esconder geometria de colisão
- **I** - Painel de informações (stats)

### Sistema
- **P** - Imprimir estatísticas no terminal
- **F12** - Screenshot
- **ESC** - Sair

**Dica:** Pressione **H** no programa para mostrar esta ajuda.

---

## 📋 Requisitos

### Software
- **Linguagem:** C/C++ (recomendado) ou Python
- **Bibliotecas:**
  - **Gráficos:** SDL2 + OpenGL (ou SFML) para visualização 3D
  - **Matemática:** GLM (OpenGL Mathematics) para álgebra linear
  - **Parser:** JSON-C ou similar para configuração
  
**Instalação (Ubuntu/Debian):**
```bash
sudo apt-get install libsdl2-dev libglew-dev libglm-dev libjson-c-dev
```

**Alternativa Python:**
- Pygame + PyOpenGL + NumPy
- Matplotlib para visualização 2D/3D

### Matemática Necessária
Você deve ter estudado:
- [x] Pontos, retas e planos (k1-teoria/t1)
- [x] Distâncias e ângulos (k1-teoria/t2)
- [x] Cônicas e superfícies (k1-teoria/t3)
- [x] Coordenadas e transformações (k1-teoria/t4)
- [x] Implementações (k3-implementacao/i1-i4)

---

## 🏗️ Arquitetura do Projeto

```
projeto-navegacao-3d/
├── src/
│   ├── main.c
│   ├── geometry/
│   │   ├── point3d.h/c       # Pontos 3D
│   │   ├── line3d.h/c        # Retas 3D
│   │   ├── plane3d.h/c       # Planos
│   │   ├── sphere.h/c        # Esferas (colisão)
│   │   └── transforms.h/c    # Matrizes 4×4
│   ├── navigation/
│   │   ├── gps.h/c           # Conversão GPS ↔ Cartesiano
│   │   ├── route.h/c         # Roteamento e waypoints
│   │   ├── vehicle.h/c       # Estado do veículo
│   │   └── pathfinding.h/c   # Algoritmos de caminho
│   ├── collision/
│   │   ├── collision.h/c     # Detecção de colisão
│   │   ├── bvh.h/c          # Bounding Volume Hierarchy
│   │   └── raycast.h/c       # Ray casting
│   ├── render/
│   │   ├── camera.h/c        # Câmera 3D
│   │   ├── renderer.h/c      # Renderização OpenGL
│   │   └── ui.h/c            # Interface (ImGui ou custom)
│   └── utils/
│       ├── config.h/c        # Carregamento de configuração
│       └── logger.h/c        # Logging e debug
├── data/
│   ├── routes/
│   │   └── sample_route.gpx  # Rota de exemplo
│   ├── obstacles/
│   │   └── city.json         # Obstáculos (prédios, etc)
│   └── config.json           # Configuração geral
├── Makefile
└── README.md
```

---

## 📝 Etapas de Desenvolvimento

### Etapa 0: Setup e Estruturas Base (45 min)

**Objetivo:** Criar estrutura do projeto e bibliotecas base de geometria

**Checklist:**
- [ ] Criar estrutura de diretórios
- [ ] Configurar Makefile/CMake
- [ ] Implementar Point3D básico
- [ ] Implementar Vec3 (vetores 3D)
- [ ] Testes unitários para geometria básica

**Arquivo: `src/geometry/point3d.h`**
```c
#ifndef POINT3D_H
#define POINT3D_H

typedef struct {
    double x, y, z;
} Point3D;

// Construtores
Point3D point3d_create(double x, double y, double z);
Point3D point3d_zero(void);

// Operações
double point3d_distance(Point3D a, Point3D b);
Point3D point3d_midpoint(Point3D a, Point3D b);
Point3D point3d_vector_between(Point3D from, Point3D to);

// Utilidades
void point3d_print(Point3D p);
int point3d_equals(Point3D a, Point3D b, double epsilon);

#endif
```

**Arquivo: `src/geometry/point3d.c`**
```c
#include "point3d.h"
#include <stdio.h>
#include <math.h>

Point3D point3d_create(double x, double y, double z) {
    Point3D p = {x, y, z};
    return p;
}

Point3D point3d_zero(void) {
    return point3d_create(0.0, 0.0, 0.0);
}

double point3d_distance(Point3D a, Point3D b) {
    double dx = b.x - a.x;
    double dy = b.y - a.y;
    double dz = b.z - a.z;
    return sqrt(dx*dx + dy*dy + dz*dz);
}

Point3D point3d_midpoint(Point3D a, Point3D b) {
    return point3d_create(
        (a.x + b.x) / 2.0,
        (a.y + b.y) / 2.0,
        (a.z + b.z) / 2.0
    );
}

Point3D point3d_vector_between(Point3D from, Point3D to) {
    return point3d_create(
        to.x - from.x,
        to.y - from.y,
        to.z - from.z
    );
}

void point3d_print(Point3D p) {
    printf("(%.3f, %.3f, %.3f)", p.x, p.y, p.z);
}

int point3d_equals(Point3D a, Point3D b, double epsilon) {
    return point3d_distance(a, b) < epsilon;
}
```

**Teste:**
```c
// src/test_geometry.c
#include "geometry/point3d.h"
#include <assert.h>
#include <stdio.h>

int main() {
    Point3D p1 = point3d_create(0, 0, 0);
    Point3D p2 = point3d_create(3, 4, 0);
    
    double dist = point3d_distance(p1, p2);
    assert(fabs(dist - 5.0) < 0.001);
    
    Point3D mid = point3d_midpoint(p1, p2);
    assert(point3d_equals(mid, point3d_create(1.5, 2.0, 0), 0.001));
    
    printf("✅ Testes de geometria básica passaram!\n");
    return 0;
}
```

**Critério de conclusão:**
- [ ] Estrutura compilando
- [ ] Testes básicos passando
- [ ] Makefile funcional

**XP:** 30 XP

---

### Etapa 1: Sistema de Coordenadas GPS (2h)

**Objetivo:** Implementar conversão GPS ↔ Cartesiano Local

**Conceitos:**
- Conversão lat/lon para coordenadas locais (ENU - East-North-Up)
- Projeção aproximada para pequenas áreas
- Precisão e erro de conversão

**Arquivo: `src/navigation/gps.h`**
```c
#ifndef GPS_H
#define GPS_H

#include "../geometry/point3d.h"

typedef struct {
    double latitude;   // Graus
    double longitude;  // Graus
    double altitude;   // Metros
} GPSCoord;

typedef struct {
    GPSCoord reference;  // Ponto de referência (origem)
} GPSConverter;

// Inicialização
GPSConverter gps_create_converter(GPSCoord reference);

// Conversões
Point3D gps_to_local(GPSConverter* conv, GPSCoord gps);
GPSCoord gps_from_local(GPSConverter* conv, Point3D local);

// Cálculos diretos GPS
double gps_distance(GPSCoord a, GPSCoord b);  // Haversine
double gps_bearing(GPSCoord from, GPSCoord to);  // Azimute

#endif
```

**Implementação (haversine):**
```c
double gps_distance(GPSCoord a, GPSCoord b) {
    const double R = 6371000.0;  // Raio da Terra em metros
    
    double lat1 = deg_to_rad(a.latitude);
    double lat2 = deg_to_rad(b.latitude);
    double dlat = deg_to_rad(b.latitude - a.latitude);
    double dlon = deg_to_rad(b.longitude - a.longitude);
    
    double a_val = sin(dlat/2) * sin(dlat/2) +
                   cos(lat1) * cos(lat2) *
                   sin(dlon/2) * sin(dlon/2);
    
    double c = 2 * atan2(sqrt(a_val), sqrt(1-a_val));
    
    return R * c;
}
```

**Teste:**
```c
// Teste com coordenadas conhecidas
GPSCoord sp_paulista = {-23.5505, -46.6333, 800};  // São Paulo
GPSCoord sp_masp = {-23.5475, -46.6361, 800};

double dist = gps_distance(sp_paulista, sp_masp);
// Esperado: ~400m

printf("Distância: %.1f metros\n", dist);
```

**Critério:**
- [ ] Conversão GPS → Local precisa (< 1m erro em 1km)
- [ ] Conversão Local → GPS reversível
- [ ] Haversine correto (testes com coordenadas reais)

**XP:** 60 XP

---

### Etapa 2: Roteamento e Navegação (2-3h)

**Objetivo:** Carregar rotas, calcular desvios, fornecer instruções

**Arquivo: `src/navigation/route.h`**
```c
typedef struct {
    GPSCoord* waypoints;  // Array de waypoints
    int num_waypoints;
    double* distances;    // Distância acumulada até cada waypoint
} Route;

typedef struct {
    Route* route;
    int current_segment;   // Índice do segmento atual
    Point3D position;      // Posição local atual
    double speed;          // m/s
    double heading;        // Graus (0 = Norte)
} NavigationState;

// Carregar rota de arquivo GPX
Route* route_load_gpx(const char* filename);

// Cálculos
double route_deviation(NavigationState* nav);  // Distância perpendicular
double route_correction_angle(NavigationState* nav);  // Ângulo de correção
GPSCoord route_closest_point(NavigationState* nav);  // Ponto mais próximo na rota

// Atualização
void nav_update(NavigationState* nav, double dt);  // Integrar movimento
int nav_reached_waypoint(NavigationState* nav, double tolerance);
```

**Teste:**
```bash
$ ./navegacao data/routes/test_route.gpx

=== Sistema de Navegação ===
Rota carregada: 5 waypoints, 2.3 km total

t=0.0s: Posição (0.0, 0.0, 0.0)
  Velocidade: 10.0 m/s
  Desvio: 0.0 m
  Próximo waypoint: 500 m

t=10.0s: Posição (100.0, 5.0, 0.0)
  Desvio: 5.0 m
  Correção sugerida: Virar 3° à direita
```

**Critério:**
- [ ] Parser GPX funcional
- [ ] Cálculo de desvio correto (< 0.1m erro)
- [ ] Detecção de waypoint alcançado
- [ ] Simulação time-stepped

**XP:** 70 XP

---

### Etapa 3: Visualização 3D Básica (2-3h)

**Objetivo:** Renderizar rota, veículo e terreno em 3D

**Tecnologias:** SDL2 + OpenGL (ou SFML)

**Features:**
- Câmera 3D (first-person ou follow)
- Desenhar rota como linha 3D
- Representar veículo (cubo/seta)
- Grid de referência
- Controles de câmera

**Arquivo: `src/render/camera.h`**
```c
typedef struct {
    Point3D position;
    Point3D target;    // Lookעat
    Point3D up;
    
    double fov;        // Campo de visão (graus)
    double aspect;     // Aspect ratio
    double near, far;  // Planos de corte
} Camera;

void camera_update_view_matrix(Camera* cam, float* out_matrix);
void camera_update_projection_matrix(Camera* cam, float* out_matrix);
void camera_rotate(Camera* cam, float yaw, float pitch);
void camera_move(Camera* cam, Point3D delta);
```

**Renderização básica:**
```c
void render_route(Route* route, GPSConverter* conv) {
    glBegin(GL_LINE_STRIP);
    glColor3f(0.0f, 1.0f, 0.0f);  // Verde
    
    for (int i = 0; i < route->num_waypoints; i++) {
        Point3D local = gps_to_local(conv, route->waypoints[i]);
        glVertex3f(local.x, local.z, -local.y);  // Y-up para Z-up
    }
    
    glEnd();
}
```

**Critério:**
- [ ] Câmera controlável
- [ ] Rota visível em 3D
- [ ] Veículo renderizado
- [ ] Grid de referência

**XP:** 60 XP

---

### Etapa 4: Detecção de Colisões (3-4h)

**Objetivo:** Detectar colisões com obstáculos (esferas, AABBs)

**Arquivo: `src/collision/collision.h`**
```c
typedef struct {
    Point3D center;
    double radius;
} Sphere;

typedef struct {
    Point3D min;
    Point3D max;
} AABB;  // Axis-Aligned Bounding Box

// Detecção
int collision_sphere_sphere(Sphere a, Sphere b);
int collision_sphere_aabb(Sphere s, AABB box);
int collision_ray_sphere(Point3D origin, Point3D dir, Sphere s, double* t_out);

// Resposta
Point3D collision_resolve_sphere_sphere(Sphere* a, Sphere* b);
```

**Teste de ray casting:**
```c
// Lançar raio da câmera para picking
Point3D ray_origin = camera.position;
Point3D ray_dir = camera_get_forward_vector(&camera);

double t;
if (collision_ray_sphere(ray_origin, ray_dir, obstacle_sphere, &t)) {
    Point3D hit_point = point_add(ray_origin, point_scale(ray_dir, t));
    printf("Hit obstáculo em: ");
    point3d_print(hit_point);
    printf("\n");
}
```

**Critério:**
- [ ] Colisão esfera-esfera
- [ ] Ray casting funcional
- [ ] Veículo para ao colidir
- [ ] Visualização de colisões (debug)

**XP:** 80 XP

---

### Etapa 5: Integração Completa (2-3h)

**Objetivo:** Juntar todos os sistemas em simulação completa

**Features:**
- Veículo segue rota automaticamente
- Desvia de obstáculos
- UI mostra estatísticas em tempo real
- Replay de trajetória

**Loop principal:**
```c
while (running) {
    // Input
    handle_input(&input);
    
    // Update
    nav_update(&navigation, dt);
    
    // Colisões
    for (each obstacle) {
        if (collision_detect(vehicle, obstacle)) {
            collision_resolve(&vehicle, &obstacle);
        }
    }
    
    // Auto-navegação
    if (autopilot_enabled) {
        double correction = route_correction_angle(&navigation);
        vehicle.heading += correction * dt;
    }
    
    // Render
    camera_follow_vehicle(&camera, vehicle.position);
    render_scene(&scene, &camera);
    render_ui(&navigation, &vehicle);
    
    SDL_GL_SwapWindow(window);
}
```

**Critério:**
- [ ] Simulação completa rodando
- [ ] Veículo navega autonomamente
- [ ] Colisões funcionam
- [ ] UI informativa

**XP:** 70 XP

---

## 🏆 Critérios de Conclusão (Projeto Base)

- [ ] Todas as etapas 0-5 completas
- [ ] Código bem documentado
- [ ] Pelo menos 3 rotas de teste
- [ ] README com instruções de compilação
- [ ] Screenshots/vídeo de demonstração

**Total XP Base:** 370 XP

---

## 🚀 Features Avançadas (Etapa 6)

### 6.1 Múltiplos Veículos (+50 XP)
Simular vários veículos simultaneamente, evitando colisões entre si.

### 6.2 Obstáculos Dinâmicos (+60 XP)
Obstáculos que se movem (pedestres, outros carros).

### 6.3 Otimização Espacial - BVH (+70 XP)
Implementar Bounding Volume Hierarchy para colisões eficientes.

### 6.4 Exportação para Visualizadores (+40 XP)
Exportar trajetória para KML/KMZ (Google Earth) ou JSON (Mapbox).

### 6.5 Replay de Trajetória (+50 XP)
Gravar e reproduzir trajetórias completas com controle de tempo.

---

## 📚 Recursos e Referências

- **GPS Math:** https://www.movable-type.co.uk/scripts/latlong.html
- **OpenGL Tutorial:** https://learnopengl.com/
- **Collision Detection:** "Real-Time Collision Detection" (Ericson)
- **Navigation:** "Artificial Intelligence for Games" (Millington)

---

## 🎓 O Que Você Aprenderá

Ao completar este projeto, você terá:
- ✅ Domínio de geometria analítica prática
- ✅ Experiência com sistemas de coordenadas complexos
- ✅ Habilidades em gráficos 3D
- ✅ Portfólio impressionante

**Boa sorte e divirta-se construindo!** 🚀
