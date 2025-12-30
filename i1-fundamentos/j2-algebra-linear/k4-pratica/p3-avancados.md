# Problemas Avançados de Programação

## 🎯 Meta

Integrar álgebra linear em projetos práticos: compressão SVD, PCA para redução de dimensionalidade, e solvers eficientes para sistemas grandes.

---

## ⏱️ Tempo Estimado

- **Problema 1 (SVD Compressão):** 1h-1h30
- **Problema 2 (PCA):** 1h15-1h45
- **Problema 3 (Solver Grandes Sistemas):** 1h30-2h
- **Total:** ~4h-5h15

---

## 📋 Pré-requisitos

- **Teoria:** `k1-teoria/t3-autovalores.md`, `t4-decomposicoes.md`
- **Implementação:** `k3-implementacao/codigo/i4-decomposicoes.md`
- **Exercícios:** `k2-exercicios/e3-autovalores-exercicios.md`, `e4-decomposicoes-exercicios.md` (completos)

---

## 🎚️ Dificuldade

⭐⭐⭐⭐⭐ Muito Avançado

---

## 💪 Sistema de XP

- **Problema 1:** 120 XP
- **Problema 2:** 130 XP
- **Problema 3:** 150 XP

**XP Total Disponível:** 400 XP

---

## 📊 Rastreamento de Progresso

- [ ] Problema 1: Compressor de Imagens SVD (0/1) - 120 XP
- [ ] Problema 2: PCA para Visualização de Dados (0/1) - 130 XP
- [ ] Problema 3: Solver Iterativo para Sistemas Grandes (0/1) - 150 XP

**XP Conquistado:** ___ / 400 XP

---

## Problema 1: Compressor de Imagens com SVD

### 🎯 Objetivo

Implementar compressor de imagens em escala de cinza usando Decomposição em Valores Singulares (SVD), permitindo controlar taxa de compressão vs qualidade.

### 📐 Contexto Teórico

SVD decompõe matriz `A (m×n)` em:
```
A = U Σ Vᵀ
```

Onde:
- `U (m×m)`: autovetores de `AAᵀ` (linhas)
- `Σ (m×n)`: valores singulares na diagonal (σ₁ ≥ σ₂ ≥ ... ≥ 0)
- `Vᵀ (n×n)`: autovetores de `AᵀA` (colunas)

**Compressão:** Manter apenas k maiores valores singulares:
```
A_aprox = U_k Σ_k V_kᵀ
```

Erro: `||A - A_aprox||_F = √(σ²_(k+1) + σ²_(k+2) + ... + σ²_r)`

### 🛠️ Especificação

**Entrada:**
- Imagem grayscale (matriz de pixels 0-255)
- Parâmetro k (número de componentes a manter)

**Saída:**
- Imagem comprimida (reconstruída com rank k)
- Taxa de compressão
- Erro de reconstrução (RMSE - Root Mean Square Error)

**Funcionalidades:**
1. Ler imagem PGM (formato simples ASCII)
2. Calcular SVD (pode usar biblioteca ou implementar simplificado)
3. Comprimir mantendo k componentes
4. Reconstruir imagem
5. Calcular métricas (taxa compressão, RMSE)
6. Salvar resultado

### 📝 Pseudocódigo

