# Implementação: Matrizes

## 🎯 Meta

Implementar operações fundamentais com matrizes, incluindo multiplicação, transposta, determinante e inversa, entendendo a base das transformações lineares.

---

## ⏱️ Tempo Estimado

- **Leitura + Pseudocódigo:** 40-50 min
- **Implementação em linguagem real:** 1h30-2h
- **Testes e debugging:** 30-45 min
- **Total:** ~2h30-3h30

---

## 📋 Pré-requisitos

- Leitura de `k1-teoria/t2-transformacoes-matrizes.md`
- Implementação de `i1-vetores.md`
- Exercícios `k2-exercicios/e2-transformacoes-exercicios.md` (Nível 1)

---

## 🎚️ Dificuldade

⭐⭐⭐ Intermediário

---

## 📐 Conceitos-Chave

1. **Matriz:** Tabela retangular de números `A[m×n]`
2. **Multiplicação:** `(AB)ij = Σk Aik * Bkj` (O(n³)!)
3. **Transposta:** `Aᵀij = Aji` (trocar linhas/colunas)
4. **Determinante:** Escalar que indica "volume" da transformação
5. **Inversa:** `A⁻¹` tal que `AA⁻¹ = I`

---

## 🧩 Estrutura de Dados

### Pseudocódigo

```
ESTRUTURA Matriz2x2:
    CAMPOS:
        m: array[2][2] de números reais

ESTRUTURA Matriz3x3:
    CAMPOS:
        m: array[3][3] de números reais

ESTRUTURA Matriz4x4:
    CAMPOS:
        m: array[4][4] de números reais
```

**Design:** Arrays bidimensionais em row-major order (linhas contíguas na memória). Em C: `m[linha][coluna]`.

---

## 🛠️ Implementações

### 1. Matriz Identidade

#### Pseudocódigo

```
FUNÇÃO criar_identidade_3x3():
    """Cria matriz identidade I (diagonal = 1, resto = 0)"""
    mat = criar_matriz_zero_3x3()
    
    PARA i DE 0 ATÉ 2:
        mat.m[i][i] = 1.0
    
    RETORNAR mat
```

**🔍 Checkpoint:** Identidade 3×3: diagonal com 1s, resto zeros.

---

### 2. Multiplicação Matriz × Vetor

#### Pseudocódigo

```
FUNÇÃO multiplicar_matriz_vetor_3x3(mat, v):
    """
    Multiplica matriz 3×3 por vetor 3D
    Resultado: novo vetor transformado
    """
    resultado = criar_vetor(0, 0, 0)
    
    PARA i DE 0 ATÉ 2:
        resultado[i] = 0
        PARA j DE 0 ATÉ 2:
            resultado[i] += mat.m[i][j] * v[j]
    
    RETORNAR resultado
```

**💡 Insight:** Cada elemento do resultado é o produto escalar entre uma linha da matriz e o vetor.

**🔍 Checkpoint:** Multiplicar identidade por `(5,6,7)` retorna `(5,6,7)`.

---

### 3. Multiplicação Matriz × Matriz

#### Pseudocódigo

```
FUNÇÃO multiplicar_matriz_matriz_3x3(A, B):
    """
    Multiplica A × B
    (AB)ij = soma de Aik * Bkj para k de 0 a 2
    
    ATENÇÃO: AB ≠ BA (não comutativa!)
    """
    resultado = criar_matriz_zero_3x3()
    
    PARA i DE 0 ATÉ 2:  // Linhas de A
        PARA j DE 0 ATÉ 2:  // Colunas de B
            soma = 0
            PARA k DE 0 ATÉ 2:  // Índice interno
                soma += A.m[i][k] * B.m[k][j]
            resultado.m[i][j] = soma
    
    RETORNAR resultado
```

**⚠️ Complexidade:** O(n³) - muito caro! 3 loops aninhados.

**🔍 Checkpoint:** Multiplicar identidade por qualquer matriz retorna a mesma matriz.

---

### 4. Transposta

#### Pseudocódigo

```
FUNÇÃO transpor_3x3(mat):
    """Troca linhas por colunas: (Aᵀ)ij = Aji"""
    resultado = criar_matriz_zero_3x3()
    
    PARA i DE 0 ATÉ 2:
        PARA j DE 0 ATÉ 2:
            resultado.m[i][j] = mat.m[j][i]
    
    RETORNAR resultado
```

