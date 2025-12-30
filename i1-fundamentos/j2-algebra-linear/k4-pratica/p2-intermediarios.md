# Problemas de Programação: Intermediários

## 🎯 Meta

Integrar álgebra linear em sistemas complexos: física 2D, ray casting, e detecção de colisões.

---

## ⏱️ Tempo Estimado

- **Problema 1 (Physics Engine):** 2h30-3h30
- **Problema 2 (Ray Casting):** 2h-2h45
- **Problema 3 (Collision Detection):** 2h15-3h
- **Total:** ~7h-9h15

---

## 📋 Pré-requisitos

- **Teoria:** `k1-teoria/t1-vetores-espacos.md`, `t2-transformacoes-matrizes.md`
- **Exercícios:** `k2-exercicios/e1-vetores-exercicios.md`, `e2-transformacoes-exercicios.md` (Níveis 2-3)
- **Implementação:** `k3-implementacao/codigo/i1-vetores.md`, `i2-matrizes.md`, `i3-transformacoes.md`
- **Prática:** `k4-pratica/p1-basicos.md` (Problema 1 obrigatório)
- **Linguagem:** C/C++ ou Python

---

## 🎚️ Dificuldade

⭐⭐⭐⭐ Avançado

---

## 💪 Sistema de XP

- **Problema 1:** 120 XP
- **Problema 2:** 110 XP  
- **Problema 3:** 130 XP

**XP Total Disponível:** 360 XP

---

## 📊 Rastreamento de Progresso

- [ ] Problema 1: Physics Engine 2D (0/1) - 120 XP
- [ ] Problema 2: Ray Casting System (0/1) - 110 XP
- [ ] Problema 3: Collision Detection (0/1) - 130 XP

**XP Conquistado:** ___ / 360 XP

---

## Problema 1: Physics Engine 2D

### 🎯 Objetivo

Criar motor de física baseado em vetores para simular movimento, forças, e colisões elásticas em 2D.

### 📐 Contexto Teórico

**Segunda Lei de Newton:** F = ma → a = F/m

**Equações de movimento:**
- Aceleração: a = ΣF / m
- Velocidade: v(t+Δt) = v(t) + a·Δt
- Posição: p(t+Δt) = p(t) + v·Δt + 0.5·a·Δt²

**Forças comuns:**
- **Gravidade:** F_g = (0, -9.8·m)
- **Atrito:** F_f = -μ·v̂ (oposta à velocidade)
- **Mola:** F_s = -k·(x - x₀) (Lei de Hooke)

**Colisão elástica (conserva momento e energia):**
```
v1' = v1 - 2m2/(m1+m2) · ((v1-v2)·n) · n
v2' = v2 - 2m1/(m1+m2) · ((v2-v1)·n) · n
```
onde n é o vetor normal no ponto de colisão.

### 🛠️ Especificação

**Estruturas de dados:**

```c
typedef struct {
    Vec2 posicao;
    Vec2 velocidade;
    Vec2 aceleracao;
    double massa;
    double raio;  // Para colisões circulares
    Vec2 cor;     // Para renderização
} Particula;

typedef struct {
    Vec2 direcao;
    double magnitude;
} Forca;
```

**Funcionalidades obrigatórias:**

**Sistema de partículas:**
- Criar/destruir partículas
- Aplicar força a uma partícula
- Atualizar física (integração Euler ou Verlet)
- Detectar e resolver colisões partícula-partícula
- Detectar e resolver colisões partícula-borda

**Forças implementadas:**
- Gravidade (constante para baixo)
- Arrasto/atrito do ar (proporcional a v²)
- Mola (conectar duas partículas)
- Força manual (aplicada pelo usuário)

**Simulação:**
- Loop principal com timestep fixo (ex: 60 FPS)
- Acumular forças → calcular aceleração → integrar movimento
- Verificar e resolver colisões
- Renderizar estado atual (opcional: SDL/SFML)

**Interface:**
```
Comandos:
  [SPACE] - Adicionar partícula na posição do mouse
  [G] - Ativar/desativar gravidade
  [D] - Ativar/desativar arrasto
  [R] - Reset (limpar todas partículas)
  [Click] - Aplicar força explosiva no ponto
```

