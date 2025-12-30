# Implementação: Transformações Geométricas

## 🎯 Meta

Implementar transformações 2D e 3D usando matrizes (rotação, translação, escala, cisalhamento), entendendo coordenadas homogêneas e composição de transformações.

---

## ⏱️ Tempo Estimado

- **Leitura + Pseudocódigo:** 50-60 min
- **Implementação em linguagem real:** 1h45-2h15
- **Testes e debugging:** 35-45 min
- **Total:** ~3h-4h

---

## 📋 Pré-requisitos

- Leitura de `k1-teoria/t2-transformacoes-matrizes.md`
- Implementação de `i2-matrizes.md`
- Exercícios `k2-exercicios/e2-transformacoes-exercicios.md` (Nível 1-2)

---

## 🎚️ Dificuldade

⭐⭐⭐⭐ Avançado

---

## 📐 Conceitos-Chave

1. **Coordenadas Homogêneas:** 2D `(x,y)` → `(x,y,1)` para representar translação como matriz
2. **Transformações Afins:** Preservam paralelismo (rotação, escala, translação, cisalhamento)
3. **Composição:** `M = T * R * S` (ordem importa! Aplica S→R→T)
4. **Matriz Model:** Transforma do espaço local para mundo
5. **Matriz View:** Transforma mundo para câmera

---

## 🧩 Estrutura de Dados

### Pseudocódigo

```
ESTRUTURA Vetor2DHomogeneo:
    CAMPOS:
        x, y, w: números reais  // w=1 para pontos, w=0 para vetores
```

**Design:** Terceira coordenada `w` diferencia pontos (transladam) de vetores (não transladam).

---

## 🛠️ Implementações 2D

### 1. Rotação (2D)

#### Pseudocódigo

```
FUNÇÃO criar_matriz_rotacao_2d(angulo):
    """
    Rotação anti-horária ao redor da origem
    
    [cos(θ)  -sen(θ)   0]
    [sen(θ)   cos(θ)   0]
    [  0        0      1]
    """
    mat = criar_matriz_zero_3x3()
    
    c = cosseno(angulo)  // Ângulo em RADIANOS!
    s = seno(angulo)
    
    mat.m[0][0] = c
    mat.m[0][1] = -s
    mat.m[1][0] = s
    mat.m[1][1] = c
    mat.m[2][2] = 1
    
    RETORNAR mat
```

**💡 Insight:** Rotação preserva distâncias e ângulos (transformação rígida).

**🔍 Checkpoint:** 90° anti-horário: `(1,0)` → `(0,1)`.

---

### 2. Translação (2D)

#### Pseudocódigo

```
FUNÇÃO criar_matriz_translacao_2d(tx, ty):
    """
    Move ponto por deslocamento (tx, ty)
    
    [1   0   tx]
    [0   1   ty]
    [0   0   1 ]
    """
    mat = criar_identidade_3x3()
    
    mat.m[0][2] = tx
    mat.m[1][2] = ty
    
    RETORNAR mat
```

**💡 Insight:** Por isso usamos coordenadas homogêneas - translação não é linear em 2D puro!

**🔍 Checkpoint:** Ponto `(2,3,1)` transladado por `(5,7)` → `(7,10,1)`.

---

### 3. Escala (2D)

#### Pseudocódigo

```
FUNÇÃO criar_matriz_escala_2d(sx, sy):
    """
    Escala em X por sx, em Y por sy
    
    [sx   0    0]
    [0    sy   0]
    [0    0    1]
    """
    mat = criar_matriz_zero_3x3()
    
    mat.m[0][0] = sx
    mat.m[1][1] = sy
    mat.m[2][2] = 1
    
    RETORNAR mat
```

**💡 Insight:** Escala uniforme (sx=sy) preserva formas. Não-uniforme distorce.

**🔍 Checkpoint:** Escalar `(2,3)` por `(2,2)` → `(4,6)`.

---

### 4. Cisalhamento (Shear)

#### Pseudocódigo