**💡 Insight:** `(AB)ᵀ = BᵀAᵀ` (ordem inverte!).

**🔍 Checkpoint:** Transpor matriz 2 vezes retorna original.

---

### 5. Determinante 2×2

#### Pseudocódigo

```
FUNÇÃO determinante_2x2(mat):
    """
    det([a b]) = ad - bc
        [c d]
    """
    a = mat.m[0][0]
    b = mat.m[0][1]
    c = mat.m[1][0]
    d = mat.m[1][1]
    
    RETORNAR a*d - b*c
```

**📊 Interpretação geométrica:** Área do paralelogramo formado pelas colunas.

**🔍 Checkpoint:** `det(I) = 1`, `det(matriz_singular) = 0`.

---

### 6. Determinante 3×3 (Regra de Sarrus)

#### Pseudocódigo

```
FUNÇÃO determinante_3x3(mat):
    """
    Regra de Sarrus (produtos de diagonais)
    Ou expansão por cofatores (mais geral)
    """
    a = mat.m[0][0]
    b = mat.m[0][1]
    c = mat.m[0][2]
    d = mat.m[1][0]
    e = mat.m[1][1]
    f = mat.m[1][2]
    g = mat.m[2][0]
    h = mat.m[2][1]
    i = mat.m[2][2]
    
    // Diagonais positivas - diagonais negativas
    positivo = a*e*i + b*f*g + c*d*h
    negativo = c*e*g + b*d*i + a*f*h
    
    RETORNAR positivo - negativo
```

**🔍 Checkpoint:** Determinante de matriz diagonal = produto dos elementos da diagonal.

---

### 7. Inversa 2×2

#### Pseudocódigo

```
FUNÇÃO inverter_2x2(mat):
    """
    A⁻¹ = (1/det) * [d  -b]
                     [-c  a]
    """
    det = determinante_2x2(mat)
    
    SE abs(det) < EPSILON:
        ERRO "Matriz singular (não invertível)"
    
    a = mat.m[0][0]
    b = mat.m[0][1]
    c = mat.m[1][0]
    d = mat.m[1][1]
    
    resultado = criar_matriz_zero_2x2()
    resultado.m[0][0] = d / det
    resultado.m[0][1] = -b / det
    resultado.m[1][0] = -c / det
    resultado.m[1][1] = a / det
    
    RETORNAR resultado
```

**💡 Insight:** Inverter matriz é como "desfazer" uma transformação.

**🔍 Checkpoint:** `A * A⁻¹ = I`.

---

### 8. Inversa 3×3 (Matriz de Cofatores)

#### Pseudocódigo

```
FUNÇÃO inverter_3x3(mat):
    """
    A⁻¹ = (1/det) * adj(A)
    onde adj(A) = matriz de cofatores transposta
    """
    det = determinante_3x3(mat)
    
    SE abs(det) < EPSILON:
        ERRO "Matriz singular"
    
    // Calcular matriz de cofatores (9 determinantes 2×2)
    cofatores = criar_matriz_zero_3x3()
    
    PARA i DE 0 ATÉ 2:
        PARA j DE 0 ATÉ 2:
            // Extrair submatriz 2×2 removendo linha i e coluna j
            sub = extrair_submatriz(mat, i, j)
            cofator = determinante_2x2(sub)
            
            // Aplicar sinal alternado (tabuleiro de xadrez)
            SE (i + j) % 2 == 1:
                cofator = -cofator
            
            cofatores.m[i][j] = cofator
    
    // Transpor cofatores (adjunta)
    adjunta = transpor_3x3(cofatores)
    
    // Dividir por determinante
    PARA i DE 0 ATÉ 2:
        PARA j DE 0 ATÉ 2:
            adjunta.m[i][j] /= det
    
    RETORNAR adjunta
```

**⚠️ Atenção:** Método custoso! Em produção usar LU ou algoritmos otimizados.

**🔍 Checkpoint:** Inverter matriz de rotação retorna rotação oposta.

---

## 🐛 Debugging Comum

### Problema 1: Ordem da Multiplicação