### 📝 Pseudocódigo

```
CONSTANTES:
    GRAVIDADE = Vec2(0, -9.8)
    COEF_ARRASTO = 0.1
    COEF_RESTITUICAO = 0.8  // Elasticidade (1=perfeitamente elástica)
    DT = 1.0 / 60.0          // Timestep (60 FPS)

FUNÇÃO aplicar_forca(particula, forca):
    particula.aceleracao += forca / particula.massa

FUNÇÃO aplicar_gravidade(particula):
    forca_g = GRAVIDADE * particula.massa
    aplicar_forca(particula, forca_g)

FUNÇÃO aplicar_arrasto(particula):
    SE magnitude(particula.velocidade) > 0.01:
        // Forca de arrasto proporcional a v²
        direcao = normalizar(particula.velocidade)
        magnitude_arrasto = COEF_ARRASTO * magnitude²(particula.velocidade)
        forca_arrasto = -direcao * magnitude_arrasto
        
        aplicar_forca(particula, forca_arrasto)

FUNÇÃO integrar_movimento(particula, dt):
    """Integração Euler semi-implícita"""
    // Atualizar velocidade
    particula.velocidade += particula.aceleracao * dt
    
    // Atualizar posição
    particula.posicao += particula.velocidade * dt
    
    // Resetar aceleração para próximo frame
    particula.aceleracao = Vec2(0, 0)

FUNÇÃO resolver_colisao_particulas(p1, p2):
    """Colisão elástica entre duas partículas circulares"""
    delta = p2.posicao - p1.posicao
    distancia = magnitude(delta)
    
    // Verificar se há sobreposição
    SE distancia < (p1.raio + p2.raio):
        // Vetor normal (direção da colisão)
        normal = normalizar(delta)
        
        // Separar partículas (correção de posição)
        sobreposicao = (p1.raio + p2.raio) - distancia
        p1.posicao -= normal * (sobreposicao / 2)
        p2.posicao += normal * (sobreposicao / 2)
        
        // Velocidade relativa
        vel_relativa = p1.velocidade - p2.velocidade
        vel_ao_longo_normal = produto_escalar(vel_relativa, normal)
        
        // Só resolver se partículas estão se aproximando
        SE vel_ao_longo_normal > 0:
            RETORNAR
        
        // Calcular impulso
        e = COEF_RESTITUICAO
        impulso = -(1 + e) * vel_ao_longo_normal
        impulso /= (1/p1.massa + 1/p2.massa)
        
        // Aplicar impulso
        impulso_vec = normal * impulso
        p1.velocidade += impulso_vec / p1.massa
        p2.velocidade -= impulso_vec / p2.massa

FUNÇÃO resolver_colisao_bordas(particula, largura, altura):
    """Colisão com paredes da caixa"""
    // Parede esquerda
    SE particula.posicao.x - particula.raio < 0:
        particula.posicao.x = particula.raio
        particula.velocidade.x *= -COEF_RESTITUICAO
    
    // Parede direita
    SE particula.posicao.x + particula.raio > largura:
        particula.posicao.x = largura - particula.raio
        particula.velocidade.x *= -COEF_RESTITUICAO
    
    // Parede inferior
    SE particula.posicao.y - particula.raio < 0:
        particula.posicao.y = particula.raio
        particula.velocidade.y *= -COEF_RESTITUICAO
    
    // Parede superior
    SE particula.posicao.y + particula.raio > altura:
        particula.posicao.y = altura - particula.raio
        particula.velocidade.y *= -COEF_RESTITUICAO

FUNÇÃO loop_principal():
    particulas = lista_vazia()
    tempo_acumulado = 0
    
    ENQUANTO executando:
        // Processar entrada
        processar_eventos()
        
        // Física em timestep fixo
        tempo_acumulado += tempo_frame
        
        ENQUANTO tempo_acumulado >= DT:
            // Aplicar forças
            PARA CADA p EM particulas:
                aplicar_gravidade(p)
                aplicar_arrasto(p)
            
            // Integrar movimento
            PARA CADA p EM particulas:
                integrar_movimento(p, DT)
            
            // Resolver colisões
            PARA i DE 0 ATÉ tamanho(particulas)-1:
                PARA j DE i+1 ATÉ tamanho(particulas)-1:
                    resolver_colisao_particulas(particulas[i], particulas[j])
                
                resolver_colisao_bordas(particulas[i], LARGURA, ALTURA)
            
            tempo_acumulado -= DT
        
        // Renderizar
        limpar_tela()
        PARA CADA p EM particulas:
            desenhar_circulo(p.posicao, p.raio, p.cor)
        atualizar_tela()
```

