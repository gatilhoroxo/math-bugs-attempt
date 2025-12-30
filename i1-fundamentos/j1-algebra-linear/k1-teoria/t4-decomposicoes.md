# Decomposições Matriciais

## 🎯 Meta de Aprendizado

Ao completar este tópico, você será capaz de:
- Entender decomposições como "fatorações" de matrizes
- Aplicar LU, QR, Cholesky para resolver sistemas eficientemente
- Usar SVD para compressão, recomendação e análise de dados
- Escolher decomposição apropriada para cada problema

---

## ⏱️ Tempo Estimado

- **Leitura ativa:** 60-75 min
- **Exercícios relacionados:** 50-65 min (`k2-exercicios/e4-decomposicoes-exercicios.md`)
- **Implementação:** 80-110 min (`k3-implementacao/codigo/i4-decomposicoes.md`)
- **Total:** ~3h10-4h10

---

## 📋 Pré-requisitos

- [x] **t1-vetores-espacos.md** (bases ortonormais)
- [x] **t2-transformacoes-lineares.md** (sistemas lineares, inversas)
- [x] **t3-autovalores-autovetores.md** (ESSENCIAL para SVD)
- [ ] Conforto com matrizes grandes

---

## 🗺️ Mapa Mental

```
DECOMPOSIÇÕES MATRICIAIS
├── 1. Por que Decompor?
│   ├── Revelar estrutura oculta
│   ├── Simplificar computações
│   ├── Estabilidade numérica
│   └── Compressão de dados
│
├── 2. LU (Lower-Upper)
│   ├── A = LU
│   ├── Resolver sistemas múltiplos (mesmo A, vários b)
│   ├── Complexidade O(n³)
│   └── Pivotamento para estabilidade
│
├── 3. QR (Ortogonal-Triangular)
│   ├── A = QR
│   ├── Q: ortogonal, R: triangular superior
│   ├── Gram-Schmidt
│   ├── Resolver sistemas (mais estável que LU)
│   └── Algoritmo QR para autovalores
│
├── 4. Cholesky
│   ├── A = LLᵀ (simétrica positiva definida)
│   ├── Metade do custo de LU
│   └── ML: inverter matrizes de covariância
│
└── 5. SVD (Singular Value Decomposition)
    ├── A = UΣVᵀ (QUALQUER matriz!)
    ├── U, V: ortogonais
    ├── Σ: diagonal (valores singulares)
    ├── Aplicações incríveis:
    │   ├── Compressão de imagens
    │   ├── Recomendação (Netflix)
    │   ├── PCA
    │   └── Pseudo-inversa
    └── Algoritmo mais caro, mas mais versátil
```

---

## 📖 Conteúdo

### 1. Por que Decomposições?

Assim como fatoramos `12 = 2² × 3`, podemos "fatorar" matrizes!

**Vantagens:**

1. **Revelar estrutura:** Separar informação importante de ruído
2. **Simplificar cálculos:** Resolver sistemas, inverter matrizes
3. **Estabilidade numérica:** Métodos mais robustos a erros de arredondamento
4. **Compressão:** Representar dados grandes com menos informação
5. **Aplicações específicas:** Cada decomposição tem seu "superpoder"

> 💡 **Intuição:** Decompor = quebrar problema complexo em pedaços simples e bem comportados.

**Exemplo conceitual:**
```
Resolver Ax = b diretamente: difícil
Decompor A = LU, resolver Ly = b e Ux = y: fácil (triangulares!)
```

---

### 2. Decomposição LU

#### Conceito

Fatorar matriz `A` em:

```
A = L * U
```

- `L`: **Lower** triangular (triangular inferior)
- `U`: **Upper** triangular (triangular superior)

**Exemplo:**
```
A = [2  1  1]
    [4  3  3]
    [8  7  9]

L = [1  0  0]      U = [2  1  1]
    [2  1  0]          [0  1  1]
    [4  3  1]          [0  0  2]

Verificação: L*U = A ✓
```

