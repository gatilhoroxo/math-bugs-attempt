# Problemas de Programação: Básicos

## 🎯 Meta

Implementar estruturas de dados e funções fundamentais de geometria analítica, criando ferramentas reutilizáveis para pontos, retas e planos.

---

## ⏱️ Tempo Estimado

- **Problema 1 (Biblioteca Geometria 3D):** 1h-1h30
- **Problema 2 (Calculadora de Distâncias):** 1h-1h30
- **Problema 3 (Visualizador de Interseções 2D):** 1h15-1h45
- **Total:** ~3h15-4h45

---

## 📋 Pré-requisitos

- **Teoria:** `k1-teoria/t1-pontos-retas-planos.md`, `t2-distancias-angulos.md`
- **Exercícios:** `k2-exercicios/e1-pontos-retas-planos-exercicios.md`, `e2-distancias-angulos-exercicios.md` (Nível 1-2)
- **Implementação:** `k3-implementacao/i1-pontos-retas-planos.md`, `i2-distancias-angulos.md` (leitura)
- **Linguagem:** C ou C++

---

## 🎚️ Dificuldade

⭐⭐⭐ Intermediário

---

## 💪 Sistema de XP

- **Problema 1:** 100 XP
- **Problema 2:** 90 XP  
- **Problema 3:** 90 XP

**XP Total Disponível:** 280 XP

---

## 📊 Rastreamento de Progresso

- [ ] Problema 1: Biblioteca de Geometria 3D (0/1) - 100 XP
- [ ] Problema 2: Calculadora de Distâncias CLI (0/1) - 90 XP
- [ ] Problema 3: Visualizador de Interseções 2D (0/1) - 90 XP

**XP Conquistado:** ___ / 280 XP

---

## Problema 1: Biblioteca de Geometria 3D

### 🎯 Objetivo

Criar uma biblioteca C completa para operações com pontos, retas e planos em 3D, incluindo header file, implementação e programa de testes.

### 📐 Contexto Teórico

Geometria analítica fundamental:
- **Ponto:** Posição no espaço `P = (x, y, z)`
- **Reta:** Conjunto de pontos `P(t) = P₀ + t·d` (forma paramétrica)
- **Plano:** Conjunto de pontos `ax + by + cz + d = 0` (forma implícita)

**Aplicações:** Gráficos 3D (ray casting), navegação (rotas), física (colisões)

### 🛠️ Especificação

**Estruturas de dados:**
```c
typedef struct {
    double x, y, z;
} Point3D;

typedef struct {
    Point3D origin;    // Ponto por onde a reta passa
    Point3D direction; // Vetor direção (não precisa ser unitário)
} Line3D;

typedef struct {
    Point3D normal;    // Vetor normal (a, b, c)
    double d;          // Termo constante em ax + by + cz + d = 0
} Plane3D;
```

**Funcionalidades obrigatórias:**

**Pontos:**
- `Point3D point_create(double x, double y, double z)`
- `double point_distance(Point3D p1, Point3D p2)` - Distância euclidiana
- `Point3D point_midpoint(Point3D p1, Point3D p2)` - Ponto médio
- `Point3D point_vector_between(Point3D from, Point3D to)` - Vetor direção
- `void point_print(Point3D p)` - Imprimir "(x, y, z)"

**Retas:**
- `Line3D line_create(Point3D origin, Point3D direction)`
- `Line3D line_from_two_points(Point3D p1, Point3D p2)`
- `Point3D line_point_at(Line3D line, double t)` - P(t) = P₀ + t·d
- `double line_distance_to_point(Line3D line, Point3D p)` - Distância ponto-reta
- `void line_print(Line3D line)` - Formato "P=(x,y,z) + t*(dx,dy,dz)"

**Planos:**
- `Plane3D plane_create(Point3D normal, double d)`
- `Plane3D plane_from_point_and_normal(Point3D point, Point3D normal)`
- `Plane3D plane_from_three_points(Point3D p1, Point3D p2, Point3D p3)`
- `double plane_distance_to_point(Plane3D plane, Point3D p)` - Distância ponto-plano
- `int plane_contains_point(Plane3D plane, Point3D p, double epsilon)` - Verifica se ponto está no plano
- `void plane_print(Plane3D plane)` - Formato "ax + by + cz + d = 0"