### 🧪 Testes

**Teste 1: Queda livre**
```c
Particula p = criar_particula(100, 500, massa=1.0);
// Após 1 segundo: y ≈ 500 - 0.5*9.8*1² = 495.1
simular(60 frames);
assert(fabs(p.posicao.y - 495.1) < 0.5);
```

**Teste 2: Colisão elástica perfeita**
```c
Particula p1 = criar_particula(100, 100, vel=(10, 0), massa=1.0);
Particula p2 = criar_particula(150, 100, vel=(0, 0), massa=1.0);
// Massas iguais → velocidades trocam
resolver_colisao(&p1, &p2);
assert(vec2_equals(p1.velocidade, (0, 0), 0.1));
assert(vec2_equals(p2.velocidade, (10, 0), 0.1));
```

**Teste 3: Conservação de energia (sem gravidade/arrasto)**
```c
// Criar 10 partículas com velocidades aleatórias
energia_inicial = calcular_energia_total();
simular(1000 frames);
energia_final = calcular_energia_total();
assert(fabs(energia_final - energia_inicial) / energia_inicial < 0.05);
```

**Teste 4: Bounce nas paredes**
```c
Particula p = criar_particula(10, 100, vel=(-5, 0));
resolver_colisao_bordas(&p, LARGURA=800, ALTURA=600);
// Velocidade inverte e reduz (coef=0.8)
assert(p.velocidade.x > 0 && p.velocidade.x < 5);
```

### 💡 Insights

- **Timestep fixo:** Evita instabilidade em física (dt variável causa bugs)
- **Semi-implicit Euler:** v antes de p (mais estável que Euler explícito)
- **Separação de colisões:** Corrigir posição antes de impulso (evita "stick")
- **Broad-phase:** Para muitas partículas, usar spatial hashing

### 🎯 Desafios Extras (+40 XP cada)

1. **Verlet Integration:** Substituir Euler por Verlet (mais preciso para conservação de energia)
2. **Molas conectadas:** Sistema de partículas conectadas por molas (cloth simulation básico)
3. **Formas não-circulares:** Colisão AABB (axis-aligned bounding box)
4. **Constraint solver:** Manter distância fixa entre pares (ex: corrente rígida)

---

## Problema 2: Ray Casting System

### 🎯 Objetivo

Implementar sistema de ray casting 2D para calcular interseções raio-segmento, criar campo de visão, e detectar visibilidade.

### 📐 Contexto Teórico

**Raio:** Origem + direção parametrizada
```
r(t) = origem + t · direção, t ≥ 0
```

**Interseção raio-segmento:**
Segmento: `s(u) = A + u(B-A), u ∈ [0,1]`

Resolver: `origem + t·dir = A + u(B-A)`

**Campo de visão (FOV):**
- Lançar raios em ângulos uniformes
- Encontrar ponto de interseção mais próximo para cada raio
- Conectar pontos para formar polígono de visibilidade

**Aplicações:**
- Jogos 2D (linha de visão de inimigos)
- Iluminação (shadows)
- Pathfinding (visibilidade entre pontos)

### 🛠️ Especificação

**Estruturas:**