```
FUNÇÃO criar_matriz_cisalhamento_x(k):
    """
    Cisalhamento horizontal (inclina X baseado em Y)
    
    [1   k   0]
    [0   1   0]
    [0   0   1]
    
    x' = x + k*y
    y' = y
    """
    mat = criar_identidade_3x3()
    mat.m[0][1] = k
    RETORNAR mat

FUNÇÃO criar_matriz_cisalhamento_y(k):
    """Cisalhamento vertical"""
    mat = criar_identidade_3x3()
    mat.m[1][0] = k
    RETORNAR mat
```

**📊 Aplicação:** Transformar retângulo em paralelogramo.

**🔍 Checkpoint:** Cisalhar-X com k=1: `(0,1)` → `(1,1)`.

---

### 5. Reflexão

#### Pseudocódigo

```
FUNÇÃO criar_reflexao_eixo_x():
    """
    Espelhar sobre eixo X (inverter Y)
    
    [1    0   0]
    [0   -1   0]
    [0    0   1]
    """
    mat = criar_identidade_3x3()
    mat.m[1][1] = -1
    RETORNAR mat

FUNÇÃO criar_reflexao_eixo_y():
    """Espelhar sobre eixo Y (inverter X)"""
    mat = criar_identidade_3x3()
    mat.m[0][0] = -1
    RETORNAR mat

FUNÇÃO criar_reflexao_origem():
    """Espelhar sobre origem (inverter ambos)"""
    mat = criar_identidade_3x3()
    mat.m[0][0] = -1
    mat.m[1][1] = -1
    RETORNAR mat
```

**🔍 Checkpoint:** Reflexão sobre X: `(2,3)` → `(2,-3)`.

---

### 6. Composição de Transformações

#### Pseudocódigo

```
FUNÇÃO transformacao_composta_2d(posicao, angulo, escala_x, escala_y):
    """
    Ordem típica: ESCALA → ROTAÇÃO → TRANSLAÇÃO
    
    Matriz resultante M = T * R * S
    Aplicação: ponto_mundo = M * ponto_local
    """
    S = criar_matriz_escala_2d(escala_x, escala_y)
    R = criar_matriz_rotacao_2d(angulo)
    T = criar_matriz_translacao_2d(posicao.x, posicao.y)
    
    // Multiplicar DA DIREITA para ESQUERDA
    temp = multiplicar_matriz(R, S)  // Primeiro R*S
    M = multiplicar_matriz(T, temp)  // Depois T*(R*S)
    
    RETORNAR M
```

**⚠️ Atenção:** Ordem importa! `T*R*S ≠ S*R*T`.

**💡 Insight:** Multiplica-se à direita para aplicar transformações da direita para esquerda: S primeiro, depois R, depois T.

**🔍 Checkpoint:** Escalar 2x, depois rotar 45°, depois transladar (10,0) produz matriz específica.

---

## 🛠️ Implementações 3D

### 7. Rotações 3D (Eixos Principais)

#### Pseudocódigo

```
FUNÇÃO criar_rotacao_eixo_x(angulo):
    """
    Rotação ao redor do eixo X
    Y e Z giram, X fixo
    
    [1     0         0      0]
    [0   cos(θ)  -sen(θ)   0]
    [0   sen(θ)   cos(θ)   0]
    [0     0         0      1]
    """
    mat = criar_identidade_4x4()
    
    c = cosseno(angulo)
    s = seno(angulo)
    
    mat.m[1][1] = c
    mat.m[1][2] = -s
    mat.m[2][1] = s
    mat.m[2][2] = c
    
    RETORNAR mat

FUNÇÃO criar_rotacao_eixo_y(angulo):
    """Rotação ao redor de Y (X e Z giram)"""
    mat = criar_identidade_4x4()
    
    c = cosseno(angulo)
    s = seno(angulo)
    
    mat.m[0][0] = c
    mat.m[0][2] = s  // Sinal POSITIVO! (regra mão direita)
    mat.m[2][0] = -s
    mat.m[2][2] = c
    
    RETORNAR mat

FUNÇÃO criar_rotacao_eixo_z(angulo):
    """Rotação ao redor de Z (X e Y giram, como 2D)"""
    mat = criar_identidade_4x4()
    
    c = cosseno(angulo)
    s = seno(angulo)
    
    mat.m[0][0] = c
    mat.m[0][1] = -s
    mat.m[1][0] = s
    mat.m[1][1] = c
    
    RETORNAR mat
```