#### Para que Serve?

**Resolver sistemas `Ax = b` para múltiplos `b`:**

```
Passo 1: Fatorar A = LU (uma vez, O(n³))
Passo 2: Para cada b:
  2a. Resolver Ly = b    (substituição direta, O(n²))
  2b. Resolver Ux = y    (substituição reversa, O(n²))
```

**Quando usar:** Precisa resolver `Ax = b` para muitos valores diferentes de `b`.

**Exemplo numérico:**
```
A = [2  1]      b = [3]
    [4  3]          [7]

L = [1  0]      U = [2  1]
    [2  1]          [0  1]

Ly = b:
[1 0] [y₁] = [3]  →  y₁ = 3
[2 1] [y₂]   [7]      2*3 + y₂ = 7  →  y₂ = 1

Ux = y:
[2 1] [x₁] = [3]  →  2x₁ + x₂ = 3
[0 1] [x₂]   [1]      x₂ = 1  →  x₁ = 1

Solução: x = (1, 1)
```

#### Pivotamento

**Problema:** Divisão por zero ou números muito pequenos (instabilidade numérica)

**Solução:** **Pivotamento parcial** - trocar linhas para colocar maior elemento no pivô

```
A = PLU
```

- `P`: matriz de permutação (troca de linhas)

> ⚠️ **Armadilha:** LU sem pivotamento pode falhar mesmo quando A é invertível!

#### Complexidade

- Fatoração: **O(n³)**
- Cada solução: **O(n²)**

---

### 3. Decomposição QR

#### Conceito

Fatorar matriz `A` em:

```
A = Q * R
```

- `Q`: matriz **ortogonal** (QᵀQ = I)
- `R`: matriz triangular **superior** (Right/Upper)

**Propriedades de Q:**
- Colunas são ortonormais
- `Q⁻¹ = Qᵀ` (super barato inverter!)
- Preserva normas: `||Qx|| = ||x||`

#### Gram-Schmidt (método para construir QR)

**Ideia:** Ortonormalizar colunas de A

```
Dado: a₁, a₂, a₃ (colunas de A)

1. u₁ = a₁
   q₁ = u₁ / ||u₁||

2. u₂ = a₂ - (a₂·q₁)q₁       (remove componente paralela a q₁)
   q₂ = u₂ / ||u₂||

3. u₃ = a₃ - (a₃·q₁)q₁ - (a₃·q₂)q₂
   q₃ = u₃ / ||u₃||

Q = [q₁ | q₂ | q₃]
R = QᵀA  (triangular superior por construção)
```

**Exemplo:**
```
A = [1  1]
    [0  1]
    [0  0]

Gram-Schmidt:
a₁ = (1,0,0)  →  q₁ = (1,0,0)

a₂ = (1,1,0)
u₂ = a₂ - (a₂·q₁)q₁ = (1,1,0) - 1*(1,0,0) = (0,1,0)
q₂ = (0,1,0)

Q = [1  0]      R = [1  1]
    [0  1]          [0  1]
    [0  0]          [0  0]
```

> 🔗 **Conexão:** Gram-Schmidt é usado para criar bases ortonormais (vimos em t1)!

#### Para que Serve?

**1. Resolver sistemas (mais estável que LU):**
```
Ax = b
QRx = b
Rx = Qᵀb  (multiplicação barata!)
```

**2. Mínimos quadrados:**
```
Minimizar ||Ax - b||²
Solução: x = R⁻¹Qᵀb
```

**3. Calcular autovalores (Algoritmo QR iterativo):**
```
Repetir:
  A = QR
  A ← RQ
Converge para matriz triangular (autovalores na diagonal)
```

#### Complexidade

- Fatoração: **O(mn²)** (m ≥ n)
- Mais caro que LU, mas mais estável numericamente

---

### 4. Decomposição de Cholesky

#### Conceito