```c
typedef struct {
    Vec2 origem;
    Vec2 direcao;  // Normalizado
} Raio;

typedef struct {
    Vec2 A, B;  // Pontos extremos
} Segmento;

typedef struct {
    int hit;       // Booleano: houve interseção?
    Vec2 ponto;    // Ponto de interseção
    double t;      // Parâmetro do raio
    double distancia;  // Distância da origem ao ponto
} ResultadoIntersecao;
```

**Funcionalidades:**

**Interseções:**
- `intersecao_raio_segmento(Raio r, Segmento s)` → Resultado
- `intersecao_raio_circulo(Raio r, Vec2 centro, double raio)` → Resultado
- `ponto_mais_proximo_em_raio(Raio r, Vec2 ponto)` → Vec2

**Campo de visão:**
- `calcular_fov(Vec2 origem, double angulo_visao, double alcance_max, Segmento[] obstaculos)` → Vec2[] (polígono)
- `esta_visivel(Vec2 origem, Vec2 alvo, Segmento[] obstaculos)` → Booleano

**Visualização:**
- Desenhar raios (linhas coloridas)
- Destacar pontos de interseção
- Preencher polígono de visibilidade (transparente)

### 📝 Pseudocódigo

```
FUNÇÃO intersecao_raio_segmento(raio, segmento):
    """
    Raio: P = raio.origem + t * raio.direcao (t >= 0)
    Segmento: Q = A + u * (B - A) (0 <= u <= 1)
    Resolver: P = Q
    """
    
    o = raio.origem
    d = raio.direcao
    A = segmento.A
    B = segmento.B
    
    v1 = o - A
    v2 = B - A
    v3 = Vec2(-d.y, d.x)  // Perpendicular a d
    
    // Produtos escalares
    dot_v2_v3 = produto_escalar(v2, v3)
    
    // Verificar se raio é paralelo ao segmento
    SE abs(dot_v2_v3) < EPSILON:
        RETORNAR {hit: falso}
    
    // Calcular parâmetros
    t = produto_escalar_cruzado(v2, v1) / dot_v2_v3
    u = produto_escalar(v1, v3) / dot_v2_v3
    
    // Verificar se interseção está nos limites válidos
    SE t >= 0 E u >= 0 E u <= 1:
        ponto = o + d * t
        RETORNAR {
            hit: verdadeiro,
            ponto: ponto,
            t: t,
            distancia: t
        }
    
    RETORNAR {hit: falso}

FUNÇÃO produto_escalar_cruzado(a, b):
    """Componente Z do produto vetorial 2D"""
    RETORNAR a.x * b.y - a.y * b.x

FUNÇÃO calcular_fov(origem, angulo_visao, alcance_max, obstaculos):
    """
    Calcula polígono de campo de visão.
    angulo_visao: ângulo total em radianos (ex: π para 180°)
    """
    
    NUM_RAIOS = 360  // Resolução
    pontos_fov = lista_vazia()
    
    angulo_inicio = -angulo_visao / 2
    incremento = angulo_visao / NUM_RAIOS
    
    PARA i DE 0 ATÉ NUM_RAIOS:
        angulo = angulo_inicio + i * incremento
        
        // Criar raio nessa direção
        direcao = Vec2(cos(angulo), sin(angulo))
        raio = Raio{origem, direcao}
        
        // Encontrar interseção mais próxima
        distancia_min = alcance_max
        ponto_hit = origem + direcao * alcance_max  // Default: fim do alcance
        
        PARA CADA obstaculo EM obstaculos:
            resultado = intersecao_raio_segmento(raio, obstaculo)
            
            SE resultado.hit E resultado.distancia < distancia_min:
                distancia_min = resultado.distancia
                ponto_hit = resultado.ponto
        
        adicionar(pontos_fov, ponto_hit)
    
    RETORNAR pontos_fov

FUNÇÃO esta_visivel(origem, alvo, obstaculos):
    """Verifica se há linha de visão direta"""
    
    direcao = normalizar(alvo - origem)
    distancia_alvo = magnitude(alvo - origem)
    raio = Raio{origem, direcao}
    
    PARA CADA obstaculo EM obstaculos:
        resultado = intersecao_raio_segmento(raio, obstaculo)
        
        // Se algum obstáculo bloqueia antes do alvo
        SE resultado.hit E resultado.distancia < distancia_alvo - EPSILON:
            RETORNAR falso
    
    RETORNAR verdadeiro

FUNÇÃO intersecao_raio_circulo(raio, centro, raio_circulo):
    """Interseção raio-círculo (retorna ponto mais próximo)"""
    
    // Vetor da origem do raio ao centro
    oc = raio.origem - centro
    
    // Coeficientes da equação quadrática ||o + td - c||² = r²
    a = produto_escalar(raio.direcao, raio.direcao)  // = 1 se normalizado
    b = 2.0 * produto_escalar(oc, raio.direcao)
    c = produto_escalar(oc, oc) - raio_circulo²
    
    discriminante = b² - 4*a*c
    
    SE discriminante < 0:
        RETORNAR {hit: falso}  // Sem interseção
    
    // Calcular t (queremos a raiz menor, ponto mais próximo)
    t = (-b - raiz_quadrada(discriminante)) / (2*a)
    
    SE t < 0:
        RETORNAR {hit: falso}  // Interseção atrás da origem
    
    ponto = raio.origem + raio.direcao * t
    
    RETORNAR {
        hit: verdadeiro,
        ponto: ponto,
        t: t,
        distancia: t
    }
```

