# Implementação: Vetores e Espaços Vetoriais

## 🎯 Meta

Implementar operações com vetores do zero, entendendo produto escalar, produto vetorial, normalização e como essas operações aparecem em física e gráficos 3D.

---

## ⏱️ Tempo Estimado

- **Leitura + Pseudocódigo:** 30-40 min
- **Implementação em linguagem real:** 1h-1h30
- **Testes e debugging:** 20-30 min
- **Total:** ~2h-2h40

---

## 📋 Pré-requisitos

- Leitura de `k1-teoria/t1-vetores-espacos.md`
- Exercícios `k2-exercicios/e1-vetores-exercicios.md` (pelo menos Nível 1)

---

## 🎚️ Dificuldade

⭐⭐ Iniciante-Intermediário

---

## 📐 Conceitos-Chave

1. **Vetor:** Entidade com magnitude e direção `v = (x, y, z)`
2. **Soma vetorial:** `a + b = (a.x + b.x, a.y + b.y, a.z + b.z)`
3. **Produto escalar:** `a · b = a.x*b.x + a.y*b.y + a.z*b.z` (escalar!)
4. **Produto vetorial:** `a × b` (perpendicular a ambos, regra da mão direita)
5. **Normalização:** `v̂ = v / ||v||` (vetor unitário)

---

## 🧩 Estrutura de Dados

### Pseudocódigo (abstrato)

```
ESTRUTURA Vetor3D:
    CAMPOS:
        x: número real
        y: número real
        z: número real
```

**Design:** Usamos estrutura simples com 3 componentes. Em produção, arrays de `float[3]` ou classes com sobrecarga de operadores (C++).

---

## 🛠️ Implementações

### 1. Operações Básicas

#### Pseudocódigo

```
FUNÇÃO criar_vetor(x, y, z):
    RETORNAR Vetor3D com (x, y, z)

FUNÇÃO somar(a, b):
    RETORNAR criar_vetor(
        a.x + b.x,
        a.y + b.y,
        a.z + b.z
    )

FUNÇÃO subtrair(a, b):
    RETORNAR criar_vetor(
        a.x - b.x,
        a.y - b.y,
        a.z - b.z
    )

FUNÇÃO escalar_multiplicar(v, k):
    """Multiplica vetor por escalar: k * v"""
    RETORNAR criar_vetor(
        k * v.x,
        k * v.y,
        k * v.z
    )
```

**🔍 Checkpoint:** Teste com `a=(1,2,3)`, `b=(4,5,6)`. Esperado: `a+b=(5,7,9)`, `2*a=(2,4,6)`.

---

### 2. Magnitude e Normalização

#### Pseudocódigo

```
FUNÇÃO magnitude(v):
    """Comprimento do vetor: ||v|| = √(x² + y² + z²)"""
    RETORNAR raiz_quadrada(v.x² + v.y² + v.z²)

FUNÇÃO normalizar(v):
    """Retorna vetor unitário na mesma direção"""
    mag = magnitude(v)
    
    SE mag == 0:
        RETORNAR criar_vetor(0, 0, 0)  // Vetor zero não tem direção
    
    RETORNAR escalar_multiplicar(v, 1.0 / mag)
```

**💡 Insight:** Normalizar antes de usar em iluminação (vetores normais) garante cálculos corretos mesmo com escala irregular.

**🔍 Checkpoint:** Vetor `(3, 4, 0)` tem magnitude 5 e normalizado é `(0.6, 0.8, 0)`.

---

### 3. Produto Escalar (Dot Product)

#### Pseudocódigo

```
FUNÇÃO produto_escalar(a, b):
    """
    a · b = |a| |b| cos(θ)
    
    Usos:
    - Projeção de um vetor em outro
    - Testar perpendicularidade (dot = 0)
    - Calcular ângulo entre vetores
    """
    RETORNAR a.x * b.x + a.y * b.y + a.z * b.z

FUNÇÃO angulo_entre(a, b):
    """Retorna ângulo em radianos"""
    dot = produto_escalar(a, b)
    mag_a = magnitude(a)
    mag_b = magnitude(b)
    
    cos_theta = dot / (mag_a * mag_b)
    
    // Garantir que cos_theta está em [-1, 1] (erros numéricos)
    cos_theta = max(-1.0, min(1.0, cos_theta))
    
    RETORNAR arccos(cos_theta)
```

**📊 Casos Especiais:**
- `dot > 0`: Ângulo agudo (< 90°)
- `dot = 0`: Perpendiculares (90°)
- `dot < 0`: Ângulo obtuso (> 90°)

**🔍 Checkpoint:** `(1,0,0) · (0,1,0) = 0` (eixos perpendiculares).

---

### 4. Produto Vetorial (Cross Product)

