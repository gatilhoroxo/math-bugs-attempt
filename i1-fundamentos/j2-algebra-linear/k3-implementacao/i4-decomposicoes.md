# Implementação: Decomposições Matriciais

## 🎯 Meta

Implementar decomposições LU, QR e Cholesky, além de algoritmos de resolução de sistemas lineares e mínimos quadrados.

---

## ⏱️ Tempo Estimado

- **Leitura + Pseudocódigo:** 60-75 min
- **Implementação em linguagem real:** 2h-2h45
- **Testes e debugging:** 45-60 min
- **Total:** ~3h30-5h

---

## 📋 Pré-requisitos

- Leitura de `k1-teoria/t4-decomposicoes.md`
- Implementação de `i2-matrizes.md`
- Exercícios `k2-exercicios/e4-decomposicoes-exercicios.md` (Nível 1-2)

---

## 🎚️ Dificuldade

⭐⭐⭐⭐⭐ Muito Avançado

---

## 📐 Conceitos-Chave

1. **LU:** `A = LU` (triangular inferior × superior) - resolver múltiplos sistemas
2. **QR:** `A = QR` (ortogonal × triangular superior) - mínimos quadrados
3. **Cholesky:** `A = LLᵀ` para matrizes simétricas positivas definidas (metade do custo de LU)
4. **SVD:** `A = UΣVᵀ` (decomposição em valores singulares) - aplicações universais
5. **Pivotamento:** Trocar linhas para evitar divisão por zero ou instabilidade numérica

---

## 🧩 Estrutura de Dados

### Pseudocódigo

```
ESTRUTURA ResultadoLU:
    CAMPOS:
        L: matriz triangular inferior
        U: matriz triangular superior
        permutacao: array de índices de linhas trocadas
        sucesso: booleano

ESTRUTURA ResultadoQR:
    CAMPOS:
        Q: matriz ortogonal
        R: matriz triangular superior
```

---

## 🛠️ Implementações

### 1. Decomposição LU com Pivotamento

#### Pseudocódigo

```
FUNÇÃO decompor_LU_pivotamento(A, n):
    """
    Decompõe A em PA = LU
    
    P = matriz de permutação (registro de trocas de linhas)
    L = triangular inferior (diagonal = 1)
    U = triangular superior
    """
    // Copiar A para U (será modificada)
    U = copiar_matriz(A, n)
    
    // Inicializar L como identidade
    L = criar_identidade(n)
    
    // Registro de permutações
    perm = [0, 1, 2, ..., n-1]
    
    PARA k DE 0 ATÉ n-2:  // Para cada coluna (exceto última)
        // === PIVOTAMENTO PARCIAL ===
        // Encontrar linha com maior |U[i][k]| em coluna k
        pivo_linha = k
        pivo_max = abs(U[k][k])
        
        PARA i DE k+1 ATÉ n-1:
            SE abs(U[i][k]) > pivo_max:
                pivo_max = abs(U[i][k])
                pivo_linha = i
        
        // Verificar singularidade
        SE pivo_max < EPSILON:
            RETORNAR erro "Matriz singular"
        
        // Trocar linhas k e pivo_linha
        SE pivo_linha != k:
            trocar_linhas(U, k, pivo_linha)
            trocar_linhas(L, k, pivo_linha, apenas_até_coluna=k-1)
            trocar(perm[k], perm[pivo_linha])
        
        // === ELIMINAÇÃO GAUSSIANA ===
        PARA i DE k+1 ATÉ n-1:  // Linhas abaixo do pivô
            multiplicador = U[i][k] / U[k][k]
            L[i][k] = multiplicador  // Guardar multiplicador em L
            
            // Atualizar linha i: linha_i -= mult * linha_k
            PARA j DE k ATÉ n-1:
                U[i][j] -= multiplicador * U[k][j]
    
    RETORNAR {L, U, perm, sucesso=verdadeiro}
```

**💡 Insight:** Pivotamento é ESSENCIAL para estabilidade numérica. Sem ele, erros de arredondamento explodem.

**🔍 Checkpoint:** Após k iterações, primeiras k colunas de L completas, primeiras k+1 linhas de U completas.

---

### 2. Resolver Sistema com LU

#### Pseudocódigo