### 🧪 Testes

**Teste 1: Interseção básica**
```c
Raio r = {origem: (0, 0), direcao: (1, 0)};
Segmento s = {A: (5, -1), B: (5, 1)};
Resultado res = intersecao_raio_segmento(r, s);
assert(res.hit == true);
assert(vec2_equals(res.ponto, (5, 0), 1e-6));
assert(fabs(res.t - 5.0) < 1e-6);
```

**Teste 2: Raio paralelo (sem interseção)**
```c
Raio r = {origem: (0, 0), direcao: (1, 0)};
Segmento s = {A: (0, 1), B: (10, 1)};
assert(intersecao_raio_segmento(r, s).hit == false);
```

**Teste 3: Interseção atrás do raio**
```c
Raio r = {origem: (5, 0), direcao: (1, 0)};
Segmento s = {A: (0, -1), B: (0, 1)};
assert(intersecao_raio_segmento(r, s).hit == false);  // t < 0
```

**Teste 4: Círculo**
```c
Raio r = {origem: (0, 0), direcao: (1, 0)};
Vec2 centro = (10, 0);
double raio = 2;
Resultado res = intersecao_raio_circulo(r, centro, raio);
assert(res.hit == true);
assert(fabs(res.distancia - 8.0) < 1e-6);  // 10 - 2
```

**Teste 5: Visibilidade bloqueada**
```c
Vec2 origem = (0, 0);
Vec2 alvo = (10, 0);
Segmento muro = {A: (5, -1), B: (5, 1)};
assert(esta_visivel(origem, alvo, &muro, 1) == false);
```

### 💡 Insights

- **Epsilon:** Use tolerância pequena (~1e-6) para evitar falsos positivos
- **Otimização:** Broad-phase com AABB antes de testar cada segmento
- **Precisão numérica:** Produto vetorial 2D pode ter erro acumulado
- **Ângulos especiais:** Testar 0°, 90°, 180°, 270° explicitamente

### 🎯 Desafios Extras (+35 XP cada)

1. **Sombras dinâmicas:** Projetar sombras de objetos iluminados por ponto de luz
2. **Reflexões:** Raios refletindo em superfícies (Lei de Snell simplificada)
3. **FOV otimizado:** Algoritmo de visibilidade com menos raios (Angular Sweep)
4. **3D Ray tracing:** Expandir para raios em 3D com planos e esferas

---

## Problema 3: Collision Detection System

### 🎯 Objetivo

Implementar sistema robusto de detecção de colisões com AABB, SAT (Separating Axis Theorem), e resposta a colisões.

### 📐 Contexto Teórico

**AABB (Axis-Aligned Bounding Box):**
- Retângulo alinhado aos eixos
- Teste simples: sobreposição em X **E** sobreposição em Y