**🔍 Checkpoint:** Rotar `(1,0,0)` 90° ao redor de Z → `(0,1,0)`.

---

### 8. Rotação em Eixo Arbitrário (Rodrigues)

#### Pseudocódigo

```
FUNÇÃO criar_rotacao_eixo_arbitrario(eixo, angulo):
    """
    Fórmula de Rodrigues:
    R = I + sen(θ)*K + (1-cos(θ))*K²
    
    onde K é matriz anti-simétrica do eixo
    """
    // Normalizar eixo
    eixo_norm = normalizar(eixo)
    ux = eixo_norm.x
    uy = eixo_norm.y
    uz = eixo_norm.z
    
    c = cosseno(angulo)
    s = seno(angulo)
    um_menos_c = 1 - c
    
    mat = criar_matriz_zero_4x4()
    
    mat.m[0][0] = c + ux*ux*um_menos_c
    mat.m[0][1] = ux*uy*um_menos_c - uz*s
    mat.m[0][2] = ux*uz*um_menos_c + uy*s
    
    mat.m[1][0] = uy*ux*um_menos_c + uz*s
    mat.m[1][1] = c + uy*uy*um_menos_c
    mat.m[1][2] = uy*uz*um_menos_c - ux*s
    
    mat.m[2][0] = uz*ux*um_menos_c - uy*s
    mat.m[2][1] = uz*uy*um_menos_c + ux*s
    mat.m[2][2] = c + uz*uz*um_menos_c
    
    mat.m[3][3] = 1
    
    RETORNAR mat
```

**💡 Insight:** Usado em animação 3D, física de corpos rígidos.

---

### 9. Look-At Matrix (Câmera)

#### Pseudocódigo

```
FUNÇÃO criar_look_at(olho, alvo, cima):
    """
    Cria matriz de visualização (view matrix)
    
    olho: posição da câmera
    alvo: ponto para onde a câmera olha
    cima: vetor "up" aproximado (geralmente (0,1,0))
    """
    // Calcular base ortonormal da câmera
    z = normalizar(subtrair(olho, alvo))  // Direção CONTRÁRIA ao olhar
    x = normalizar(produto_vetorial(cima, z))
    y = produto_vetorial(z, x)
    
    // Montar matriz de rotação (inversa)
    mat = criar_identidade_4x4()
    
    mat.m[0][0] = x.x;  mat.m[0][1] = x.y;  mat.m[0][2] = x.z
    mat.m[1][0] = y.x;  mat.m[1][1] = y.y;  mat.m[1][2] = y.z
    mat.m[2][0] = z.x;  mat.m[2][1] = z.y;  mat.m[2][2] = z.z
    
    // Translação (mover câmera para origem)
    mat.m[0][3] = -produto_escalar(x, olho)
    mat.m[1][3] = -produto_escalar(y, olho)
    mat.m[2][3] = -produto_escalar(z, olho)
    
    RETORNAR mat
```

**📊 Aplicação:** Pipeline gráfico: `MVP = Projection * View * Model`.

**🔍 Checkpoint:** Câmera em `(0,0,5)` olhando para origem produz matriz específica.

---

## 🐛 Debugging Comum

### Problema 1: Radianos vs Graus

```
❌ ERRADO:
rotacao = criar_rotacao_2d(90)  // Assume graus, mas função usa radianos!

✅ CORRETO:
DEFINE PI = 3.14159265359
DEFINE GRAUS_PARA_RAD(graus) = graus * PI / 180

rotacao = criar_rotacao_2d(GRAUS_PARA_RAD(90))
```

