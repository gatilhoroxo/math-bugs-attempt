# Álgebra Linear - Exercícios Progressivos

## Como Usar Este Guia

Os exercícios estão organizados em **níveis** e por **tópico**. Cada exercício tem:
- 🎯 Objetivo: O que você vai praticar
- 💡 Dica: Sugestão para resolver
- ⏱️ Tempo estimado

**Recomendação:** Faça pelo menos 2-3 exercícios de cada nível antes de avançar.

---

## Nível 1: Fundamentos de Vetores

### Exercício 1.1: Implementar Operações Básicas
🎯 **Objetivo:** Solidificar operações vetoriais básicas

Implemente em C as seguintes funções para vetores 2D:
```c
typedef struct { double x, y; } Vec2;

Vec2 vec2_add(Vec2 a, Vec2 b);
Vec2 vec2_sub(Vec2 a, Vec2 b);
Vec2 vec2_scale(Vec2 v, double k);
double vec2_length(Vec2 v);
Vec2 vec2_normalize(Vec2 v);
double vec2_dot(Vec2 a, Vec2 b);
```

**Teste com:**
- a = (3, 4), b = (1, 2)
- Calcule: a + b, a - b, 2 * a, |a|, â (normalizado), a · b

⏱️ **Tempo:** 30-40 minutos

---

### Exercício 1.2: Ângulo Entre Vetores
🎯 **Objetivo:** Usar produto escalar para calcular ângulos

Dado que `a · b = |a| |b| cos(θ)`, implemente uma função que calcula o ângulo entre dois vetores:

```c
double vec2_angle(Vec2 a, Vec2 b);
```

**Teste:**
- Vetores perpendiculares: (1, 0) e (0, 1) → deve dar 90°
- Vetores paralelos: (1, 0) e (2, 0) → deve dar 0°
- Vetores opostos: (1, 0) e (-1, 0) → deve dar 180°

💡 **Dica:** `θ = arccos((a · b) / (|a| |b|))`

⏱️ **Tempo:** 20 minutos

---

### Exercício 1.3: Projeção de Vetor
🎯 **Objetivo:** Entender projeção vetorial (muito usado em física/jogos)

A projeção de **a** sobre **b** é:
```
proj_b(a) = ((a · b) / (b · b)) * b
```

Implemente:
```c
Vec2 vec2_project(Vec2 a, Vec2 b);
```

**Contexto:** Em um jogo, o jogador está em A e quer se mover para B, mas há um muro. A projeção do movimento desejado sobre o muro dá o movimento permitido.

**Teste:** 
- a = (3, 4), b = (1, 0) → proj = (3, 0)
- a = (1, 1), b = (1, 0) → proj = (1, 0)

⏱️ **Tempo:** 25 minutos

---

## Nível 2: Matrizes e Transformações 2D

### Exercício 2.1: Multiplicação Matriz × Vetor
🎯 **Objetivo:** Implementar a operação fundamental

Implemente a multiplicação de uma matriz 2×2 por um vetor 2D:
```c
typedef struct {
    double m[2][2];
} Mat2;

Vec2 mat2_mul_vec2(Mat2 mat, Vec2 v);
```

**Teste com matriz de rotação 90°:**
```
R = [0  -1]
    [1   0]
```
- R × (1, 0) deve dar (0, 1)
- R × (0, 1) deve dar (-1, 0)

⏱️ **Tempo:** 20 minutos

---

### Exercício 2.2: Criar Matriz de Rotação
🎯 **Objetivo:** Entender rotações

Implemente:
```c
Mat2 mat2_rotation(double angle);  // ângulo em radianos
```

A matriz de rotação é:
```
[cos(θ)  -sin(θ)]
[sin(θ)   cos(θ)]
```

**Teste:**
- Rotacionar o ponto (1, 0) por 45° → deve dar aproximadamente (0.707, 0.707)
- Rotacionar (0, 1) por 90° → deve dar (-1, 0)