**SAT (Separating Axis Theorem):**
- Dois polígonos convexos **não** colidem sse existe um eixo onde suas projeções não se sobrepõem
- Testar eixos: normais das arestas de ambos polígonos

**Resposta a colisão:**
- **MTV (Minimum Translation Vector):** Menor deslocamento para separar objetos
- **Impulso:** Mudança instantânea de velocidade para simular impacto

### 🛠️ Especificação

**Estruturas:**

```c
typedef struct {
    Vec2 min;  // Canto inferior-esquerdo
    Vec2 max;  // Canto superior-direito
} AABB;

typedef struct {
    Vec2 centro;
    Vec2 vertices[MAX_VERTICES];  // Relativo ao centro
    int num_vertices;
    double angulo;  // Rotação
} Poligono;

typedef struct {
    int colidiram;
    Vec2 normal;   // Direção da separação (normalizado)
    double profundidade;  // Penetração
    Vec2 ponto_contato;
} ResultadoColisao;
```

**Funcionalidades:**

**AABB:**
- `aabb_de_poligono(Poligono p)` → AABB
- `colisao_aabb(AABB a, AABB b)` → Booleano
- `ponto_dentro_aabb(Vec2 p, AABB box)` → Booleano

**Polígonos convexos (SAT):**
- `colisao_sat(Poligono a, Poligono b)` → ResultadoColisao
- `obter_normais_arestas(Poligono p)` → Vec2[]
- `projetar_poligono(Poligono p, Vec2 eixo)` → (min, max)

**Utilitários:**
- `transformar_vertices(Poligono p)` → Vec2[] (aplicar rotação + translação)
- `calcular_ponto_contato(Poligono a, Poligono b, Vec2 normal)`

### 📝 Pseudocódigo