```
FUNÇÃO resolver_com_LU(L, U, perm, b, n):
    """
    Resolve Ax = b usando decomposição PA = LU
    
    Passos:
    1. Permutar b: b' = Pb
    2. Resolver Ly = b' (substituição progressiva)
    3. Resolver Ux = y (substituição regressiva)
    """
    // Passo 1: Permutar b
    b_perm = criar_vetor(n)
    PARA i DE 0 ATÉ n-1:
        b_perm[i] = b[perm[i]]
    
    // Passo 2: Substituição progressiva (forward substitution)
    y = criar_vetor(n)
    PARA i DE 0 ATÉ n-1:
        soma = 0
        PARA j DE 0 ATÉ i-1:
            soma += L[i][j] * y[j]
        y[i] = b_perm[i] - soma  // L[i][i] = 1
    
    // Passo 3: Substituição regressiva (backward substitution)
    x = criar_vetor(n)
    PARA i DE n-1 ATÉ 0 (DECRESCENTE):
        soma = 0
        PARA j DE i+1 ATÉ n-1:
            soma += U[i][j] * x[j]
        x[i] = (y[i] - soma) / U[i][i]
    
    RETORNAR x
```

**💡 Insight:** Custo de fatoração LU é O(n³), mas cada solução adicional é apenas O(n²). Ótimo para múltiplos vetores b!

**🔍 Checkpoint:** Resolver `Ly = b` com L triangular inferior deve dar valores corretos de y.

---

### 3. Decomposição QR (Gram-Schmidt)

#### Pseudocódigo

```
FUNÇÃO decompor_QR_gram_schmidt(A, m, n):
    """
    Decompõe A (m×n) em A = QR
    
    Q (m×n): colunas ortonormais
    R (n×n): triangular superior
    
    Algoritmo: Gram-Schmidt modificado
    """
    Q = criar_matriz_zero(m, n)
    R = criar_matriz_zero(n, n)
    
    PARA j DE 0 ATÉ n-1:  // Para cada coluna de A
        // Copiar coluna j de A para vetor temporário
        v = copiar_coluna(A, j)
        
        // Ortogonalizar contra colunas anteriores de Q
        PARA i DE 0 ATÉ j-1:
            // R[i][j] = Q_i · A_j (projeção)
            R[i][j] = 0
            PARA k DE 0 ATÉ m-1:
                R[i][j] += Q[k][i] * A[k][j]
            
            // Subtrair projeção: v = v - R[i][j] * Q_i
            PARA k DE 0 ATÉ m-1:
                v[k] -= R[i][j] * Q[k][i]
        
        // Normalizar v
        R[j][j] = 0
        PARA k DE 0 ATÉ m-1:
            R[j][j] += v[k] * v[k]
        R[j][j] = raiz_quadrada(R[j][j])
        
        // Q_j = v / ||v||
        PARA k DE 0 ATÉ m-1:
            Q[k][j] = v[k] / R[j][j]
    
    RETORNAR {Q, R}
```

**⚠️ Atenção:** Gram-Schmidt clássico pode ser numericamente instável. Gram-Schmidt modificado é melhor (ortogonaliza contra Q, não A).

**🔍 Checkpoint:** Colunas de Q devem ser ortonormais: `Q[:,i] · Q[:,j] = 0` se i≠j, `= 1` se i=j.

---

### 4. Mínimos Quadrados via QR

#### Pseudocódigo

```
FUNÇÃO minimos_quadrados_QR(A, b, m, n):
    """
    Resolve min ||Ax - b||² para sistema sobre-determinado (m > n)
    
    Método: usar QR de A
    Ax = b → QRx = b → Rx = Qᵀb
    """
    resultado_qr = decompor_QR_gram_schmidt(A, m, n)
    Q = resultado_qr.Q
    R = resultado_qr.R
    
    // Calcular c = Qᵀb
    c = criar_vetor(n)
    PARA i DE 0 ATÉ n-1:
        c[i] = 0
        PARA j DE 0 ATÉ m-1:
            c[i] += Q[j][i] * b[j]
    
    // Resolver Rx = c (sistema triangular superior n×n)
    x = criar_vetor(n)
    PARA i DE n-1 ATÉ 0 (DECRESCENTE):
        soma = 0
        PARA j DE i+1 ATÉ n-1:
            soma += R[i][j] * x[j]
        x[i] = (c[i] - soma) / R[i][i]
    
    RETORNAR x
```

**📊 Aplicação:** Ajuste de curvas (regressão linear), calibração de sensores.