💡 **Dica:** Use `#include <math.h>` para cos() e sin()

⏱️ **Tempo:** 25 minutos

---

### Exercício 2.3: Combinar Transformações
🎯 **Objetivo:** Entender composição de transformações

Implemente multiplicação de matrizes 3×3:
```c
typedef struct {
    double m[3][3];
} Mat3;

Mat3 mat3_mul(Mat3 a, Mat3 b);
```

**Desafio:** Crie uma transformação que:
1. Rotaciona 45° ao redor da origem
2. Escala 2x em X e 0.5x em Y
3. Translada para (5, 3)

Aplique essa transformação ao quadrado [(0,0), (1,0), (1,1), (0,1)].

💡 **Dica:** A ordem é T × S × R (translação por último!)

⏱️ **Tempo:** 40 minutos

---

## Nível 3: Aplicações Práticas

### Exercício 3.1: Detecção de Colisão (SAT)
🎯 **Objetivo:** Usar projeções para detectar colisão

O **Separating Axis Theorem (SAT)** usa projeções para detectar se dois polígonos se sobrepõem.

**Tarefa:** Dados dois retângulos axis-aligned, use projeções nos eixos X e Y para determinar se colidem.

```c
typedef struct {
    Vec2 min, max;  // Bounding box
} AABB;

int aabb_intersects(AABB a, AABB b);
```

**Teste:**
- A = {(0,0), (2,2)}, B = {(1,1), (3,3)} → colidem
- A = {(0,0), (1,1)}, B = {(2,2), (3,3)} → não colidem

⏱️ **Tempo:** 30 minutos

---

### Exercício 3.2: Câmera 2D
🎯 **Objetivo:** Implementar view matrix para jogos 2D

Uma câmera 2D precisa:
1. Translação (posição da câmera)
2. Rotação (orientação)
3. Zoom (escala)

Implemente:
```c
typedef struct {
    Vec2 position;
    double rotation;
    double zoom;
} Camera2D;

Mat3 camera2d_get_view_matrix(Camera2D cam);
```

A view matrix é o **inverso** da transform matrix da câmera.

💡 **Dica:** View = T(-pos) × R(-rot) × S(1/zoom)

**Teste:** Posicione a câmera em (10, 10) com zoom 2x. Um objeto em (15, 15) deve aparecer em (5, 5) na tela (perto do centro).

⏱️ **Tempo:** 45 minutos

---

### Exercício 3.3: Interpolação Linear (Lerp)
🎯 **Objetivo:** Base para animações

Interpolar entre dois valores é essencial para animações. A forma vetorial é:
```
lerp(a, b, t) = a + t * (b - a)  onde t ∈ [0, 1]
```

Implemente:
```c
Vec2 vec2_lerp(Vec2 a, Vec2 b, double t);
double lerp(double a, double b, double t);
```

**Desafio:** Anime um objeto se movendo de (0, 0) para (10, 10) em 60 frames.

⏱️ **Tempo:** 20 minutos

---

## Nível 4: Sistemas Lineares

### Exercício 4.1: Resolver Sistema 2×2
🎯 **Objetivo:** Resolver sistemas pequenos manualmente

Resolva o sistema:
```
2x + y = 5
x - y = 1
```

Usando:
1. Método algébrico (substituição)
2. Forma matricial (A⁻¹ × b)
3. Código (eliminação Gaussiana)

⏱️ **Tempo:** 30 minutos

---

### Exercício 4.2: Regressão Linear Simples
🎯 **Objetivo:** Aplicar sistemas lineares em ML

Dados pontos {(1,2), (2,4), (3,5), (4,7)}, encontre a melhor reta y = mx + c.

Isso se resume a resolver:
```
[n    Σx  ] [c]   [Σy  ]
[Σx   Σx² ] [m] = [Σxy ]
```

Implemente e teste!

💡 **Dica:** Este é o **método dos mínimos quadrados**.

⏱️ **Tempo:** 45 minutos

---