**Utilidades (funções auxiliares de vetores):**
- `double vec3_magnitude(Point3D v)`
- `Point3D vec3_normalize(Point3D v)`
- `double vec3_dot(Point3D v1, Point3D v2)`
- `Point3D vec3_cross(Point3D v1, Point3D v2)`

### 📝 Pseudocódigo

```
FUNÇÃO point_distance(p1, p2):
    dx = p2.x - p1.x
    dy = p2.y - p1.y
    dz = p2.z - p1.z
    RETORNAR raiz_quadrada(dx² + dy² + dz²)

FUNÇÃO line_point_at(line, t):
    RETORNAR Point3D(
        line.origin.x + t * line.direction.x,
        line.origin.y + t * line.direction.y,
        line.origin.z + t * line.direction.z
    )

FUNÇÃO line_distance_to_point(line, p):
    // d = ||AP × d|| / ||d||
    // onde A = origin, P = ponto, d = direction
    
    ap = point_vector_between(line.origin, p)
    cruz = vec3_cross(ap, line.direction)
    
    mag_cruz = vec3_magnitude(cruz)
    mag_dir = vec3_magnitude(line.direction)
    
    RETORNAR mag_cruz / mag_dir

FUNÇÃO plane_from_point_and_normal(point, normal):
    // ax + by + cz + d = 0
    // Substituir ponto: a*px + b*py + c*pz + d = 0
    d = -(normal.x*point.x + normal.y*point.y + normal.z*point.z)
    RETORNAR Plane3D(normal, d)

FUNÇÃO plane_distance_to_point(plane, p):
    // d = |ax + by + cz + d| / √(a² + b² + c²)
    numerador = abs(plane.normal.x*p.x + plane.normal.y*p.y + plane.normal.z*p.z + plane.d)
    denominador = vec3_magnitude(plane.normal)
    RETORNAR numerador / denominador
```

### 🧪 Testes

Crie `test_geometry.c` que verifica:

```c
// Teste 1: Distância entre pontos
Point3D p1 = point_create(0, 0, 0);
Point3D p2 = point_create(3, 4, 0);
double dist = point_distance(p1, p2);
assert(fabs(dist - 5.0) < 0.001);

// Teste 2: Ponto na reta
Line3D line = line_create(
    point_create(1, 1, 1),
    point_create(1, 0, 0)  // Direção: paralela ao eixo X
);
Point3D p = line_point_at(line, 5.0);
assert(fabs(p.x - 6.0) < 0.001);
assert(fabs(p.y - 1.0) < 0.001);

// Teste 3: Distância ponto-reta
Line3D line_x = line_create(point_create(0,0,0), point_create(1,0,0));
Point3D p_above = point_create(0, 3, 0);
double dist_pr = line_distance_to_point(line_x, p_above);
assert(fabs(dist_pr - 3.0) < 0.001);

// Teste 4: Plano e distância
Plane3D plane_xy = plane_create(point_create(0,0,1), 0); // z=0
Point3D p_high = point_create(0, 0, 5);
double dist_pp = plane_distance_to_point(plane_xy, p_high);
assert(fabs(dist_pp - 5.0) < 0.001);

// Teste 5: Plano de 3 pontos
Plane3D plane_tri = plane_from_three_points(
    point_create(1,0,0),
    point_create(0,1,0),
    point_create(0,0,1)
);
// Deve ser x + y + z - 1 = 0
Point3D test_p = point_create(0.5, 0.5, 0);
assert(plane_contains_point(plane_tri, test_p, 0.001));

printf("✅ Todos os testes passaram!\n");
```

### 📦 Estrutura de Arquivos

```
src/
  geometry3d.h      (header com declarações)
  geometry3d.c      (implementação)
  test_geometry.c   (testes)
  Makefile

# Makefile básico:
test: geometry3d.o test_geometry.o
	gcc -o test geometry3d.o test_geometry.o -lm
	./test

geometry3d.o: geometry3d.c geometry3d.h
	gcc -c geometry3d.c -Wall -Wextra

test_geometry.o: test_geometry.c geometry3d.h
	gcc -c test_geometry.c -Wall -Wextra
```

### 🏆 Critérios de Conclusão

- [ ] Todas as 20+ funções implementadas
- [ ] Header file bem documentado com comentários
- [ ] Todos os 5 testes passando
- [ ] Compilação sem warnings
- [ ] Makefile funcional

**XP Concedido:** 100 XP

---

## Problema 2: Calculadora de Distâncias CLI