```
FUNÇÃO comprimir_svd(imagem, k):
    """
    Comprime imagem usando SVD de rank k
    """
    m, n = dimensoes(imagem)
    
    // Calcular SVD: A = UΣVᵀ
    U, sigma, Vt = calcular_svd(imagem)
    
    // Truncar para rank k
    U_k = U[:, 0:k]  // Primeiras k colunas
    sigma_k = sigma[0:k]  // Primeiros k valores
    Vt_k = Vt[0:k, :]  // Primeiras k linhas
    
    // Reconstruir: A_aprox = U_k * diag(sigma_k) * Vt_k
    imagem_comprimida = multiplicar(U_k, multiplicar(diag(sigma_k), Vt_k))
    
    // Calcular métricas
    original_bytes = m * n * 8  // 8 bytes por double
    comprimido_bytes = k * (m + n + 1) * 8  // U_k + sigma_k + Vt_k
    taxa_compressao = original_bytes / comprimido_bytes
    
    rmse = raiz_quadrada(media((imagem - imagem_comprimida)²))
    
    RETORNAR {imagem_comprimida, taxa_compressao, rmse}

FUNÇÃO calcular_svd_simplificado(A):
    """
    SVD via autovalores (apenas valores singulares e vetores)
    """
    // Calcular AᵀA
    AtA = multiplicar(transpor(A), A)
    
    // Autovalores e autovetores de AᵀA
    lambdas, V = calcular_autovalores_autovetores(AtA)
    
    // Valores singulares: σᵢ = √λᵢ
    sigma = raiz_quadrada(lambdas)
    
    // Calcular U: uᵢ = (1/σᵢ) * A * vᵢ
    m, n = dimensoes(A)
    U = criar_matriz_zero(m, n)
    
    PARA i DE 0 ATÉ n-1:
        SE sigma[i] > EPSILON:
            v_i = V[:, i]
            Av = multiplicar_matriz_vetor(A, v_i)
            U[:, i] = escalar_multiplicar(Av, 1.0 / sigma[i])
    
    Vt = transpor(V)
    
    RETORNAR {U, sigma, Vt}
```

### 🧪 Testes

```
Imagem de teste: Matriz 8×8 com gradiente simples

A = [0  32  64  96  128 160 192 224]
    [0  32  64  96  128 160 192 224]
    ...

Comprimir com k=2 (de rank 8)

Esperado:
- Taxa compressão > 2x
- RMSE baixo (imagem simples)
- Valores singulares decaem rapidamente
```

### 💡 Insights

- Imagens naturais têm energia concentrada nos primeiros componentes (baixa frequência)
- k=10-50 geralmente suficiente para 512×512
- Trade-off compressão vs qualidade controlado por k

### 🎯 Desafios Extras (+30 XP cada)

1. **Compressão por blocos:** Dividir imagem em blocos 8×8 (como JPEG)
2. **Gráfico de energia:** Plotar energia cumulativa vs k
3. **Limiar automático:** Escolher k para manter 95% da energia

---

## Problema 2: PCA para Visualização de Dados

### 🎯 Objetivo

Implementar Principal Component Analysis (PCA) para reduzir dataset de alta dimensionalidade para 2D/3D, permitindo visualização.

### 📐 Contexto Teórico

PCA encontra direções de máxima variância nos dados:

1. **Centralizar:** `X_centered = X - mean(X)`
2. **Covariância:** `C = (1/n) * X_centeredᵀ * X_centered`
3. **Autovalores:** Decomposição de C
4. **Projetar:** `X_pca = X_centered * V_k` (k primeiros autovetores)

**Variância explicada:** `λᵢ / Σλⱼ`

### 🛠️ Especificação

**Entrada:**
- Dataset numérico (matriz n×d: n amostras, d features)
- Número de componentes k (geralmente 2 ou 3)
- Labels das amostras (opcional, para colorir gráfico)

**Saída:**
- Dados projetados em k dimensões
- Variância explicada por cada componente
- Componentes principais (direções)

**Funcionalidades:**
1. Carregar dataset (CSV ou formato simples)
2. Centralizar dados
3. Calcular matriz de covariância
4. Encontrar autovalores/autovetores
5. Projetar em k dimensões
6. Salvar resultado (pontos 2D/3D)

### 📝 Pseudocódigo