```
FUNÇÃO colisao_aabb(a, b):
    """Teste rápido de sobreposição AABB"""
    
    // Verificar sobreposição em X
    sobrepoe_x = (a.min.x <= b.max.x) E (a.max.x >= b.min.x)
    
    // Verificar sobreposição em Y
    sobrepoe_y = (a.min.y <= b.max.y) E (a.max.y >= b.min.y)
    
    RETORNAR sobrepoe_x E sobrepoe_y

FUNÇÃO aabb_de_poligono(poligono):
    """Calcula AABB envolvente"""
    
    vertices = transformar_vertices(poligono)
    
    min_x = infinito
    max_x = -infinito
    min_y = infinito
    max_y = -infinito
    
    PARA CADA v EM vertices:
        min_x = minimo(min_x, v.x)
        max_x = maximo(max_x, v.x)
        min_y = minimo(min_y, v.y)
        max_y = maximo(max_y, v.y)
    
    RETORNAR AABB{Vec2(min_x, min_y), Vec2(max_x, max_y)}

FUNÇÃO transformar_vertices(poligono):
    """Aplica rotação e translação aos vértices"""
    
    vertices_mundo = lista_vazia()
    
    PARA CADA v EM poligono.vertices:
        // Rotacionar
        cos_theta = cos(poligono.angulo)
        sin_theta = sin(poligono.angulo)
        
        x_rot = v.x * cos_theta - v.y * sin_theta
        y_rot = v.x * sin_theta + v.y * cos_theta
        
        // Transladar
        v_mundo = Vec2(x_rot, y_rot) + poligono.centro
        
        adicionar(vertices_mundo, v_mundo)
    
    RETORNAR vertices_mundo

FUNÇÃO obter_normais_arestas(poligono):
    """Retorna normais perpendiculares a cada aresta"""
    
    vertices = transformar_vertices(poligono)
    normais = lista_vazia()
    
    PARA i DE 0 ATÉ poligono.num_vertices - 1:
        j = (i + 1) % poligono.num_vertices
        
        aresta = vertices[j] - vertices[i]
        
        // Normal perpendicular (rotação 90°)
        normal = Vec2(-aresta.y, aresta.x)
        normal = normalizar(normal)
        
        adicionar(normais, normal)
    
    RETORNAR normais

FUNÇÃO projetar_poligono(poligono, eixo):
    """Projeta polígono no eixo e retorna intervalo [min, max]"""
    
    vertices = transformar_vertices(poligono)
    
    min_proj = infinito
    max_proj = -infinito
    
    PARA CADA v EM vertices:
        proj = produto_escalar(v, eixo)
        
        min_proj = minimo(min_proj, proj)
        max_proj = maximo(max_proj, proj)
    
    RETORNAR {min: min_proj, max: max_proj}

FUNÇÃO intervalo_sobrepoe(int1, int2):
    """Verifica se dois intervalos se sobrepõem"""
    RETORNAR int1.max >= int2.min E int2.max >= int1.min

FUNÇÃO calcular_sobreposicao(int1, int2):
    """Calcula profundidade da sobreposição"""
    RETORNAR minimo(int1.max - int2.min, int2.max - int1.min)

FUNÇÃO colisao_sat(a, b):
    """Separating Axis Theorem para polígonos convexos"""
    
    // Broad-phase: teste AABB primeiro
    SE NAO colisao_aabb(aabb_de_poligono(a), aabb_de_poligono(b)):
        RETORNAR {colidiram: falso}
    
    // Coletar todos os eixos a testar
    eixos = combinar(obter_normais_arestas(a), obter_normais_arestas(b))
    
    menor_sobreposicao = infinito
    eixo_separacao = nulo
    
    // Testar cada eixo
    PARA CADA eixo EM eixos:
        proj_a = projetar_poligono(a, eixo)
        proj_b = projetar_poligono(b, eixo)
        
        // Se não sobrepõem neste eixo → não colidem
        SE NAO intervalo_sobrepoe(proj_a, proj_b):
            RETORNAR {colidiram: falso}
        
        // Calcular profundidade da sobreposição
        sobreposicao = calcular_sobreposicao(proj_a, proj_b)
        
        // Guardar menor sobreposição (MTV)
        SE sobreposicao < menor_sobreposicao:
            menor_sobreposicao = sobreposicao
            eixo_separacao = eixo
    
    // Todos eixos sobrepõem → há colisão
    
    // Garantir normal aponta de A para B
    direcao_ab = b.centro - a.centro
    SE produto_escalar(eixo_separacao, direcao_ab) < 0:
        eixo_separacao = -eixo_separacao
    
    RETORNAR {
        colidiram: verdadeiro,
        normal: eixo_separacao,
        profundidade: menor_sobreposicao,
        ponto_contato: calcular_ponto_contato(a, b, eixo_separacao)
    }

FUNÇÃO resolver_colisao(a, b, resultado):
    """Aplica impulso para separar e responder à colisão"""
    
    // 1. Separação (correção de posição)
    separacao = resultado.normal * resultado.profundidade
    a.centro -= separacao * 0.5
    b.centro += separacao * 0.5
    
    // 2. Impulso (resposta dinâmica)
    vel_relativa = b.velocidade - a.velocidade
    vel_ao_longo_normal = produto_escalar(vel_relativa, resultado.normal)
    
    // Objetos se afastando → não aplicar impulso
    SE vel_ao_longo_normal > 0:
        RETORNAR
    
    // Coeficiente de restituição (elasticidade)
    e = 0.5  // 0=inelástico, 1=perfeitamente elástico
    
    // Calcular magnitude do impulso
    j = -(1 + e) * vel_ao_longo_normal
    j /= (1/a.massa + 1/b.massa)
    
    // Aplicar impulso
    impulso = resultado.normal * j
    a.velocidade -= impulso / a.massa
    b.velocidade += impulso / b.massa
```

### 🧪 Testes

**Teste 1: AABB básico**
```c
AABB a = {min: (0, 0), max: (10, 10)};
AABB b = {min: (5, 5), max: (15, 15)};
assert(colisao_aabb(a, b) == true);

AABB c = {min: (20, 20), max: (30, 30)};
assert(colisao_aabb(a, c) == false);
```