## Nível 5: Autovalores e Autovetores

### Exercício 5.1: Método da Potência
🎯 **Objetivo:** Encontrar o maior autovalor

Implemente o método da potência (visto em 02-implementacao.md) e teste com:

```
A = [2  1]
    [1  2]
```

O maior autovalor deve ser λ = 3, com autovetor (1, 1).

⏱️ **Tempo:** 40 minutos

---

### Exercício 5.2: Análise de Estabilidade
🎯 **Objetivo:** Usar autovalores para análise

Um sistema linear discreto é:
```
x_{n+1} = A × x_n
```

O sistema é **estável** se todos os autovalores têm magnitude < 1.

**Tarefa:** Dado:
```
A = [0.8  0.1]
    [0.1  0.8]
```

Determine se o sistema é estável (dica: calcule autovalores).

⏱️ **Tempo:** 35 minutos

---

## Exercícios Desafio 🏴‍☠️

### Desafio 1: PageRank Simplificado
🎯 **Objetivo:** Entender o algoritmo do Google

Dado um grafo de 4 páginas web:
- A aponta para B e C
- B aponta para A
- C aponta para A
- D aponta para B e C

Crie a matriz de adjacência e use o método da potência para encontrar o PageRank (autovetor principal).

⏱️ **Tempo:** 60+ minutos

---

### Desafio 2: PCA (Principal Component Analysis)
🎯 **Objetivo:** Redução de dimensionalidade

Dados pontos 2D: {(1,2), (2,3), (3,4), (4,5), (5,6)}

1. Calcule a matriz de covariância
2. Encontre autovalores/autovetores
3. Projete os dados no componente principal

Este é o **PCA** usado em ML para reduzir dimensões!

⏱️ **Tempo:** 90+ minutos

---

## Links para Prática Online

### Problemas de Álgebra Linear
- **Project Euler** (problemas matemáticos): https://projecteuler.net
- **HackerRank - Linear Algebra**: https://www.hackerrank.com/domains/mathematics
- **LeetCode - Matrix Problems**: https://leetcode.com/tag/matrix/

### Problemas de Geometria Computacional
- **CSES - Geometry**: https://cses.fi/problemset/ (seção Geometry)
- **Codeforces - Geometry Tag**: https://codeforces.com/problemset?tags=geometry

### Visualizadores Interativos
- **3Blue1Brown - Essence of Linear Algebra**: https://www.3blue1brown.com/topics/linear-algebra
  - Melhor série de vídeos sobre álgebra linear, com visualizações incríveis
- **Matrix Multiplier**: http://matrixmultiplication.xyz
- **Linear Transformation Visualizer**: https://www.geogebra.org/m/cF8QFFVH

---

## Checklist de Progresso

Marque conforme completa:

**Vetores:**
- [ ] Operações básicas (add, sub, scale)
- [ ] Produto escalar e ângulos
- [ ] Projeção vetorial
- [ ] Aplicação em jogos/física

**Matrizes:**
- [ ] Multiplicação matriz × vetor
- [ ] Multiplicação matriz × matriz
- [ ] Transformações 2D (rotação, escala, translação)
- [ ] Composição de transformações

**Sistemas Lineares:**
- [ ] Resolver 2×2 manualmente
- [ ] Implementar eliminação Gaussiana
- [ ] Aplicar em regressão linear

**Autovalores:**
- [ ] Entender conceito geometricamente
- [ ] Implementar método da potência
- [ ] Aplicar em análise de estabilidade

**Projeto:**
- [ ] Transformações 2D interativas (próximo arquivo!)

---

## Próximos Passos

Após completar os exercícios dos níveis 1-3, você estará pronto para o **projeto âncora** (transformações 2D). Os exercícios dos níveis 4-5 podem ser feitos em paralelo com o projeto ou depois dele, conforme seu interesse.

Não se preocupe em fazer TODOS os exercícios - escolha os que mais te interessam! O importante é entender os conceitos e conseguir aplicá-los.