---

### Problema 2: Ordem de Multiplicação

```
❌ ERRADO:
M = multiplicar(S, multiplicar(R, T))  // Errado! T primeiro, depois R, depois S

✅ CORRETO:
// Para aplicar T → R → S (nessa ordem temporal):
M = multiplicar(multiplicar(S, R), T)
// Ou mais claro:
M = S * R * T
// Aplica da DIREITA para ESQUERDA: T primeiro, R segundo, S terceiro
```

---

### Problema 3: Coordenada Homogênea Esquecida

```
❌ ERRADO:
ponto_2d = {x, y}  // Apenas 2 componentes
transformado = multiplicar_matriz_vetor(mat_3x3, ponto_2d)  // ERRO!

✅ CORRETO:
ponto_homogeneo = {x, y, 1}  // Ponto (w=1)
vetor_homogeneo = {dx, dy, 0}  // Vetor direcional (w=0, não translada)
transformado = multiplicar_matriz_vetor(mat_3x3, ponto_homogeneo)
```

---

### Problema 4: Não Normalizar Eixo de Rotação

```
❌ ERRADO:
eixo = {1, 2, 3}  // Não unitário!
rot = criar_rotacao_eixo_arbitrario(eixo, angulo)  // Resultado ERRADO

✅ CORRETO:
eixo = normalizar({1, 2, 3})
rot = criar_rotacao_eixo_arbitrario(eixo, angulo)
```

---

## 🧪 Testes Unitários

```
TESTE rotacao_90_graus:
    rot = criar_rotacao_2d(PI / 2)  // 90° em radianos
    ponto = {1, 0, 1}  // Ponto (1,0) em homogêneas
    resultado = multiplicar_matriz_vetor(rot, ponto)
    ASSERT abs(resultado.x - 0) < EPSILON
    ASSERT abs(resultado.y - 1) < EPSILON

TESTE translacao:
    trans = criar_translacao_2d(10, 20)
    ponto = {5, 3, 1}
    resultado = multiplicar_matriz_vetor(trans, ponto)
    ASSERT resultado.x == 15
    ASSERT resultado.y == 23

TESTE escala_uniforme:
    escala = criar_escala_2d(3, 3)
    ponto = {2, 5, 1}
    resultado = multiplicar_matriz_vetor(escala, ponto)
    ASSERT resultado.x == 6
    ASSERT resultado.y == 15

TESTE composicao_ordem:
    S = criar_escala_2d(2, 2)
    T = criar_translacao_2d(10, 0)
    
    // T depois S: escala 2x, depois move 10
    M1 = multiplicar(T, S)
    ponto = {1, 0, 1}
    r1 = multiplicar_matriz_vetor(M1, ponto)  // (1*2)+10 = 12
    
    // S depois T: move 10, depois escala 2x
    M2 = multiplicar(S, T)
    r2 = multiplicar_matriz_vetor(M2, ponto)  // (1+10)*2 = 22
    
    ASSERT r1.x != r2.x  // Ordens diferentes!
```

---

## 📚 Exercícios de Implementação

1. **Transformação 2D interativa:** Aplicar T,R,S com sliders
2. **Orbitar câmera:** Look-at ao redor de ponto fixo
3. **Interpolação de transformações (LERP):** Animar entre duas poses
4. **Sistema de hierarquia:** Transformações pai-filho (esqueleto)

---

## 🎯 Próximos Passos

1. ✅ **Decomposições:** `i4-decomposicoes.md`
2. ✅ **Projeto:** Motor 2D com transformações (`k5-projeto/`)
3. ✅ **Avançado:** Quaternions para rotações 3D estáveis

---

## 💡 Aplicações Reais

- **Gráficos 3D:** Pipeline MVP (Model-View-Projection)
- **Jogos:** Movimentação de objetos, câmeras, animações
- **Robótica:** Cinemática direta/inversa (transformações de juntas)
- **CAD:** Modelagem 3D com operações de transformação

