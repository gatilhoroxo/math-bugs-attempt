# Problemas de Programação: Intermediários

## 🎯 Meta

Integrar geometria analítica em sistemas complexos: ray tracing 2D, navegação GPS, e sistemas de colisão.

---

## ⏱️ Tempo Estimado

- **Problema 1 (Ray Tracer 2D):** 2h-2h45
- **Problema 2 (Sistema de Navegação GPS):** 2h30-3h15
- **Problema 3 (Motor de Colisão 3D):** 2h15-3h
- **Total:** ~7h-9h

---

## 📋 Pré-requisitos

- **Teoria:** `k1-teoria/t1-pontos-retas-planos.md`, `t2-distancias-angulos.md`, `t3-conicas-superficies.md`
- **Exercícios:** `k2-exercicios/` (Níveis 2-3)
- **Implementação:** `k3-implementacao/i1-pontos-retas-planos.md`, `i2-distancias-angulos.md`, `i3-conicas-superficies.md`
- **Prática:** `k4-pratica/p1-basicos.md` (Problema 1 obrigatório)
- **Linguagem:** C/C++ ou Python

---

## 🎚️ Dificuldade

⭐⭐⭐⭐ Avançado

---

## 💪 Sistema de XP

- **Problema 1:** 120 XP
- **Problema 2:** 110 XP  
- **Problema 3:** 110 XP

**XP Total Disponível:** 340 XP

---

## 📊 Rastreamento de Progresso

- [ ] Problema 1: Ray Tracer 2D com Reflexões (0/1) - 120 XP
- [ ] Problema 2: Sistema de Navegação GPS (0/1) - 110 XP
- [ ] Problema 3: Motor de Colisão 3D (0/1) - 110 XP

**XP Conquistado:** ___ / 340 XP

---

## Problema 1: Ray Tracer 2D com Reflexões

### 🎯 Objetivo

Criar um ray tracer 2D que simula luz refletindo em superfícies (círculos e segmentos de reta), com visualização do caminho dos raios.

### 📐 Contexto Teórico

**Ray Tracing:** Técnica que simula comportamento da luz lançando raios da câmera e calculando interseções.

**Lei da Reflexão:**
- Ângulo de incidência = Ângulo de reflexão
- Fórmula vetorial: `R = V - 2(V·N)N`
  - V = raio incidente (normalizado)
  - N = normal da superfície (normalizado)
  - R = raio refletido

**Aplicações:** Renderização realista, simulação de laser, óptica.

### 🛠️ Especificação

**Cena (arquivo `scene.txt`):**
```
# Câmera (posição e direção)
CAMERA 50 300 0 -1

# Espelhos (segmentos de reta)
MIRROR 100 100 300 150
MIRROR 100 400 300 350

# Círculos refletivos
CIRCLE 400 250 50

# Alvos (não refletem, mas absorvem)
TARGET 500 200 20
```

**Funcionalidades obrigatórias:**

1. **Parser de cena:** Ler objetos do arquivo
2. **Ray casting:**
   - Lançar raio da câmera
   - Encontrar primeira interseção com objetos
   - Se espelho: calcular reflexão e continuar
   - Se alvo: marcar como "hit" e parar
   - Limite de reflexões (ex: 10)
3. **Renderização:**
   - Desenhar cena (SDL/SFML/PIL)
   - Mostrar caminho completo do raio (segmentos coloridos)
   - Destacar pontos de interseção
4. **Estatísticas:**
   - Número de reflexões
   - Distância total percorrida
   - Tempo de execução

### 📝 Pseudocódigo