**Teste 2: Quadrados rotacionados (SAT)**
```c
// Quadrado 1: 10×10 em (0,0) sem rotação
Poligono quad1 = criar_quadrado(centro=(0,0), tamanho=10, angulo=0);

// Quadrado 2: 10×10 em (7,0) rotacionado 45°
Poligono quad2 = criar_quadrado(centro=(7,0), tamanho=10, angulo=π/4);

Resultado res = colisao_sat(quad1, quad2);
assert(res.colidiram == true);
assert(res.profundidade > 0);
```

**Teste 3: Triângulos separados**
```c
Poligono tri1 = criar_triangulo((0,0), (10,0), (5,10));
Poligono tri2 = criar_triangulo((20,0), (30,0), (25,10));
assert(colisao_sat(tri1, tri2).colidiram == false);
```

**Teste 4: MTV correto**
```c
// Dois quadrados sobrepostos 2 unidades no eixo X
Poligono a = criar_quadrado((0,0), 10, 0);
Poligono b = criar_quadrado((8,0), 10, 0);
Resultado res = colisao_sat(a, b);
assert(fabs(res.profundidade - 2.0) < 1e-6);
assert(vec2_equals(res.normal, (1, 0), 1e-6));
```

### 💡 Insights

- **Broad-phase:** AABB filtra ~90% de não-colisões rapidamente
- **Polígonos convexos:** SAT só funciona para convexos (decompor côncavos)
- **Cache de eixos:** Pré-calcular normais se polígono não rotaciona
- **Estabilidade:** Correção de posição gradual evita "jitter"

### 🎯 Desafios Extras (+45 XP cada)

1. **GJK Algorithm:** Implementar GJK (alternativa ao SAT, funciona com côncavos)
2. **Continuous Collision:** Detectar colisão durante movimento (sweep test)
3. **Spatial Partitioning:** Quadtree ou grid para otimizar broad-phase
4. **Friction:** Adicionar atrito tangencial na resposta de colisão

---

## 🎓 Entregáveis

Para cada problema:
1. ✅ Código-fonte completo
2. ✅ Suite de testes automatizados
3. ✅ Demo visual (opcional mas recomendado)
4. ✅ Documentação técnica:
   - Algoritmos escolhidos
   - Análise de complexidade
   - Casos especiais tratados

---

## 🎯 Próximos Passos

1. ✅ **Avançados:** `p3-avancados.md` (SVD, PCA, Solvers iterativos)
2. ✅ **Projeto integrador:** `k5-projeto/` (Engine 2D completo)
3. ✅ **Otimizações:** Profiling e SIMD

---

## 💡 Dicas Gerais

**Performance:**
- Profile antes de otimizar (80/20 rule)
- Broad-phase elimina 90%+ dos testes
- Cache-friendly: estruturas contíguas (arrays > listas)

**Debugging:**
- Visualize vetores (normais, MTV, impulsos)
- Teste casos extremos (velocidades altas, rotações rápidas)
- Valide invariantes (energia, momento)

**Arquitetura:**
- Separe detecção de resolução de colisões
- Use pooling para partículas (evitar alloc/free constante)
- Event-driven: callbacks para colisões

---

<details>
<summary><strong>📐 Referência: Fórmula Completa de Impulso com Rotação</strong></summary>

Para objetos que rotacionam, o impulso considera torque:

```
vel_rel = (vB + ωB × rB) - (vA + ωA × rA)
v_rel_normal = vel_rel · n

j = -(1 + e) * v_rel_normal / (1/mA + 1/mB + (rA × n)²/IA + (rB × n)²/IB)

vA' = vA - j*n/mA
vB' = vB + j*n/mB
ωA' = ωA - (rA × (j*n))/IA
ωB' = ωB + (rB × (j*n))/IB
```

Onde:
- `ω` = velocidade angular
- `r` = vetor do centro de massa ao ponto de contato
- `I` = momento de inércia
- `×` = produto vetorial (2D: retorna escalar)

</details>

---

**Total XP disponível:** 360 XP (+ 280 XP extras)  
**Tempo total estimado:** 7h-9h15  
**Dificuldade:** ⭐⭐⭐⭐ Avançado