#### Pseudocódigo

```
FUNÇÃO produto_vetorial(a, b):
    """
    a × b = vetor perpendicular a ambos
    Magnitude: |a| |b| sin(θ)
    Direção: regra da mão direita
    
    Usos:
    - Calcular vetores normais (gráficos 3D)
    - Testar colinearidade (cross = 0)
    - Área de paralelogramo
    """
    RETORNAR criar_vetor(
        a.y * b.z - a.z * b.y,
        a.z * b.x - a.x * b.z,
        a.x * b.y - a.y * b.x
    )
```

**💡 Insight:** Em coordenadas homogêneas de mão direita (x-direita, y-cima, z-fora da tela):
- `x × y = z`
- `y × z = x`
- `z × x = y`

**🔍 Checkpoint:** `(1,0,0) × (0,1,0) = (0,0,1)`.

---

### 5. Projeção de Vetor

#### Pseudocódigo

```
FUNÇÃO projecao(v, sobre_u):
    """
    Projeta v sobre u:
    proj_u(v) = ((v · u) / (u · u)) * u
    """
    dot_vu = produto_escalar(v, sobre_u)
    dot_uu = produto_escalar(sobre_u, sobre_u)
    
    escalar = dot_vu / dot_uu
    
    RETORNAR escalar_multiplicar(sobre_u, escalar)

FUNÇÃO componente_perpendicular(v, sobre_u):
    """Componente de v perpendicular a u"""
    proj = projecao(v, sobre_u)
    RETORNAR subtrair(v, proj)
```

**📊 Aplicação:** Separar velocidade em componentes paralela e perpendicular a uma superfície.

**🔍 Checkpoint:** Projetar `(3,3)` sobre `(1,0)` dá `(3,0)`.

---

## 🐛 Debugging Comum

### Problema 1: Divisão por Zero em Normalização

```
❌ ERRADO:
FUNÇÃO normalizar(v):
    mag = magnitude(v)
    RETORNAR escalar_multiplicar(v, 1.0 / mag)  // CRASH se mag=0!

✅ CORRETO:
FUNÇÃO normalizar(v):
    mag = magnitude(v)
    SE mag < EPSILON:  // Ex: EPSILON = 1e-10
        RETORNAR criar_vetor(0, 0, 0)
    RETORNAR escalar_multiplicar(v, 1.0 / mag)
```

---

### Problema 2: Ordem no Produto Vetorial

```
❌ ERRADO:
// a × b ≠ b × a  (anticomutativo!)
normal = produto_vetorial(tangente, binormal)  // Pode inverter normal

✅ CORRETO:
// Sempre usar ordem consistente (regra da mão direita)
normal = produto_vetorial(tangente, bitangente)
SE produto_escalar(normal, esperado) < 0:
    normal = escalar_multiplicar(normal, -1)  // Inverter se necessário
```

---

### Problema 3: Comparação de Floats

```
❌ ERRADO:
SE magnitude(v) == 0:  // Nunca exatamente 0 com floats!

✅ CORRETO:
DEFINE EPSILON = 1e-10
SE magnitude(v) < EPSILON:
```

---

## 🧪 Testes Unitários (Casos de Teste)

```
TESTE soma_vetores:
    a = criar_vetor(1, 2, 3)
    b = criar_vetor(4, 5, 6)
    c = somar(a, b)
    ASSERT c.x == 5 E c.y == 7 E c.z == 9

TESTE vetor_zero:
    v = criar_vetor(0, 0, 0)
    ASSERT magnitude(v) < EPSILON

TESTE normalizacao:
    v = criar_vetor(3, 4, 0)
    v_norm = normalizar(v)
    ASSERT abs(magnitude(v_norm) - 1.0) < EPSILON

TESTE produto_escalar_perpendiculares:
    x = criar_vetor(1, 0, 0)
    y = criar_vetor(0, 1, 0)
    ASSERT abs(produto_escalar(x, y)) < EPSILON

TESTE produto_vetorial_eixos:
    x = criar_vetor(1, 0, 0)
    y = criar_vetor(0, 1, 0)
    z = produto_vetorial(x, y)
    ASSERT abs(z.x) < EPSILON
    ASSERT abs(z.y) < EPSILON
    ASSERT abs(z.z - 1.0) < EPSILON
```

---

## 📚 Exercícios de Implementação

1. **Distância entre dois pontos** usando vetores
2. **Reflexão de vetor** sobre plano dado vetor normal
3. **Interpolação linear (LERP)** entre dois vetores
4. **Rotação de vetor 2D** (sem matrizes, usando fórmulas trigonométricas)

---

## 🎯 Próximos Passos