Para matrizes **simétricas positivas definidas** (ex: matrizes de covariância):

```
A = L * Lᵀ
```

- `L`: triangular inferior

**Condição:** A simétrica e todos autovalores `> 0`

**Vantagem:** Metade do custo de LU!

**Exemplo:**
```
A = [4  2]
    [2  3]

L = [2    0  ]
    [1  √2  ]

Verificação: L*Lᵀ = [2  0] [2  1  ] = [4  2] ✓
                     [1  √2] [0  √2]   [2  3]
```

#### Para que Serve?

**Machine Learning:**
- Inverter matrizes de covariância (regressão linear)
- Amostrar distribuições gaussianas multivariadas

**Exemplo (geração de dados correlacionados):**
```python
# Gerar amostras N(0, Σ)
L = cholesky(Σ)
z = random.normal(0, 1, n)  # N(0, I)
x = L @ z  # N(0, Σ)
```

> 🔗 **Conexão:** Em ML, quase toda matriz de covariância pode usar Cholesky!

#### Complexidade

- Fatoração: **O(n³/3)** (metade de LU)

> ⚠️ **Armadilha:** Se A não for positiva definida, Cholesky falha! Teste autovalores antes.

---

### 5. SVD (Singular Value Decomposition)

#### Conceito

**A decomposição mais poderosa!** Funciona para **QUALQUER** matriz (não precisa ser quadrada):

```
A = U * Σ * Vᵀ
```

- `U` (m×m): matriz ortogonal (autovetores de AAᵀ)
- `Σ` (m×n): diagonal (valores singulares σ₁ ≥ σ₂ ≥ ... ≥ 0)
- `V` (n×n): matriz ortogonal (autovetores de AᵀA)

**Forma reduzida (rank r):**
```
A ≈ U_r * Σ_r * V_rᵀ
```

Mantém r maiores valores singulares, descarta resto (compressão!)

> 💡 **Intuição:** SVD = "receita universal" para decompor QUALQUER transformação linear em rotações e escalas.

#### Interpretação Geométrica

SVD diz: "Toda matriz faz 3 coisas":

1. **Vᵀ**: Rotação em ℝⁿ
2. **Σ**: Escala eixos (σ₁, σ₂, ...)
3. **U**: Rotação em ℝᵐ

**Exemplo:**
```
Matriz 2×2 qualquer = Rotação₁ → Escala → Rotação₂
```

#### Relação com Autovalores

**Para matriz quadrada simétrica `A = Aᵀ`:**
- Valores singulares = |autovalores|
- U = V = autovetores

**Caso geral:**
- σᵢ² são autovalores de AᵀA (ou AAᵀ)
- SVD generaliza diagonalização para matrizes não-quadradas!

#### Como Calcular

**Passo 1:** Calcular AᵀA (n×n)

**Passo 2:** Encontrar autovalores/autovetores de AᵀA
```
AᵀA v_i = σᵢ² v_i
V = [v₁ | v₂ | ... | vₙ]
```

**Passo 3:** Valores singulares
```
σᵢ = √(autovalores de AᵀA)
```

**Passo 4:** Calcular U
```
u_i = (1/σᵢ) * A * v_i
U = [u₁ | u₂ | ... | uₘ]
```

**Exemplo:**
```
A = [3  0]
    [0  2]
    [0  0]

AᵀA = [3 0] [3  0] = [9  0]
      [0 2] [0  2]   [0  4]
      [0 0] [0  0]

Autovalores: λ₁ = 9, λ₂ = 4
Valores singulares: σ₁ = 3, σ₂ = 2

V = [1  0]      Σ = [3  0]
    [0  1]          [0  2]
                    [0  0]

u₁ = (1/3)*A*v₁ = (1,0,0)ᵀ
u₂ = (1/2)*A*v₂ = (0,1,0)ᵀ
u₃ = (0,0,1)ᵀ (ortogonal aos outros)

U = [1  0  0]
    [0  1  0]
    [0  0  1]
```