**💡 Insight:** QR evita formar `AᵀA` (equações normais), que pode ter número de condição ruim.

**🔍 Checkpoint:** Para dados `(0,1), (1,2), (2,2)` e modelo `y = a + bx`, solução deve aproximar pontos.

---

### 5. Decomposição de Cholesky

#### Pseudocódigo

```
FUNÇÃO decompor_cholesky(A, n):
    """
    Decompõe matriz SIMÉTRICA POSITIVA DEFINIDA em A = LLᵀ
    
    L = triangular inferior
    
    Custo: ~n³/3 (metade de LU!)
    """
    L = criar_matriz_zero(n, n)
    
    PARA i DE 0 ATÉ n-1:
        PARA j DE 0 ATÉ i:  // Apenas metade inferior
            SE j == i:  // Elemento diagonal
                soma = 0
                PARA k DE 0 ATÉ j-1:
                    soma += L[j][k] * L[j][k]
                
                valor = A[j][j] - soma
                
                SE valor <= 0:
                    RETORNAR erro "Matriz não é positiva definida"
                
                L[j][j] = raiz_quadrada(valor)
            
            SENÃO:  // Elemento fora da diagonal
                soma = 0
                PARA k DE 0 ATÉ j-1:
                    soma += L[i][k] * L[j][k]
                
                L[i][j] = (A[i][j] - soma) / L[j][j]
    
    RETORNAR L
```

**💡 Insight:** Cholesky falha se matriz não for positiva definida. Use isso como teste!

**🔍 Checkpoint:** `L * Lᵀ` deve reconstruir A original.

---

### 6. Determinante via LU

#### Pseudocódigo

```
FUNÇÃO determinante_via_LU(A, n):
    """
    det(A) = det(P) * det(L) * det(U)
    
    det(L) = 1 (diagonal = 1)
    det(U) = produto da diagonal
    det(P) = (-1)^número_de_trocas
    """
    resultado_lu = decompor_LU_pivotamento(A, n)
    
    SE NÃO resultado_lu.sucesso:
        RETORNAR 0  // Matriz singular
    
    // Contar número de trocas de linhas
    trocas = 0
    PARA i DE 0 ATÉ n-1:
        SE resultado_lu.perm[i] != i:
            trocas += 1
    trocas = trocas / 2  // Cada troca conta 2x no loop acima
    
    // det(P) = (-1)^trocas
    sinal = (trocas % 2 == 0) ? 1 : -1
    
    // det(U) = produto da diagonal
    det_U = 1.0
    PARA i DE 0 ATÉ n-1:
        det_U *= resultado_lu.U[i][i]
    
    RETORNAR sinal * det_U
```

**🔍 Checkpoint:** Determinante de identidade é 1.

---

## 🐛 Debugging Comum

### Problema 1: Esquecer Pivotamento

```
❌ ERRADO:
FUNÇÃO decompor_LU_sem_pivo(A):
    PARA k DE 0 ATÉ n-2:
        // Usar A[k][k] direto como pivô
        PARA i DE k+1 ATÉ n-1:
            mult = A[i][k] / A[k][k]  // CRASH se A[k][k]=0!
            ...

✅ CORRETO:
FUNÇÃO decompor_LU_com_pivo(A):
    PARA k DE 0 ATÉ n-2:
        // Encontrar maior elemento em coluna k
        pivo = encontrar_max_abs(A, k)
        trocar_linhas(A, k, pivo)
        ...
```

---

### Problema 2: Gram-Schmidt Clássico Instável

```
❌ ERRADO (clássico):
PARA j DE 0 ATÉ n-1:
    v = A[:, j]  // Copia coluna original de A
    PARA i DE 0 ATÉ j-1:
        v -= proj(A[:, j], Q[:, i])  // Projeta A, não v acumulado

✅ CORRETO (modificado):
PARA j DE 0 ATÉ n-1:
    v = A[:, j]
    PARA i DE 0 ATÉ j-1:
        v -= proj(v, Q[:, i])  // Projeta v já ortogonalizado
```

---

### Problema 3: Matriz Não Simétrica em Cholesky

```
❌ ERRADO:
L = cholesky(A)  // A não simétrica → resultados sem sentido

✅ CORRETO:
SE NÃO eh_simetrica(A):
    ERRO "Cholesky requer matriz simétrica"

SE NÃO eh_positiva_definida(A):
    ERRO "Cholesky requer autovalores > 0"
```