### 🎯 Objetivo

Criar um programa de linha de comando que calcula distâncias entre diferentes objetos geométricos (ponto-ponto, ponto-reta, ponto-plano, reta-reta).

### 📐 Contexto Teórico

Cálculos de distância são fundamentais para:
- **Navegação:** Quão longe estou da rota?
- **Colisão:** Objetos estão próximos demais?
- **Otimização:** Qual é o ponto mais próximo?

### 🛠️ Especificação

**Interface CLI:**

```bash
$ ./distcalc

=== Calculadora de Distâncias 3D ===
Escolha o tipo de cálculo:
  1) Distância ponto-ponto
  2) Distância ponto-reta
  3) Distância ponto-plano
  4) Distância reta-reta
  5) Sair

Opção: 2

=== Distância Ponto-Reta ===
Entre o ponto (x y z): 0 3 0
Entre a origem da reta (x y z): 0 0 0
Entre a direção da reta (x y z): 1 0 0

Resultado:
  Distância: 3.000
  Ponto mais próximo na reta: (0.000, 0.000, 0.000)
  Parâmetro t: 0.000
```

**Funcionalidades obrigatórias:**

1. **Menu interativo** com loop até usuário escolher sair
2. **Entrada validada** (verificar se números válidos)
3. **Cálculos precisos** usando biblioteca do Problema 1
4. **Saída formatada** com 3 casas decimais
5. **Tratamento de casos especiais:**
   - Reta com direção zero
   - Plano com normal zero
   - Retas paralelas vs reversas

### 📝 Pseudocódigo

```
LOOP PRINCIPAL:
    IMPRIMIR menu
    LER opção
    
    SE opção == 1:
        LER p1 (x, y, z)
        LER p2 (x, y, z)
        dist = point_distance(p1, p2)
        IMPRIMIR "Distância: {dist:.3f}"
    
    SENÃO SE opção == 2:
        LER ponto
        LER origem_reta
        LER direcao_reta
        
        // Validar direção não-zero
        SE magnitude(direcao) < 0.001:
            IMPRIMIR "Erro: Direção da reta é zero!"
            CONTINUAR
        
        reta = criar_reta(origem, direcao)
        resultado = distancia_ponto_reta_detalhada(ponto, reta)
        
        IMPRIMIR "Distância: {resultado.distancia:.3f}"
        IMPRIMIR "Ponto mais próximo: ({resultado.ponto2.x:.3f}, ...)"
        IMPRIMIR "Parâmetro t: {resultado.parametro2:.3f}"
    
    // ... outras opções
```

### 🧪 Casos de Teste

Teste manualmente ou crie script:

```bash
# Teste 1: Ponto-ponto diagonal
Entrada: 1, (0,0,0), (1,1,1)
Esperado: Distância = 1.732

# Teste 2: Ponto-reta perpendicular
Entrada: 2, ponto=(5,5,0), origem=(0,0,0), dir=(1,0,0)
Esperado: Distância = 5.000, ponto_mais_proximo=(5,0,0)

# Teste 3: Ponto-plano
Entrada: 3, ponto=(3,4,5), normal=(0,0,1), d=0
Esperado: Distância = 5.000

# Teste 4: Retas paralelas
Entrada: 4, r1: orig=(0,0,0) dir=(1,0,0), r2: orig=(0,1,0) dir=(2,0,0)
Esperado: Distância = 1.000, "Retas são paralelas"
```

### 🏆 Critérios de Conclusão

- [ ] Menu interativo funcional
- [ ] Todas as 4 opções de cálculo implementadas
- [ ] Validação de entrada
- [ ] Tratamento de casos especiais
- [ ] Testes manuais passam

**XP Concedido:** 90 XP

---

## Problema 3: Visualizador de Interseções 2D

### 🎯 Objetivo

Criar um programa que calcula e visualiza interseções entre retas e círculos em 2D, gerando saída ASCII art ou arquivo SVG.

### 📐 Contexto Teórico

Interseções são fundamentais para:
- **Ray casting:** Qual objeto o raio atinge?
- **Colisão:** Objetos se cruzam?
- **Navegação:** Onde rotas se encontram?

### 🛠️ Especificação

**Entrada via arquivo de configuração `scene.txt`:**
```
# Linhas (formato: LINE x1 y1 x2 y2)
LINE 0 0 10 10
LINE 0 10 10 0

# Círculos (formato: CIRCLE cx cy radius)
CIRCLE 5 5 3

# Raios (formato: RAY ox oy dx dy)
RAY 0 0 1 0.5
```