```
ESTRUTURA Raio:
    origem: Vec2
    direcao: Vec2  // Unitário

ESTRUTURA HitInfo:
    tem_hit: booleano
    ponto: Vec2
    normal: Vec2
    distancia: número
    objeto: referência

FUNÇÃO tracar_raio(raio, objetos, max_reflexoes):
    caminho = LISTA_VAZIA
    raio_atual = raio
    
    PARA i DE 0 ATÉ max_reflexoes:
        // Encontrar interseção mais próxima
        hit_mais_proximo = NULL
        t_minimo = INFINITO
        
        PARA CADA obj EM objetos:
            hit = intersecao_raio_objeto(raio_atual, obj)
            
            SE hit.tem_hit E hit.distancia < t_minimo E hit.distancia > 0.001:
                t_minimo = hit.distancia
                hit_mais_proximo = hit
        
        SE hit_mais_proximo == NULL:
            // Raio escapa para o infinito
            QUEBRAR
        
        // Adicionar segmento ao caminho
        ponto_fim = raio_atual.origem + raio_atual.direcao * hit_mais_proximo.distancia
        ADICIONAR (raio_atual.origem, ponto_fim) A caminho
        
        // Verificar tipo de objeto
        SE hit_mais_proximo.objeto.tipo == ALVO:
            IMPRIMIR "Raio atingiu alvo!"
            QUEBRAR
        
        SENÃO SE hit_mais_proximo.objeto.tipo == ESPELHO:
            // Calcular reflexão
            raio_refletido = refletir_raio(raio_atual.direcao, hit_mais_proximo.normal)
            
            // Próximo raio começa no ponto de hit
            raio_atual = Raio(hit_mais_proximo.ponto, raio_refletido)
    
    RETORNAR caminho

FUNÇÃO refletir_raio(direcao, normal):
    // R = V - 2(V·N)N
    // (Assumindo V e N normalizados)
    dot = produto_escalar(direcao, normal)
    RETORNAR direcao - 2 * dot * normal

FUNÇÃO intersecao_raio_segmento(raio, seg_inicio, seg_fim):
    """Interseção raio com segmento de reta"""
    // Raio: P(t) = O + t*D
    // Segmento: Q(s) = A + s*(B-A), s ∈ [0,1]
    // Resolver: O + t*D = A + s*(B-A)
    
    v = seg_fim - seg_inicio
    w = raio.origem - seg_inicio
    
    // Sistema 2x2 usando determinantes
    d_cross_v = cruz_2d(raio.direcao, v)
    
    SE abs(d_cross_v) < EPSILON:
        RETORNAR SEM_HIT  // Paralelos
    
    t = cruz_2d(w, v) / d_cross_v
    s = cruz_2d(w, raio.direcao) / d_cross_v
    
    SE t > 0 E s >= 0 E s <= 1:
        ponto_hit = raio.origem + t * raio.direcao
        
        // Normal ao segmento (perpendicular)
        tangente = normalizar(v)
        normal = Vec2(-tangente.y, tangente.x)
        
        // Garantir normal aponta para o raio
        SE produto_escalar(normal, -raio.direcao) < 0:
            normal = -normal
        
        RETORNAR HitInfo(VERDADEIRO, ponto_hit, normal, t)
    
    RETORNAR SEM_HIT

FUNÇÃO intersecao_raio_circulo(raio, centro, raio_circulo):
    """Interseção raio com círculo"""
    // ||(O + t*D) - C||² = r²
    
    oc = raio.origem - centro
    
    a = produto_escalar(raio.direcao, raio.direcao)  // =1 se normalizado
    b = 2 * produto_escalar(oc, raio.direcao)
    c = produto_escalar(oc, oc) - raio_circulo²
    
    discriminante = b² - 4*a*c
    
    SE discriminante < 0:
        RETORNAR SEM_HIT
    
    t1 = (-b - sqrt(discriminante)) / (2*a)
    t2 = (-b + sqrt(discriminante)) / (2*a)
    
    // Pegar primeiro t positivo
    t = SE t1 > 0 ENTÃO t1 SENÃO t2
    
    SE t <= 0:
        RETORNAR SEM_HIT
    
    ponto_hit = raio.origem + t * raio.direcao
    normal = normalizar(ponto_hit - centro)
    
    RETORNAR HitInfo(VERDADEIRO, ponto_hit, normal, t)

// Produto vetorial 2D (retorna escalar)
FUNÇÃO cruz_2d(v1, v2):
    RETORNAR v1.x * v2.y - v1.y * v2.x
```

### 🧪 Casos de Teste

**Teste 1: Reflexão simples em espelho vertical**
```
CAMERA 100 250 1 0
MIRROR 300 100 300 400
TARGET 500 250 10
```
Esperado: 2 segmentos (câmera→espelho, espelho→alvo), 1 reflexão

**Teste 2: Múltiplas reflexões (ping-pong)**
```
CAMERA 50 250 1 0
MIRROR 200 100 200 400  # Espelho esquerdo
MIRROR 400 100 400 400  # Espelho direito
TARGET 300 250 10
```
Esperado: ≥3 reflexões antes de atingir alvo

**Teste 3: Raio escapa**
```
CAMERA 100 100 1 1
CIRCLE 300 300 50
```
Esperado: 0 hits, raio vai para infinito

### 🎨 Visualização

Usar biblioteca gráfica (SDL2, SFML, ou Python PIL/matplotlib):