---

## 🧪 Testes Unitários

```
TESTE LU_sistema_simples:
    A = [[2, 1], [1, 1]]
    b = [3, 2]
    
    lu = decompor_LU_pivotamento(A, 2)
    x = resolver_com_LU(lu.L, lu.U, lu.perm, b, 2)
    
    // Verificar: Ax deve ser ≈ b
    Ax = multiplicar_matriz_vetor(A, x)
    ASSERT abs(Ax[0] - b[0]) < EPSILON
    ASSERT abs(Ax[1] - b[1]) < EPSILON

TESTE QR_ortonormalidade:
    A = [[1, 1], [1, 0], [0, 1]]  // 3×2
    qr = decompor_QR_gram_schmidt(A, 3, 2)
    Q = qr.Q
    
    // Verificar Q[:, 0] · Q[:, 1] = 0
    dot = 0
    PARA i DE 0 ATÉ 2:
        dot += Q[i][0] * Q[i][1]
    ASSERT abs(dot) < EPSILON
    
    // Verificar ||Q[:, 0]|| = 1
    norm = 0
    PARA i DE 0 ATÉ 2:
        norm += Q[i][0] * Q[i][0]
    ASSERT abs(raiz_quadrada(norm) - 1.0) < EPSILON

TESTE cholesky_reconstrucao:
    A = [[4, 2], [2, 3]]  // Simétrica positiva definida
    L = decompor_cholesky(A, 2)
    
    // Reconstruir: A_rec = L * Lᵀ
    A_rec = multiplicar(L, transpor(L))
    
    PARA i DE 0 ATÉ 1:
        PARA j DE 0 ATÉ 1:
            ASSERT abs(A_rec[i][j] - A[i][j]) < EPSILON

TESTE determinante:
    I = identidade(3)
    ASSERT abs(determinante_via_LU(I, 3) - 1.0) < EPSILON
    
    A_singular = [[1, 2], [2, 4]]  // det=0
    ASSERT abs(determinante_via_LU(A_singular, 2)) < EPSILON
```

---

## 📚 Exercícios de Implementação

1. **Inversa via LU:** Resolver `AX = I` coluna por coluna
2. **SVD simplificado:** Calcular valores singulares (autovalores de `AᵀA`)
3. **Iteração de Gauss-Seidel:** Método iterativo para sistemas grandes
4. **Número de condição:** `cond(A) = ||A|| ||A⁻¹||` (estabilidade)

---

## 🎯 Próximos Passos

1. ✅ **Prática avançada:** `k4-pratica/p3-avancados.md` (PCA, compressão SVD)
2. ✅ **Projeto:** Implementar solver completo de sistemas lineares
3. ✅ **Otimização:** BLAS/LAPACK para produção (100x mais rápido)

---

## 💡 Aplicações Reais

- **LU:** Simulação física (resolver muitos sistemas com mesmo A)
- **QR:** Mínimos quadrados (calibração, regressão)
- **Cholesky:** Otimização convexa, simulação Monte Carlo
- **SVD:** Compressão (imagens, dados), PCA, recomendação

---

<details>
<summary><strong>💻 Implementação Completa em C</strong></summary>