**Saída:**
```
=== Cena 2D ===
Objetos:
  - Reta 1: (0.00, 0.00) → (10.00, 10.00)
  - Reta 2: (0.00, 10.00) → (10.00, 0.00)
  - Círculo 1: Centro (5.00, 5.00), Raio 3.00
  - Raio 1: Origem (0.00, 0.00), Direção (1.00, 0.50)

Interseções:
  Reta 1 × Reta 2: (5.00, 5.00)
  Raio 1 × Círculo 1: (3.46, 1.73) e (6.54, 3.27)

=== Visualização ASCII ===
[Grade 20x20 com caracteres representando objetos e interseções]
```

**Funcionalidades obrigatórias:**

1. **Parser de arquivo** (ler linhas, círculos, raios)
2. **Cálculo de interseções:**
   - Reta-reta (2D): Resolver sistema linear 2×2
   - Raio-círculo: Equação quadrática
3. **Visualização ASCII:**
   - Grade de caracteres (ex: 40×40)
   - Retas: `/`, `\`, `|`, `-`
   - Círculos: `O`
   - Interseções: `X`
4. **Saída de relatório** (texto formatado)

### 📝 Pseudocódigo

```
ESTRUTURA Linha2D:
    p1, p2: Ponto (x, y)

ESTRUTURA Circulo2D:
    centro: Ponto (x, y)
    raio: número

FUNÇÃO intersecao_reta_reta_2d(reta1, reta2):
    // Resolver sistema:
    // P1 + t(D1) = P2 + s(D2)
    // Usando determinantes (Regra de Cramer)
    
    d1 = reta1.p2 - reta1.p1
    d2 = reta2.p2 - reta2.p1
    
    det = d1.x * d2.y - d1.y * d2.x
    
    SE abs(det) < epsilon:
        RETORNAR SEM_INTERSECAO  // Paralelas
    
    dp = reta2.p1 - reta1.p1
    t = (dp.x * d2.y - dp.y * d2.x) / det
    
    ponto_inter = reta1.p1 + t * d1
    RETORNAR ponto_inter

FUNÇÃO intersecao_raio_circulo(origem, direcao, circulo):
    // Raio: P(t) = O + t*D
    // Círculo: ||P - C||² = r²
    // Substituir: ||(O + t*D) - C||² = r²
    
    oc = origem - circulo.centro
    
    a = dot(direcao, direcao)
    b = 2 * dot(oc, direcao)
    c = dot(oc, oc) - circulo.raio²
    
    discriminante = b² - 4ac
    
    SE discriminante < 0:
        RETORNAR SEM_INTERSECAO
    
    t1 = (-b - sqrt(discriminante)) / (2a)
    t2 = (-b + sqrt(discriminante)) / (2a)
    
    ponto1 = origem + t1 * direcao
    ponto2 = origem + t2 * direcao
    
    RETORNAR (ponto1, ponto2)
```

### 🧪 Teste

Arquivo `test_scene.txt`:
```
LINE 0 5 10 5
CIRCLE 5 5 2
RAY 0 0 1 1
```

Saída esperada:
```
Interseções:
  Reta 1 × Círculo 1: (3.00, 5.00) e (7.00, 5.00)
  Raio 1 × Círculo 1: (3.59, 3.59) e (6.41, 6.41)
```

### 🏆 Critérios de Conclusão

- [ ] Parser lê arquivo corretamente
- [ ] Interseção reta-reta funcional
- [ ] Interseção raio-círculo funcional
- [ ] Relatório textual gerado
- [ ] Visualização ASCII (opcional: SVG)

**XP Concedido:** 90 XP

---

## 🔗 Próximos Passos

Após completar os problemas básicos:
- `p2-intermediarios.md` → Ray tracer 2D, navegação GPS
- `p3-avancados.md` → Ray tracer 3D, física de colisões
- `k5-projeto/` → Sistema de navegação completo

---

## 📚 Recursos

- [Scratchapixel - Geometry](https://www.scratchapixel.com/lessons/mathematics-physics-for-computer-graphics/geometry)
- [GLM (OpenGL Mathematics)](https://github.com/g-truc/glm) - Biblioteca de referência
- Real-Time Collision Detection (Ericson) - Capítulo 5