```
FUNÇÃO pca(dataset, k):
    """
    Reduz dataset de d dimensões para k dimensões
    
    dataset: matriz n×d (n amostras, d features)
    k: número de componentes principais
    """
    n, d = dimensoes(dataset)
    
    // Passo 1: Centralizar dados (média = 0)
    medias = calcular_media_colunas(dataset)
    dataset_centrado = criar_matriz_zero(n, d)
    
    PARA i DE 0 ATÉ n-1:
        PARA j DE 0 ATÉ d-1:
            dataset_centrado[i][j] = dataset[i][j] - medias[j]
    
    // Passo 2: Matriz de covariância C = (1/n) * X^T * X
    Xt = transpor(dataset_centrado)
    C = multiplicar(Xt, dataset_centrado)
    
    PARA i DE 0 ATÉ d-1:
        PARA j DE 0 ATÉ d-1:
            C[i][j] /= n
    
    // Passo 3: Autovalores e autovetores de C
    autovalores, autovetores = calcular_eigen(C)
    
    // Passo 4: Ordenar por autovalor decrescente
    indices_ordenados = ordenar_decrescente(autovalores)
    
    autovalores_sorted = reordenar(autovalores, indices_ordenados)
    autovetores_sorted = reordenar_colunas(autovetores, indices_ordenados)
    
    // Passo 5: Selecionar k primeiros autovetores
    V_k = autovetores_sorted[:, 0:k]
    
    // Passo 6: Projetar dados
    dataset_pca = multiplicar(dataset_centrado, V_k)
    
    // Calcular variância explicada
    variancia_total = somar(autovalores_sorted)
    variancia_explicada = criar_array(k)
    
    PARA i DE 0 ATÉ k-1:
        variancia_explicada[i] = autovalores_sorted[i] / variancia_total
    
    RETORNAR {
        dataset_pca,
        variancia_explicada,
        componentes_principais: V_k,
        medias: medias
    }

FUNÇÃO projetar_novo_ponto(ponto, medias, componentes_principais):
    """Projeta novo ponto usando PCA já calculado"""
    ponto_centrado = subtrair(ponto, medias)
    ponto_pca = multiplicar_vetor(transpor(componentes_principais), ponto_centrado)
    RETORNAR ponto_pca
```

### 🧪 Testes

```
Dataset de teste: Iris (4 features → 2D)

Dados:
- 150 amostras
- 4 features (sepal length, sepal width, petal length, petal width)
- 3 classes (setosa, versicolor, virginica)

Esperado:
- PC1 explica ~73% da variância
- PC2 explica ~23%
- Total 2 componentes: ~96%
- Classes separáveis no espaço 2D
```

### 💡 Insights

- PCA assume dados Gaussianos e relações lineares
- Normalizar features antes (StandardScaler) se escalas diferentes
- Whitening: `X_white = X_pca / √λ` (mesma variância em todas direções)

### 🎯 Desafios Extras (+30 XP cada)

1. **Scree plot:** Gráfico autovalores vs componente (cotovelo indica k ideal)
2. **Biplot:** Visualizar dados E vetores de features simultaneamente
3. **Kernel PCA:** Versão não-linear usando kernel trick

---

## Problema 3: Solver Iterativo para Sistemas Grandes

### 🎯 Objetivo

Implementar método iterativo de Gauss-Seidel para resolver sistemas lineares grandes e esparsos `Ax = b` de forma eficiente.

### 📐 Contexto Teórico

**Métodos diretos (LU, Cholesky):** O(n³), inviável para n > 10⁴

**Métodos iterativos:**
- Começar com chute inicial x⁰
- Refinar iterativamente: x^(k+1) = f(x^k)
- Convergir para solução exata

**Gauss-Seidel:**
```
Para i = 1 até n:
    x_i^(k+1) = (b_i - Σ(j<i) a_ij*x_j^(k+1) - Σ(j>i) a_ij*x_j^(k)) / a_ii
```

Usa valores **já atualizados** (mais rápido que Jacobi).

**Convergência:** Garantida se A é estritamente diagonalmente dominante:
```
|a_ii| > Σ(j≠i) |a_ij|  para todo i
```

### 🛠️ Especificação

**Entrada:**
- Matriz A (pode ser esparsa, formato COO/CSR)
- Vetor b
- Chute inicial x⁰ (ou vetor zero)
- Tolerância (ex: 1e-6)
- Máximo de iterações

**Saída:**
- Solução aproximada x
- Número de iterações
- Resíduo final: ||Ax - b||

**Funcionalidades:**
1. Verificar diagonal dominância
2. Iterar Gauss-Seidel até convergência
3. Calcular resíduo a cada iteração
4. Retornar histórico de convergência

### 📝 Pseudocódigo