---

### 6. Aplicações de SVD

#### 1. Compressão de Imagens

**Ideia:** Manter apenas k maiores valores singulares

```
Imagem original: m×n (ex: 1000×1000 = 1M pixels)

SVD: A = UΣVᵀ
Aproximação: A ≈ U_k Σ_k V_kᵀ

Armazenamento:
  Original: mn
  SVD-k: k(m + n + 1)
  
Para k=50, m=n=1000:
  Original: 1.000.000
  SVD-50: 50(1000 + 1000 + 1) ≈ 100.050  (10× menor!)
```

**Exemplo (imagem em tons de cinza):**
```python
U, S, Vt = np.linalg.svd(img)
k = 50
img_compressed = U[:, :k] @ np.diag(S[:k]) @ Vt[:k, :]
```

#### 2. Sistemas de Recomendação

**Problema:** Matriz usuários × filmes (esparsa, muitos valores faltando)

```
Usuários \ Filmes   | A | B | C | D
--------------------|---|---|---|---
Alice               | 5 | ? | 3 | ?
Bob                 | 4 | 2 | ? | 5
Carol               | ? | 5 | 4 | ?
```

**Solução (SVD):**
```
M ≈ U_k Σ_k V_kᵀ

U_k: usuários em "espaço de preferências" (k dimensões)
V_k: filmes em "espaço de gêneros" (k dimensões)

Preenche valores faltantes com aproximação!
```

> 🔗 **Conexão:** Netflix Prize (2009) usou variantes de SVD!

#### 3. PCA (Principal Component Analysis)

**PCA usando SVD:**
```python
# Centralizar dados
X_centered = X - X.mean(axis=0)

# SVD
U, S, Vt = np.linalg.svd(X_centered)

# Componentes principais = colunas de V
PC = Vt.T[:, :k]

# Projetar
X_pca = X_centered @ PC
```

**Vantagem sobre autovalores:** SVD é mais estável numericamente que calcular autovalores de XᵀX.

#### 4. Pseudo-Inversa (A⁺)

**Para matrizes não-inversíveis ou não-quadradas:**

```
A⁺ = V * Σ⁺ * Uᵀ

Σ⁺: inverter valores singulares não-zero
    [σ₁  0   0 ]⁺   [1/σ₁   0    0]
    [ 0  σ₂  0 ]  = [  0   1/σ₂  0]
    [ 0   0  0 ]    [  0     0   0]
```

**Aplicação:** Resolver sistemas sobre/subdeterminados (mínimos quadrados)

```
Minimizar ||Ax - b||²
Solução: x = A⁺b
```

#### 5. Análise de Ruído

```
Valores singulares grandes: Informação
Valores singulares pequenos: Ruído

Filtrar ruído: manter apenas k maiores σᵢ
```

---

### 7. Comparação de Decomposições

| Decomposição | Condição | Custo | Quando Usar |
|--------------|----------|-------|-------------|
| **LU** | Quadrada (invertível) | O(n³) | Múltiplos sistemas (mesmo A) |
| **Cholesky** | Simétrica positiva def. | O(n³/3) | Covariâncias, ML |
| **QR** | Qualquer | O(mn²) | Mínimos quadrados, estabilidade |
| **SVD** | **Qualquer!** | O(mn²) | Compressão, recomendação, PCA, pseudo-inversa |

> 💡 **Regra geral:** Use decomposição mais específica possível (mais eficiente). SVD é "canivete suíço" (funciona sempre, mas mais caro).

---

### 8. Estabilidade Numérica

**Ranking de estabilidade (melhor → pior):**

1. **SVD** (mais estável, mas mais caro)
2. **QR**
3. **Cholesky** (para simétricas positivas)
4. **LU com pivotamento**
5. **LU sem pivotamento** (pode falhar!)
6. **Inversa direta** (NUNCA USE!)