Após dominar vetores:
1. ✅ **Matrizes:** `i2-matrizes.md`
2. ✅ **Transformações:** Aplicar vetores em rotações/translações
3. ✅ **Projeto:** Sistema de partículas usando vetores de velocidade/aceleração

---

## 💡 Aplicações Reais

- **Física:** Forças, velocidades, acelerações
- **Gráficos 3D:** Posições, normais, iluminação (Lambert: `max(0, N·L)`)
- **IA/Jogos:** Pathfinding (direção ao alvo), cone de visão
- **ML:** Embeddings de palavras (word2vec), similarity (cosine similarity = dot normalizado)

---

<details>
<summary><strong>💻 Implementação Completa em C</strong></summary>

```c
#include <stdio.h>
#include <math.h>

#define EPSILON 1e-10

typedef struct {
    double x, y, z;
} Vec3;

Vec3 vec3_create(double x, double y, double z) {
    Vec3 v = {x, y, z};
    return v;
}

Vec3 vec3_add(Vec3 a, Vec3 b) {
    return vec3_create(a.x + b.x, a.y + b.y, a.z + b.z);
}

Vec3 vec3_sub(Vec3 a, Vec3 b) {
    return vec3_create(a.x - b.x, a.y - b.y, a.z - b.z);
}

Vec3 vec3_scale(Vec3 v, double k) {
    return vec3_create(k * v.x, k * v.y, k * v.z);
}

double vec3_length(Vec3 v) {
    return sqrt(v.x * v.x + v.y * v.y + v.z * v.z);
}

Vec3 vec3_normalize(Vec3 v) {
    double len = vec3_length(v);
    if (len < EPSILON) return vec3_create(0, 0, 0);
    return vec3_scale(v, 1.0 / len);
}

double vec3_dot(Vec3 a, Vec3 b) {
    return a.x * b.x + a.y * b.y + a.z * b.z;
}

Vec3 vec3_cross(Vec3 a, Vec3 b) {
    return vec3_create(
        a.y * b.z - a.z * b.y,
        a.z * b.x - a.x * b.z,
        a.x * b.y - a.y * b.x
    );
}

Vec3 vec3_project(Vec3 v, Vec3 onto) {
    double dot_vu = vec3_dot(v, onto);
    double dot_uu = vec3_dot(onto, onto);
    return vec3_scale(onto, dot_vu / dot_uu);
}

double vec3_angle(Vec3 a, Vec3 b) {
    double dot = vec3_dot(a, b);
    double mag_a = vec3_length(a);
    double mag_b = vec3_length(b);
    double cos_theta = dot / (mag_a * mag_b);
    
    // Clamp para [-1, 1]
    if (cos_theta < -1.0) cos_theta = -1.0;
    if (cos_theta > 1.0) cos_theta = 1.0;
    
    return acos(cos_theta);
}

void vec3_print(Vec3 v) {
    printf("(%.4f, %.4f, %.4f)\n", v.x, v.y, v.z);
}

int main() {
    // Teste 1: Soma
    Vec3 a = vec3_create(1, 2, 3);
    Vec3 b = vec3_create(4, 5, 6);
    Vec3 c = vec3_add(a, b);
    printf("a + b = "); vec3_print(c);
    
    // Teste 2: Normalização
    Vec3 v = vec3_create(3, 4, 0);
    printf("|v| = %.2f\n", vec3_length(v));
    Vec3 v_norm = vec3_normalize(v);
    printf("v normalizado = "); vec3_print(v_norm);
    printf("|v_norm| = %.4f\n", vec3_length(v_norm));
    
    // Teste 3: Produto escalar
    Vec3 x = vec3_create(1, 0, 0);
    Vec3 y = vec3_create(0, 1, 0);
    printf("x · y = %.2f\n", vec3_dot(x, y));
    
    // Teste 4: Produto vetorial
    Vec3 z = vec3_cross(x, y);
    printf("x × y = "); vec3_print(z);
    
    // Teste 5: Ângulo
    Vec3 u = vec3_create(1, 1, 0);
    double angle = vec3_angle(x, u);
    printf("Ângulo entre x e (1,1,0) = %.2f rad (%.2f°)\n", 
           angle, angle * 180.0 / M_PI);
    
    return 0;
}
```

**Compilar:** `gcc -o vetores i1-vetores.c -lm`

**Saída esperada:**
```
a + b = (5.0000, 7.0000, 9.0000)
|v| = 5.00
v normalizado = (0.6000, 0.8000, 0.0000)
|v_norm| = 1.0000
x · y = 0.00
x × y = (0.0000, 0.0000, 1.0000)
Ângulo entre x e (1,1,0) = 0.79 rad (45.00°)
```

</details>

---

**XP Disponível:** 50 XP (implementação completa + testes)  
**Tempo estimado:** 2h-2h40  
**Dificuldade:** ⭐⭐