---

<details>
<summary><strong>💻 Implementação Completa em C</strong></summary>

```c
#include <stdio.h>
#include <math.h>
#include <string.h>

#define PI 3.14159265359
#define DEG_TO_RAD(deg) ((deg) * PI / 180.0)

typedef struct { double m[3][3]; } Mat3;
typedef struct { double m[4][4]; } Mat4;
typedef struct { double x, y, z; } Vec3;

// Matriz 3×3 identidade
Mat3 mat3_identity() {
    Mat3 m;
    memset(&m, 0, sizeof(Mat3));
    m.m[0][0] = m.m[1][1] = m.m[2][2] = 1.0;
    return m;
}

// Rotação 2D (em homogêneas 3×3)
Mat3 mat3_rotation_2d(double angle_rad) {
    Mat3 m = mat3_identity();
    double c = cos(angle_rad);
    double s = sin(angle_rad);
    m.m[0][0] = c;  m.m[0][1] = -s;
    m.m[1][0] = s;  m.m[1][1] = c;
    return m;
}

// Translação 2D
Mat3 mat3_translation_2d(double tx, double ty) {
    Mat3 m = mat3_identity();
    m.m[0][2] = tx;
    m.m[1][2] = ty;
    return m;
}

// Escala 2D
Mat3 mat3_scale_2d(double sx, double sy) {
    Mat3 m = {0};
    m.m[0][0] = sx;
    m.m[1][1] = sy;
    m.m[2][2] = 1.0;
    return m;
}

// Multiplicação 3×3
Mat3 mat3_mul(Mat3 a, Mat3 b) {
    Mat3 r = {0};
    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++)
            for (int k = 0; k < 3; k++)
                r.m[i][j] += a.m[i][k] * b.m[k][j];
    return r;
}

void mat3_print(Mat3 m) {
    for (int i = 0; i < 3; i++) {
        printf("[ ");
        for (int j = 0; j < 3; j++)
            printf("%7.3f ", m.m[i][j]);
        printf("]\n");
    }
}

// Matriz 4×4 identidade
Mat4 mat4_identity() {
    Mat4 m;
    memset(&m, 0, sizeof(Mat4));
    m.m[0][0] = m.m[1][1] = m.m[2][2] = m.m[3][3] = 1.0;
    return m;
}

// Rotação 3D ao redor do eixo Z
Mat4 mat4_rotation_z(double angle_rad) {
    Mat4 m = mat4_identity();
    double c = cos(angle_rad);
    double s = sin(angle_rad);
    m.m[0][0] = c;  m.m[0][1] = -s;
    m.m[1][0] = s;  m.m[1][1] = c;
    return m;
}

void mat4_print(Mat4 m) {
    for (int i = 0; i < 4; i++) {
        printf("[ ");
        for (int j = 0; j < 4; j++)
            printf("%7.3f ", m.m[i][j]);
        printf("]\n");
    }
}

int main() {
    // Teste 2D: Composição T*R*S
    printf("=== Transformação 2D Composta ===\n");
    
    Mat3 S = mat3_scale_2d(2.0, 2.0);
    Mat3 R = mat3_rotation_2d(DEG_TO_RAD(45));
    Mat3 T = mat3_translation_2d(10, 5);
    
    Mat3 M = mat3_mul(T, mat3_mul(R, S));  // T*(R*S)
    
    printf("Matriz final (T*R*S):\n");
    mat3_print(M);
    
    // Teste 3D: Rotação ao redor de Z
    printf("\n=== Rotação 3D (90° em Z) ===\n");
    Mat4 rot_z = mat4_rotation_z(DEG_TO_RAD(90));
    mat4_print(rot_z);
    
    return 0;
}
```

**Compilar:** `gcc -o transformacoes i3-transformacoes.c -lm`

</details>

---

**XP Disponível:** 75 XP  
**Tempo estimado:** 3h-4h  
**Dificuldade:** ⭐⭐⭐⭐