```
❌ ERRADO:
// AB ≠ BA!
matriz_escala_primeiro = multiplicar(escala, rotacao)
matriz_rotacao_primeiro = multiplicar(rotacao, escala)
// Resultados DIFERENTES!

✅ CORRETO:
// Multiplicação à DIREITA aplica primeira
// M = Translate * Rotate * Scale
// Vetor é transformado: Scale → Rotate → Translate
ponto_transformado = multiplicar(M, ponto_original)
```

---

### Problema 2: Row-Major vs Column-Major

```
❌ ERRADO (acesso inconsistente):
m[coluna][linha]  // Confuso!

✅ CORRETO:
m[linha][coluna]  // C/C++ padrão (row-major)

// OpenGL usa column-major! Transpor ao enviar dados:
glUniformMatrix4fv(loc, 1, GL_TRUE, &matrix);  // GL_TRUE transpõe
```

---

### Problema 3: Determinante Zero

```
❌ ERRADO:
SE determinante == 0:  // Float nunca exatamente zero

✅ CORRETO:
DEFINE EPSILON = 1e-10
SE abs(determinante) < EPSILON:
    // Matriz singular (provavelmente)
```

---

## 🧪 Testes Unitários

```
TESTE identidade:
    I = criar_identidade_3x3()
    v = criar_vetor(2, 3, 5)
    resultado = multiplicar_matriz_vetor(I, v)
    ASSERT resultado == v

TESTE multiplicacao_associativa:
    // (AB)C = A(BC)
    AB_C = multiplicar(multiplicar(A, B), C)
    A_BC = multiplicar(A, multiplicar(B, C))
    ASSERT matrizes_iguais(AB_C, A_BC)

TESTE transposta_dupla:
    mat_original = criar_matriz_qualquer()
    mat_transposta_2x = transpor(transpor(mat_original))
    ASSERT matrizes_iguais(mat_original, mat_transposta_2x)

TESTE inversa:
    A = criar_matriz_invertivel()
    A_inv = inverter(A)
    I = multiplicar(A, A_inv)
    ASSERT matriz_eh_identidade(I)

TESTE determinante_diagonal:
    D = criar_matriz_zero_3x3()
    D.m[0][0] = 2
    D.m[1][1] = 3
    D.m[2][2] = 5
    ASSERT abs(determinante_3x3(D) - 30) < EPSILON  // 2*3*5
```

---

## 📚 Exercícios de Implementação

1. **Matriz 4×4** para transformações 3D com coordenadas homogêneas
2. **Multiplicação otimizada** com loop unrolling (desenrolar loops)
3. **Decomposição LU** (fatoração em triangular inferior × superior)
4. **Verificador de simetria** (A = Aᵀ?)

---

## 🎯 Próximos Passos

1. ✅ **Transformações:** `i3-transformacoes.md` (aplicar matrizes!)
2. ✅ **Decomposições:** `i4-decomposicoes.md` (algoritmos avançados)
3. ✅ **Projeto:** Sistema de transformações 2D/3D

---

## 💡 Aplicações Reais

- **Gráficos:** Transformações de objetos, projeção perspectiva
- **Física:** Tensores de inércia, sistemas de equações (Ax = b)
- **ML:** Multiplicação matriz-vetor em redes neurais (forward pass)
- **Criptografia:** Cifras de Hill (multiplicação modular)

---

<details>
<summary><strong>💻 Implementação Completa em C</strong></summary>