```python
# Exemplo Python com matplotlib
import matplotlib.pyplot as plt

def desenhar_cena(objetos, caminho_raio):
    fig, ax = plt.subplots(figsize=(10, 8))
    
    # Desenhar espelhos
    for espelho in objetos['espelhos']:
        ax.plot([espelho.p1.x, espelho.p2.x], 
                [espelho.p1.y, espelho.p2.y], 
                'b-', linewidth=3, label='Espelho')
    
    # Desenhar círculos
    for circulo in objetos['circulos']:
        circle = plt.Circle((circulo.centro.x, circulo.centro.y), 
                            circulo.raio, fill=False, color='cyan')
        ax.add_patch(circle)
    
    # Desenhar caminho do raio
    for i, (inicio, fim) in enumerate(caminho_raio):
        cor = plt.cm.hot(i / len(caminho_raio))
        ax.plot([inicio.x, fim.x], [inicio.y, fim.y], 
                color=cor, linewidth=2, alpha=0.7)
        ax.plot(inicio.x, inicio.y, 'ro', markersize=5)
    
    ax.set_xlim(0, 600)
    ax.set_ylim(0, 500)
    ax.set_aspect('equal')
    plt.grid(True, alpha=0.3)
    plt.show()
```

### 🏆 Critérios de Conclusão

- [ ] Parser de cena funcional
- [ ] Interseção raio-segmento correta
- [ ] Interseção raio-círculo correta
- [ ] Reflexões calculadas corretamente (Lei da Reflexão)
- [ ] Visualização mostra caminho completo
- [ ] Limite de reflexões implementado
- [ ] Pelo menos 3 cenas de teste

**XP Concedido:** 120 XP

---

## Problema 2: Sistema de Navegação GPS

### 🎯 Objetivo

Criar sistema de navegação que:
1. Carrega rota (lista de waypoints GPS)
2. Simula movimento de veículo
3. Calcula desvio da rota em tempo real
4. Fornece instruções de correção

### 📐 Contexto Teórico

**Conversão GPS → Cartesiano Local:**
- Raio Terra ≈ 6371 km
- Para pequenas distâncias: projeção plana
- `ΔN = R * Δlat` (norte)
- `ΔE = R * Δlon * cos(lat)` (leste)

**Navegação:**
- **Distância perpendicular:** Quão longe da rota?
- **Direção de correção:** Ângulo para voltar à rota
- **Próximo waypoint:** Quando considerar alcançado?

### 🛠️ Especificação

**Entrada (arquivo `route.gpx` simplificado):**
```xml
<waypoint lat="40.7128" lon="-74.0060" />  <!-- NYC -->
<waypoint lat="40.7580" lon="-73.9855" />  <!-- Times Square -->
<waypoint lat="40.7614" lon="-73.9776" />  <!-- Central Park -->
```

**Simulação:**
```
Tempo: 0s
Posição atual: (40.7128, -74.0060)
Próximo waypoint: Times Square (4.5 km)
Desvio da rota: 0.0 m
Velocidade: 15 m/s (54 km/h)

[Simular vento lateral ou erro GPS]

Tempo: 30s
Posição atual: (40.7195, -73.9985)
Próximo waypoint: Times Square (3.8 km)
Desvio da rota: 25.3 m
Correção sugerida: Virar 8° à direita
```

**Funcionalidades obrigatórias:**

1. **Parser GPX:** Ler waypoints (lat, lon)
2. **Conversão coordenadas:**
   - GPS → Cartesiano local
   - Cartesiano → GPS
3. **Cálculos de navegação:**
   - Distância perpendicular à rota
   - Ponto mais próximo no segmento atual
   - Ângulo de correção (bearing)
   - Distância até próximo waypoint
4. **Simulador:**
   - Movimento do veículo (velocidade configurável)
   - Perturbações (vento, erro GPS)
   - Atualização periódica (ex: 1s)
5. **Detecção de waypoint alcançado:**
   - Raio de tolerância (ex: 20m)
   - Avançar para próximo segmento

### 📝 Pseudocódigo