```
FUNÇÃO gauss_seidel(A, b, x_inicial, tolerancia, max_iter):
    """
    Resolve Ax = b iterativamente
    
    Convergência: ||x^(k+1) - x^k|| < tolerancia
    """
    n = tamanho(b)
    x = copiar(x_inicial)
    x_anterior = copiar(x)
    historico_residuo = criar_array(max_iter)
    
    PARA k DE 0 ATÉ max_iter-1:
        // Atualizar cada componente
        PARA i DE 0 ATÉ n-1:
            soma = 0
            
            // Soma com valores JÁ ATUALIZADOS (j < i)
            PARA j DE 0 ATÉ i-1:
                soma += A[i][j] * x[j]
            
            // Soma com valores ANTIGOS (j > i)
            PARA j DE i+1 ATÉ n-1:
                soma += A[i][j] * x_anterior[j]
            
            x[i] = (b[i] - soma) / A[i][i]
        
        // Calcular resíduo: r = Ax - b
        residuo = calcular_residuo(A, x, b, n)
        historico_residuo[k] = norma(residuo)
        
        // Verificar convergência
        erro = norma_diferenca(x, x_anterior)
        
        SE erro < tolerancia:
            RETORNAR {
                solucao: x,
                iteracoes: k+1,
                residuo_final: historico_residuo[k],
                historico: historico_residuo[0:k+1]
            }
        
        // Preparar próxima iteração
        x_anterior = copiar(x)
    
    RETORNAR {
        solucao: x,
        iteracoes: max_iter,
        residuo_final: historico_residuo[max_iter-1],
        historico: historico_residuo,
        convergiu: FALSO
    }

FUNÇÃO verificar_diagonal_dominante(A, n):
    """
    Verifica se |a_ii| > Σ|a_ij| (j≠i)
    """
    PARA i DE 0 ATÉ n-1:
        soma_fora_diagonal = 0
        
        PARA j DE 0 ATÉ n-1:
            SE j != i:
                soma_fora_diagonal += abs(A[i][j])
        
        SE abs(A[i][i]) <= soma_fora_diagonal:
            RETORNAR FALSO
    
    RETORNAR VERDADEIRO

FUNÇÃO calcular_residuo(A, x, b, n):
    """Resíduo: r = Ax - b"""
    Ax = multiplicar_matriz_vetor(A, x)
    residuo = criar_vetor(n)
    
    PARA i DE 0 ATÉ n-1:
        residuo[i] = Ax[i] - b[i]
    
    RETORNAR residuo
```

### 🧪 Testes

```
Sistema diagonal dominante:
A = [4  1  0]    b = [5]
    [1  4  1]        [6]
    [0  1  4]        [5]

Solução exata: x = [1, 1, 1]

Esperado:
- Convergência em ~10-15 iterações (tol=1e-6)
- Resíduo final < 1e-6
- Verificação diagonal dominante: TRUE

Sistema NÃO diagonal dominante:
A = [1  2  0]
    [2  1  1]
    [0  1  1]

Esperado:
- Pode não convergir ou convergir lentamente
- Aviso: "Matriz não é diagonalmente dominante"
```

### 💡 Insights

- **SOR (Successive Over-Relaxation):** Acelera Gauss-Seidel com parâmetro ω
- **Pré-condicionamento:** Transformar sistema para melhor convergência
- **Matrizes esparsas:** Guardar apenas elementos não-zero (economia massiva)

### 🎯 Desafios Extras (+40 XP cada)

1. **Suporte a matriz esparsa:** Formato CSR (Compressed Sparse Row)
2. **SOR com ω ótimo:** Encontrar parâmetro de relaxação ideal
3. **Gradiente Conjugado:** Método mais avançado para matrizes simétricas positivas definidas
4. **Comparação de performance:** Gauss-Seidel vs LU para n = 100, 1000, 10000

---

## 🎓 Entregáveis

Para cada problema:
1. ✅ Código-fonte completo (C/C++/Python)
2. ✅ Arquivo README com instruções de compilação/execução
3. ✅ Testes com dados reais ou sintéticos
4. ✅ Relatório breve (1-2 páginas):
   - Resultados obtidos
   - Gráficos (se aplicável)
   - Análise de performance
   - Desafios encontrados

---

## 🎯 Próximos Passos

Após completar estes problemas:
1. ✅ **Projeto final:** `k5-projeto/` - Motor de transformações 2D
2. ✅ **Aplicações avançadas:** Deep learning (backprop usa muito AL), física computacional
3. 🎉 **Parabéns:** Você dominou Álgebra Linear aplicada!

---

**Total XP disponível:** 400 XP (+ 210 XP extras)  
**Tempo total estimado:** 4h-5h15 (+ extras)  
**Dificuldade:** ⭐⭐⭐⭐⭐ (Muito Avançado)