```c
#include <stdio.h>
#include <math.h>
#include <string.h>

#define EPSILON 1e-10

typedef struct {
    double m[3][3];
} Mat3;

typedef struct {
    double x, y, z;
} Vec3;

// Criar matriz identidade
Mat3 mat3_identity() {
    Mat3 result;
    memset(&result, 0, sizeof(Mat3));
    for (int i = 0; i < 3; i++) {
        result.m[i][i] = 1.0;
    }
    return result;
}

// Multiplicação matriz × vetor
Vec3 mat3_mul_vec(Mat3 mat, Vec3 v) {
    Vec3 result;
    result.x = mat.m[0][0]*v.x + mat.m[0][1]*v.y + mat.m[0][2]*v.z;
    result.y = mat.m[1][0]*v.x + mat.m[1][1]*v.y + mat.m[1][2]*v.z;
    result.z = mat.m[2][0]*v.x + mat.m[2][1]*v.y + mat.m[2][2]*v.z;
    return result;
}

// Multiplicação matriz × matriz
Mat3 mat3_mul(Mat3 a, Mat3 b) {
    Mat3 result;
    memset(&result, 0, sizeof(Mat3));
    
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            for (int k = 0; k < 3; k++) {
                result.m[i][j] += a.m[i][k] * b.m[k][j];
            }
        }
    }
    return result;
}

// Transposta
Mat3 mat3_transpose(Mat3 m) {
    Mat3 result;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            result.m[i][j] = m.m[j][i];
        }
    }
    return result;
}

// Determinante 3×3
double mat3_det(Mat3 m) {
    double a = m.m[0][0], b = m.m[0][1], c = m.m[0][2];
    double d = m.m[1][0], e = m.m[1][1], f = m.m[1][2];
    double g = m.m[2][0], h = m.m[2][1], i = m.m[2][2];
    
    return a*e*i + b*f*g + c*d*h - c*e*g - b*d*i - a*f*h;
}

// Inversa 3×3 (método da adjunta)
Mat3 mat3_inverse(Mat3 m) {
    double det = mat3_det(m);
    
    if (fabs(det) < EPSILON) {
        printf("ERRO: Matriz singular (det ≈ 0)!\n");
        return mat3_identity();
    }
    
    Mat3 inv;
    
    // Calcular cofatores manualmente (otimização: evitar loops)
    inv.m[0][0] = (m.m[1][1]*m.m[2][2] - m.m[1][2]*m.m[2][1]) / det;
    inv.m[0][1] = (m.m[0][2]*m.m[2][1] - m.m[0][1]*m.m[2][2]) / det;
    inv.m[0][2] = (m.m[0][1]*m.m[1][2] - m.m[0][2]*m.m[1][1]) / det;
    
    inv.m[1][0] = (m.m[1][2]*m.m[2][0] - m.m[1][0]*m.m[2][2]) / det;
    inv.m[1][1] = (m.m[0][0]*m.m[2][2] - m.m[0][2]*m.m[2][0]) / det;
    inv.m[1][2] = (m.m[0][2]*m.m[1][0] - m.m[0][0]*m.m[1][2]) / det;
    
    inv.m[2][0] = (m.m[1][0]*m.m[2][1] - m.m[1][1]*m.m[2][0]) / det;
    inv.m[2][1] = (m.m[0][1]*m.m[2][0] - m.m[0][0]*m.m[2][1]) / det;
    inv.m[2][2] = (m.m[0][0]*m.m[1][1] - m.m[0][1]*m.m[1][0]) / det;
    
    return inv;
}

void mat3_print(Mat3 m) {
    for (int i = 0; i < 3; i++) {
        printf("[ ");
        for (int j = 0; j < 3; j++) {
            printf("%7.3f ", m.m[i][j]);
        }
        printf("]\n");
    }
}

int main() {
    // Teste 1: Identidade
    Mat3 I = mat3_identity();
    printf("Identidade:\n");
    mat3_print(I);
    
    // Teste 2: Multiplicação matriz × vetor
    Vec3 v = {2, 3, 5};
    Vec3 v2 = mat3_mul_vec(I, v);
    printf("\nI * (2,3,5) = (%.2f, %.2f, %.2f)\n", v2.x, v2.y, v2.z);
    
    // Teste 3: Determinante
    Mat3 A;
    A.m[0][0]=1; A.m[0][1]=2; A.m[0][2]=3;
    A.m[1][0]=0; A.m[1][1]=1; A.m[1][2]=4;
    A.m[2][0]=5; A.m[2][1]=6; A.m[2][2]=0;
    printf("\nMatriz A:\n");
    mat3_print(A);
    printf("det(A) = %.3f\n", mat3_det(A));
    
    // Teste 4: Inversa
    Mat3 A_inv = mat3_inverse(A);
    printf("\nA⁻¹:\n");
    mat3_print(A_inv);
    
    Mat3 produto = mat3_mul(A, A_inv);
    printf("\nA * A⁻¹ (deve ser I):\n");
    mat3_print(produto);
    
    return 0;
}
```

**Compilar:** `gcc -o matrizes i2-matrizes.c -lm`

</details>

---

**XP Disponível:** 60 XP  
**Tempo estimado:** 2h30-3h30  
**Dificuldade:** ⭐⭐⭐