```
CONSTANTES:
    RAIO_TERRA = 6371000.0  // metros
    TOLERANCIA_WAYPOINT = 20.0  // metros
    DT = 1.0  // timestep em segundos

FUNÇÃO gps_para_local(lat, lon, lat_ref, lon_ref):
    dlat = graus_para_rad(lat - lat_ref)
    dlon = graus_para_rad(lon - lon_ref)
    
    lat_media = graus_para_rad((lat + lat_ref) / 2)
    
    y = RAIO_TERRA * dlat  // Norte
    x = RAIO_TERRA * dlon * cos(lat_media)  // Leste
    
    RETORNAR (x, y)

FUNÇÃO local_para_gps(x, y, lat_ref, lon_ref):
    lat_media = graus_para_rad(lat_ref)
    
    dlat = y / RAIO_TERRA
    dlon = x / (RAIO_TERRA * cos(lat_media))
    
    lat = lat_ref + rad_para_graus(dlat)
    lon = lon_ref + rad_para_graus(dlon)
    
    RETORNAR (lat, lon)

FUNÇÃO calcular_desvio_rota(pos_atual, seg_inicio, seg_fim):
    """Calcula distância perpendicular e ponto de retorno"""
    // Converter para cartesiano local
    p = gps_para_local(pos_atual.lat, pos_atual.lon, seg_inicio.lat, seg_inicio.lon)
    a = (0, 0)  // Início do segmento na origem
    b = gps_para_local(seg_fim.lat, seg_fim.lon, seg_inicio.lat, seg_inicio.lon)
    
    // Vetor do segmento
    ab = b - a
    ap = p - a
    
    // Projeção de p em ab
    t = produto_escalar(ap, ab) / produto_escalar(ab, ab)
    
    // Clampar ao segmento
    t = max(0, min(1, t))
    
    // Ponto mais próximo
    ponto_rota = a + t * ab
    
    // Distância perpendicular
    desvio = distancia(p, ponto_rota)
    
    // Converter ponto de volta para GPS
    (lat_volta, lon_volta) = local_para_gps(ponto_rota.x, ponto_rota.y, 
                                             seg_inicio.lat, seg_inicio.lon)
    
    RETORNAR (desvio, lat_volta, lon_volta, t)

FUNÇÃO calcular_bearing(pos_atual, destino):
    """Calcula direção (azimute) de pos_atual para destino"""
    dlat = graus_para_rad(destino.lat - pos_atual.lat)
    dlon = graus_para_rad(destino.lon - pos_atual.lon)
    
    lat1 = graus_para_rad(pos_atual.lat)
    lat2 = graus_para_rad(destino.lat)
    
    y = sin(dlon) * cos(lat2)
    x = cos(lat1)*sin(lat2) - sin(lat1)*cos(lat2)*cos(dlon)
    
    bearing = atan2(y, x)
    
    RETORNAR rad_para_graus(bearing)  // 0° = Norte, 90° = Leste

FUNÇÃO simular_passo(veiculo, rota, dt):
    // Movimento ideal (seguindo direção atual)
    dist_passo = veiculo.velocidade * dt
    
    // Adicionar perturbação (vento lateral)
    perturbacao = random(-5, 5)  // metros perpendiculares
    
    // Calcular nova posição
    direcao = calcular_bearing(veiculo.pos, rota.waypoints[veiculo.idx_waypoint_atual + 1])
    
    // Mover na direção + perturbação
    // ... (trigonometria para atualizar lat/lon)
    
    // Verificar se alcançou waypoint
    dist_waypoint = distancia_gps(veiculo.pos, rota.waypoints[veiculo.idx_waypoint_atual + 1])
    
    SE dist_waypoint < TOLERANCIA_WAYPOINT:
        veiculo.idx_waypoint_atual += 1
        IMPRIMIR "Waypoint alcançado!"
```

### 🧪 Teste

Rota teste (São Paulo):
```
Waypoint 1: (-23.5505, -46.6333)  # Paulista
Waypoint 2: (-23.5475, -46.6361)  # MASP
Waypoint 3: (-23.5489, -46.6388)  # Consolação
```

Simular veículo a 10 m/s com vento lateral de 2 m/s:

```
=== Simulação de Navegação ===
Rota: 3 waypoints, ~800m total

t=0s: Posição (-23.5505, -46.6333), Desvio 0.0m
t=10s: Posição (-23.5498, -46.6342), Desvio 12.5m, Correção: 5° direita
t=20s: Posição (-23.5488, -46.6350), Desvio 8.2m, Correção: 3° direita
t=30s: WAYPOINT 2 alcançado!
...
```

### 🏆 Critérios de Conclusão

- [ ] Parser GPX funcional
- [ ] Conversão GPS ↔ Cartesiano precisa
- [ ] Cálculo de desvio correto
- [ ] Bearing calculado corretamente
- [ ] Simulação roda em tempo real
- [ ] Waypoints detectados corretamente
- [ ] Logs informativos

**XP Concedido:** 110 XP

---

## Problema 3: Motor de Colisão 3D

### 🎯 Objetivo

Implementar sistema de detecção e resposta a colisões entre esferas e planos em 3D.

### 📐 Contexto Teórico

**Detecção esfera-esfera:**
- Colisão se `||C₁ - C₂|| ≤ r₁ + r₂`

**Detecção esfera-plano:**
- Distância: `d = |n·C + D| / ||n||`
- Colisão se `d ≤ raio`