> ⚠️ **Armadilha:** Nunca calcule `x = inv(A) @ b`! Sempre use `x = solve(A, b)` (usa LU internamente).

---

## ✅ Checklist de Compreensão

Você domina decomposições se consegue:

- [ ] Explicar por que decomposições são úteis (3 razões)
- [ ] Usar LU para resolver sistemas múltiplos
- [ ] Aplicar Gram-Schmidt para ortonormalizar vetores
- [ ] Saber quando usar Cholesky vs LU
- [ ] Entender SVD geometricamente (rotação-escala-rotação)
- [ ] Usar SVD para compressão de dados
- [ ] Explicar diferença entre valores singulares e autovalores
- [ ] Escolher decomposição apropriada para problema dado
- [ ] Saber que inversa direta é má ideia numericamente

---

## 🎯 Próximos Passos

### 1. **Praticar Agora** (RECOMENDADO)
📝 Faça exercícios: `k2-exercicios/e4-decomposicoes-exercicios.md`
- Nível 1: LU, QR, Cholesky manualmente (2×2, 3×3)
- Nível 2: SVD, compressão de imagens
- Nível 3: Aplicações práticas (recomendação, PCA)

### 2. **Implementar**
💻 Veja código: `k3-implementacao/codigo/i4-decomposicoes.md`
- Implementar Gram-Schmidt
- SVD usando bibliotecas
- Compressão de imagens
- Sistema de recomendação simples

### 3. **Aplicar**
🎮 Problemas práticos: `k4-pratica/p2-intermediarios.md`
- Problema 2.1: Resolver sistemas eficientemente
- Problema 2.2: Compressor de imagens SVD

### 4. **Projeto Final**
🚀 Integrar tudo: `k5-projeto/`
- Usar decomposições para otimizar transformações
- Implementar aproximações de baixo rank

### 5. **Continuar Aprendendo**
- Tópicos avançados: Eigenfaces, Kernel PCA, NMF
- Álgebra Linear Numérica (Trefethen & Bau)
- Aplicações em Deep Learning (tensores, backprop)

---

## 📚 Recursos Adicionais

### Vídeos
- 3Blue1Brown - Nonsquare matrices (Essence ch. 15)
- MIT OCW - Strang Lecture 26-27 (SVD)

### Leitura
- Strang - Introduction to Linear Algebra (Cap. 7)
- Trefethen & Bau - Numerical Linear Algebra (THE book)

### Ferramentas
- NumPy: `np.linalg.svd()`, `np.linalg.qr()`, `scipy.linalg.cholesky()`
- LAPACK (por baixo dos panos): Implementações ultra-otimizadas

### Aplicações Reais
- Scikit-learn: `TruncatedSVD`, `PCA`
- Surprise (recomendação): Usa SVD
- OpenCV: Compressão de imagens

---

## 💬 Notas de Aprendizado

<details>
<summary>📝 Dúvidas frequentes que tive</summary>

- 

</details>

<details>
<summary>💡 Insights que tive durante o estudo</summary>

- 

</details>

<details>
<summary>🔗 Conexões que fiz com outros tópicos</summary>

- 

</details>

---

**Tempo gasto neste tópico:** ___ minutos  
**Data de conclusão:** ___/___/___  
**Revisão necessária:** [ ] Sim [ ] Não

---

## 🎉 Parabéns!

Você completou todos os tópicos de **teoria** de Álgebra Linear!

**O que você domina agora:**
- ✅ Vetores e espaços vetoriais
- ✅ Transformações lineares e sistemas
- ✅ Autovalores e autovetores
- ✅ Decomposições matriciais

**Próximos desafios:**
1. Fazer TODOS os exercícios (`k2-exercicios/`)
2. Implementar tudo em código (`k3-implementacao/`)
3. Resolver problemas práticos (`k4-pratica/`)
4. Finalizar projeto completo (`k5-projeto/`)

**Continue assim! 🚀**