```c
#include <stdio.h>
#include <math.h>
#include <string.h>
#include <stdlib.h>

#define EPSILON 1e-10
#define MAX_SIZE 10

// Decomposição LU com pivotamento parcial
int lu_decomposition(double A[][MAX_SIZE], double L[][MAX_SIZE], 
                     double U[][MAX_SIZE], int perm[], int n) {
    // Copiar A para U
    for (int i = 0; i < n; i++) {
        perm[i] = i;
        for (int j = 0; j < n; j++) {
            U[i][j] = A[i][j];
            L[i][j] = (i == j) ? 1.0 : 0.0;
        }
    }
    
    for (int k = 0; k < n - 1; k++) {
        // Pivotamento: encontrar máximo
        int pivot_row = k;
        double max_val = fabs(U[k][k]);
        
        for (int i = k + 1; i < n; i++) {
            if (fabs(U[i][k]) > max_val) {
                max_val = fabs(U[i][k]);
                pivot_row = i;
            }
        }
        
        if (max_val < EPSILON) {
            return 0;  // Singular
        }
        
        // Trocar linhas
        if (pivot_row != k) {
            for (int j = 0; j < n; j++) {
                double tmp = U[k][j];
                U[k][j] = U[pivot_row][j];
                U[pivot_row][j] = tmp;
            }
            
            for (int j = 0; j < k; j++) {
                double tmp = L[k][j];
                L[k][j] = L[pivot_row][j];
                L[pivot_row][j] = tmp;
            }
            
            int tmp_perm = perm[k];
            perm[k] = perm[pivot_row];
            perm[pivot_row] = tmp_perm;
        }
        
        // Eliminação
        for (int i = k + 1; i < n; i++) {
            L[i][k] = U[i][k] / U[k][k];
            for (int j = k; j < n; j++) {
                U[i][j] -= L[i][k] * U[k][j];
            }
        }
    }
    
    return 1;  // Sucesso
}

// Resolver sistema com LU
void solve_lu(double L[][MAX_SIZE], double U[][MAX_SIZE], int perm[], 
              double b[], double x[], int n) {
    double y[MAX_SIZE];
    
    // Permutar b
    for (int i = 0; i < n; i++) {
        y[i] = b[perm[i]];
    }
    
    // Forward substitution: Ly = b'
    for (int i = 0; i < n; i++) {
        double sum = 0;
        for (int j = 0; j < i; j++) {
            sum += L[i][j] * y[j];
        }
        y[i] = y[i] - sum;
    }
    
    // Backward substitution: Ux = y
    for (int i = n - 1; i >= 0; i--) {
        double sum = 0;
        for (int j = i + 1; j < n; j++) {
            sum += U[i][j] * x[j];
        }
        x[i] = (y[i] - sum) / U[i][i];
    }
}

// Decomposição de Cholesky
int cholesky(double A[][MAX_SIZE], double L[][MAX_SIZE], int n) {
    memset(L, 0, sizeof(double) * MAX_SIZE * MAX_SIZE);
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j <= i; j++) {
            double sum = 0;
            
            if (j == i) {  // Diagonal
                for (int k = 0; k < j; k++) {
                    sum += L[j][k] * L[j][k];
                }
                
                double val = A[j][j] - sum;
                if (val <= EPSILON) {
                    return 0;  // Não positiva definida
                }
                L[j][j] = sqrt(val);
            } else {
                for (int k = 0; k < j; k++) {
                    sum += L[i][k] * L[j][k];
                }
                L[i][j] = (A[i][j] - sum) / L[j][j];
            }
        }
    }
    
    return 1;
}

void print_matrix(double M[][MAX_SIZE], int n) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            printf("%7.3f ", M[i][j]);
        }
        printf("\n");
    }
}

int main() {
    // Teste LU
    double A[MAX_SIZE][MAX_SIZE] = {
        {2, 1, 1},
        {4, 3, 3},
        {8, 7, 9}
    };
    double L[MAX_SIZE][MAX_SIZE], U[MAX_SIZE][MAX_SIZE];
    int perm[MAX_SIZE];
    
    printf("=== Decomposição LU ===\n");
    if (lu_decomposition(A, L, U, perm, 3)) {
        printf("L:\n");
        print_matrix(L, 3);
        printf("\nU:\n");
        print_matrix(U, 3);
        
        // Resolver sistema
        double b[] = {1, 2, 3};
        double x[MAX_SIZE];
        solve_lu(L, U, perm, b, x, 3);
        
        printf("\nSolução de Ax=b:\n");
        for (int i = 0; i < 3; i++) {
            printf("x[%d] = %.4f\n", i, x[i]);
        }
    }
    
    // Teste Cholesky
    double B[MAX_SIZE][MAX_SIZE] = {
        {4, 2, 0},
        {2, 3, 1},
        {0, 1, 2}
    };
    double L_chol[MAX_SIZE][MAX_SIZE];
    
    printf("\n=== Decomposição de Cholesky ===\n");
    if (cholesky(B, L_chol, 3)) {
        printf("L (Cholesky):\n");
        print_matrix(L_chol, 3);
    } else {
        printf("Matriz não é positiva definida\n");
    }
    
    return 0;
}
```

**Compilar:** `gcc -o decomposicoes i4-decomposicoes.c -lm`

</details>

---

**XP Disponível:** 90 XP  
**Tempo estimado:** 3h30-5h  
**Dificuldade:** ⭐⭐⭐⭐⭐