**Resposta (física elástica):**
- Separar objetos (resolver penetração)
- Calcular impulso baseado em massas e coef. restituição
- Atualizar velocidades

### 🛠️ Especificação

**Entidades:**
```c
typedef struct {
    Vec3 centro;
    double raio;
    Vec3 velocidade;
    double massa;
} Esfera;

typedef struct {
    Vec3 normal;  // Unitário
    double d;     // Termo constante
    double restitution;  // Coef. restituição (0=inelástica, 1=elástica)
} Plano;
```

**Funcionalidades:**
- Detectar colisão esfera-esfera
- Detectar colisão esfera-plano
- Resolver penetração (separar objetos)
- Calcular impulso de colisão
- Loop de simulação com física básica

### 📝 Pseudocódigo

```
FUNÇÃO detectar_colisao_esferas(e1, e2):
    dist = magnitude(e1.centro - e2.centro)
    RETORNAR dist <= (e1.raio + e2.raio)

FUNÇÃO resolver_colisao_esferas(e1, e2, restitution):
    // Normal de colisão
    normal = normalizar(e2.centro - e1.centro)
    
    // Penetração
    dist = magnitude(e2.centro - e1.centro)
    penetracao = (e1.raio + e2.raio) - dist
    
    // Separar (metade cada, proporcional a massas)
    separacao1 = normal * penetracao * (e2.massa / (e1.massa + e2.massa))
    separacao2 = -normal * penetracao * (e1.massa / (e1.massa + e2.massa))
    
    e1.centro += separacao1
    e2.centro += separacao2
    
    // Velocidade relativa
    vel_rel = e1.velocidade - e2.velocidade
    vel_ao_longo_normal = produto_escalar(vel_rel, normal)
    
    // Já se afastando? Não fazer nada
    SE vel_ao_longo_normal > 0:
        RETORNAR
    
    // Impulso
    j = -(1 + restitution) * vel_ao_longo_normal
    j /= (1/e1.massa + 1/e2.massa)
    
    impulso = j * normal
    
    e1.velocidade += impulso / e1.massa
    e2.velocidade -= impulso / e2.massa

FUNÇÃO detectar_colisao_esfera_plano(esfera, plano):
    dist_sinalizada = produto_escalar(plano.normal, esfera.centro) + plano.d
    dist = abs(dist_sinalizada)
    
    RETORNAR (dist <= esfera.raio, dist_sinalizada)

FUNÇÃO resolver_colisao_esfera_plano(esfera, plano, dist_sinalizada):
    // Posicionar esfera fora do plano
    penetracao = esfera.raio - abs(dist_sinalizada)
    
    SE penetracao > 0:
        // Sinal de dist determina lado do plano
        direcao = SE dist_sinalizada < 0 ENTÃO -plano.normal SENÃO plano.normal
        esfera.centro += direcao * penetracao
    
    // Refletir velocidade
    vel_normal = produto_escalar(esfera.velocidade, plano.normal)
    
    SE vel_normal < 0:  // Indo em direção ao plano
        esfera.velocidade -= (1 + plano.restitution) * vel_normal * plano.normal
```

### 🧪 Teste

```c
// Teste 1: Esfera cai e quica no chão
Esfera bola = {centro: (0, 10, 0), raio: 1, vel: (0, 0, 0), massa: 1};
Plano chao = {normal: (0, 1, 0), d: 0, restitution: 0.8};

// Simular por 3 segundos
for (t = 0; t < 3.0; t += 0.016) {
    // Gravidade
    bola.velocidade.y -= 9.8 * 0.016;
    
    // Integrar
    bola.centro += bola.velocidade * 0.016;
    
    // Colisão
    if (detectar_colisao_esfera_plano(bola, chao)) {
        resolver_colisao_esfera_plano(bola, chao);
    }
    
    printf("t=%.2f: y=%.2f, vy=%.2f\n", t, bola.centro.y, bola.velocidade.y);
}

// Esperado: Quicar cada vez mais baixo (perda de energia)
```

### 🏆 Critérios de Conclusão

- [ ] Detecção esfera-esfera
- [ ] Detecção esfera-plano
- [ ] Resolução física correta
- [ ] Simulação de quique funcional
- [ ] Pelo menos 2 testes validados

**XP Concedido:** 110 XP

---

## 🔗 Próximos Passos

- `p3-avancados.md` → Ray tracer 3D completo, física avançada
- `k5-projeto/` → Sistema de navegação com visualização 3D

---

## 📚 Recursos

- Real-Time Collision Detection (Ericson)
- Game Physics Engine Development (Millington)
- Scratchapixel - Ray Tracing
