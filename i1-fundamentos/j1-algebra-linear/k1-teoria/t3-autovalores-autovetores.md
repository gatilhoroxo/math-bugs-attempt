# Autovalores e Autovetores

## 🎯 Meta de Aprendizado

Ao completar este tópico, você será capaz de:
- Entender **autovetores** como direções especiais que não são "torcidas"
- Calcular autovalores e autovetores de matrizes pequenas
- Usar diagonalização para simplificar transformações
- Aplicar em problemas práticos (PCA, sistemas dinâmicos, PageRank)

---

## ⏱️ Tempo Estimado

- **Leitura ativa:** 55-70 min
- **Exercícios relacionados:** 45-60 min (`k2-exercicios/e3-autovalores-exercicios.md`)
- **Implementação:** 50-70 min (`k3-implementacao/codigo/i3-autovetores.md`)
- **Total:** ~2h30-3h20

---

## 📋 Pré-requisitos

- [x] **t1-vetores-espacos.md** (independência linear, bases)
- [x] **t2-transformacoes-lineares.md** (ESSENCIAL - transformações, determinante)
- [ ] Polinômios (raízes de equações)
- [ ] Números complexos (para alguns casos)

---

## 🗺️ Mapa Mental

```
AUTOVALORES E AUTOVETORES
├── 1. Conceito Fundamental
│   ├── Definição: Av = λv
│   ├── Interpretação geométrica
│   └── Por que importam
│
├── 2. Cálculo
│   ├── Equação característica: det(A - λI) = 0
│   ├── Polinômio característico
│   ├── Encontrar autovalores (raízes)
│   └── Encontrar autovetores (núcleo)
│
├── 3. Propriedades
│   ├── Autovalores de matrizes especiais
│   ├── Multiplicidade algébrica vs geométrica
│   └── Autovalores complexos
│
├── 4. Diagonalização
│   ├── Quando é possível (n autovetores LI)
│   ├── A = PDP⁻¹
│   ├── Potências de matrizes (A^k)
│   └── Mudança de base
│
└── 5. Aplicações
    ├── PCA (Machine Learning)
    ├── Sistemas dinâmicos (estabilidade)
    ├── PageRank (Google)
    ├── Vibrações e oscilações
    └── Grafos (centralidade)
```

---

## 📖 Conteúdo

### 1. O que são Autovalores e Autovetores?

#### Definição

Dado matriz `A`, um **autovetor** `v` satisfaz:

```
A * v = λ * v
```

- `v`: **autovetor** (eigenvector) - direção especial
- `λ` (lambda): **autovalor** (eigenvalue) - fator de escala

**Restrições:**
- `v ≠ 0` (vetor zero não conta)
- `λ` pode ser qualquer número (real ou complexo)

> 💡 **Intuição:** Autovetor é uma direção que a transformação **não torce**, só estica/comprime/inverte.

#### Interpretação Geométrica

**Autovetor:** Direção que se mantém após transformação
- Só muda de comprimento (se `|λ| ≠ 1`)
- Pode inverter sentido (se `λ < 0`)
- Pode sumir (se `λ = 0`)

**Autovalor:** Quanto estica/comprime nessa direção
- `λ > 1`: estica
- `0 < λ < 1`: comprime
- `λ = 1`: não muda (invariante!)
- `λ = 0`: colapsa para origem
- `λ < 0`: inverte sentido

**Exemplo visual (matriz 2×2):**
```
A = [3  0]
    [0  2]

Autovetores: e₁ = (1,0), e₂ = (0,1)
Autovalores: λ₁ = 3, λ₂ = 2

A*(1,0) = (3,0) = 3*(1,0)  ✓
A*(0,1) = (0,2) = 2*(0,1)  ✓
```

> 🔗 **Conexão:** Matrizes diagonais têm eixos coordenados como autovetores!

---

### 2. Calculando Autovalores

#### Equação Característica

Reorganizando `Av = λv`:

```
Av = λv
Av - λv = 0
(A - λI)v = 0
```

Para ter solução não-trivial (`v ≠ 0`), precisa:

```
det(A - λI) = 0  ← Equação característica
```

#### Polinômio Característico

`det(A - λI)` é um polinômio em `λ`:

**Para matriz 2×2:**
```
A = [a  b]
    [c  d]

det(A - λI) = det([a-λ   b  ])
                  [ c   d-λ])
            = (a-λ)(d-λ) - bc
            = λ² - (a+d)λ + (ad-bc)
            = λ² - tr(A)λ + det(A)
```

**Propriedades:**
- Grau = dimensão da matriz (n×n → grau n)
- Tem **no máximo n raízes** (podem repetir)
- Coeficientes relacionados com traço e determinante

> 💡 **Intuição:** Soma de autovalores = traço (tr). Produto de autovalores = determinante.

#### Exemplo Completo (2×2)

```
A = [3  1]
    [0  2]

Equação característica:
det([3-λ   1  ]) = 0
   [ 0   2-λ])

(3-λ)(2-λ) - 0*1 = 0
λ² - 5λ + 6 = 0
(λ - 2)(λ - 3) = 0

Autovalores: λ₁ = 2, λ₂ = 3
```

#### Exemplo 3×3

```
A = [2  1  0]
    [1  2  0]
    [0  0  3]

det(A - λI) = det([2-λ   1    0  ])
                  [ 1   2-λ   0  ])
                  [ 0    0   3-λ])

= (3-λ) * det([2-λ   1  ])
              [ 1   2-λ])

= (3-λ)[(2-λ)² - 1]
= (3-λ)(λ² - 4λ + 3)
= (3-λ)(λ-1)(λ-3)

Autovalores: λ₁ = 1, λ₂ = 3 (multiplicidade 2)
```

---

### 3. Calculando Autovetores

Para cada autovalor `λ`, resolve:

```
(A - λI)v = 0
```

Isso é um **sistema homogêneo** - encontra núcleo (null space).

#### Exemplo (continuando anterior)

```
A = [3  1]      λ₁ = 2
    [0  2]

(A - 2I)v = 0
[1  1] [v₁] = [0]
[0  0] [v₂]   [0]

v₁ + v₂ = 0  →  v₂ = -v₁

Autovetor: v₁ = k[1]  (k ≠ 0 qualquer)
                [-1]

Normalizando: v₁ = [1/√2 ]
                   [-1/√2]
```

```
Para λ₂ = 3:

(A - 3I)v = 0
[0  1] [v₁] = [0]
[0 -1] [v₂]   [0]

v₂ = 0

Autovetor: v₂ = [1]
                [0]
```

**Verificação:**
```
A*v₁ = [3 1] [ 1] = [ 2] = 2*v₁  ✓
       [0 2] [-1]   [-2]

A*v₂ = [3 1] [1] = [3] = 3*v₂  ✓
       [0 2] [0]   [0]
```

> ⚠️ **Armadilha:** Autovetores têm **escala arbitrária**. `v` e `k*v` (k≠0) são o mesmo autovetor!

---

### 4. Propriedades Importantes

#### Autovalores de Matrizes Especiais

**Diagonal:**
```
D = [d₁  0  ...  0 ]
    [ 0  d₂ ...  0 ]
    [... ... ... ...]
    [ 0  0  ... dₙ]

Autovalores = elementos da diagonal (d₁, d₂, ..., dₙ)
```

**Triangular:**
Autovalores = elementos da diagonal (igual diagonal)

**Simétrica (A = Aᵀ):**
- Autovalores **sempre reais**
- Autovetores de autovalores diferentes são **ortogonais**

**Ortogonal (QᵀQ = I):**
- Autovalores têm `|λ| = 1` (módulo 1)

**Positiva definida:**
- Todos autovalores `> 0`

> 🔗 **Conexão:** Matrizes simétricas são super importantes em ML (matrizes de covariância)!

#### Multiplicidade

**Multiplicidade algébrica:** Quantas vezes `λ` aparece como raiz do polinômio característico

**Multiplicidade geométrica:** Dimensão do autoespaço (núcleo de `A - λI`)

**Relação:**
```
1 ≤ multiplicidade geométrica ≤ multiplicidade algébrica
```

**Exemplo:**
```
A = [2  1]
    [0  2]

Polinômio: (λ-2)²  →  λ = 2 com multiplicidade 2

Autoespaço:
(A - 2I)v = [0 1] v = 0  →  dimensão 1
            [0 0]

Multiplicidade algébrica = 2
Multiplicidade geométrica = 1
→ Matriz NÃO diagonalizável!
```

---

### 5. Diagonalização

#### Definição

Matriz `A` é **diagonalizável** se existe:

```
A = PDP⁻¹
```

- `D`: matriz diagonal (autovalores na diagonal)
- `P`: matriz cujas colunas são autovetores

**Condição necessária e suficiente:**
- A tem `n` autovetores linearmente independentes

#### Como Diagonalizar

**Passo 1:** Encontrar autovalores `λ₁, λ₂, ..., λₙ`

**Passo 2:** Encontrar autovetores correspondentes `v₁, v₂, ..., vₙ`

**Passo 3:** Montar:
```
P = [v₁ | v₂ | ... | vₙ]

D = [λ₁  0  ...  0 ]
    [ 0  λ₂ ...  0 ]
    [... ... ... ...]
    [ 0  0  ... λₙ]
```

**Passo 4:** Verificar `A = PDP⁻¹` ou `AP = PD`

#### Exemplo Completo

```
A = [3  1]
    [0  2]

Autovalores: λ₁ = 2, λ₂ = 3
Autovetores: v₁ = [ 1], v₂ = [1]
                  [-1]       [0]

P = [ 1  1]      D = [2  0]
    [-1  0]          [0  3]

Verificação:
AP = [3 1] [ 1  1] = [ 2  3]
     [0 2] [-1  0]   [-2  0]

PD = [ 1  1] [2 0] = [ 2  3]
     [-1  0] [0 3]   [-2  0]  ✓
```

#### Aplicações da Diagonalização

**1. Potências de Matrizes:**
```
A^k = (PDP⁻¹)^k = PD^kP⁻¹

D^k = [λ₁^k  0   ...  0  ]
      [ 0   λ₂^k ...  0  ]
      [...  ...  ... ...]
      [ 0    0   ... λₙ^k]
```

Muito mais fácil de calcular!

**2. Exponencial de Matriz:**
```
e^A = Pe^DP⁻¹

e^D = [e^λ₁  0    ...  0   ]
      [ 0   e^λ₂  ...  0   ]
      [... ...    ... ...]
      [ 0    0    ... e^λₙ]
```

**3. Sistemas Dinâmicos:**
```
x(t+1) = Ax(t)
x(t) = A^t x(0) = PD^t P⁻¹ x(0)
```

> 💡 **Intuição:** Diagonalização = mudar para "sistema de coordenadas natural" onde transformação é só escala.

---

### 6. Aplicações Práticas

#### PCA (Principal Component Analysis)

**Problema:** Reduzir dimensionalidade mantendo informação

**Solução:**
1. Calcular matriz de covariância `C` dos dados
2. Encontrar autovetores de `C` (componentes principais)
3. Projetar dados nos autovetores com maiores autovalores

```python
# PCA simplificado
C = X.T @ X  # Covariância
eigenvalues, eigenvectors = eig(C)
# Ordena por autovalores decrescentes
PC = eigenvectors[:, top_k]
X_reduced = X @ PC
```

> 🔗 **Conexão:** Em ML, PCA é usado para visualização (3D→2D), compressão de features, remoção de ruído.

#### PageRank (Google)

**Problema:** Rankear páginas web

**Solução:** Autovetor da matriz de transição de links com autovalor 1

```
r = M * r
```

- `r`: vetor de importância das páginas
- `M`: matriz de links (estocástica)

#### Sistemas Dinâmicos

**Equação:** `x(t+1) = Ax(t)`

**Comportamento:**
- Se todos `|λ| < 1`: sistema converge para zero (estável)
- Se algum `|λ| > 1`: sistema diverge (instável)
- Se todos `|λ| ≤ 1`: limitado

**Exemplo (população):**
```
[jovens  ]     [0    2  ] [jovens  ]
[adultos]t+1 = [0.5  0.8] [adultos]t

λ₁ = 1.3, λ₂ = -0.5
→ População cresce (λ₁ > 1)
```

#### Vibrações e Modos Normais

Estruturas vibrando (pontes, prédios):
- Autovetores = modos de vibração
- Autovalores = frequências naturais

> ⚠️ **Armadilha:** Ressonância ocorre quando frequência externa = autovalor!

---

## ✅ Checklist de Compreensão

Você domina autovalores/autovetores se consegue:

- [ ] Explicar autovetor como "direção que não é torcida"
- [ ] Calcular autovalores de matriz 2×2 (equação característica)
- [ ] Calcular autovetores dado autovalor
- [ ] Verificar se `Av = λv`
- [ ] Saber quando matriz é diagonalizável
- [ ] Diagonalizar matriz 2×2
- [ ] Usar diagonalização para calcular `A^10`
- [ ] Explicar aplicação prática (PCA, PageRank, ou sistemas)
- [ ] Relacionar autovalores com traço e determinante

---

## 🎯 Próximos Passos

### 1. **Praticar Agora** (RECOMENDADO)
📝 Faça exercícios: `k2-exercicios/e3-autovalores-exercicios.md`
- Nível 1: Cálculo de autovalores/autovetores
- Nível 2: Diagonalização
- Nível 3: Aplicações (PCA, sistemas dinâmicos)

### 2. **Implementar**
💻 Veja código: `k3-implementacao/codigo/i3-autovetores.md`
- Método da potência (autovalor dominante)
- Algoritmo QR (todos autovalores)
- PCA do zero

### 3. **Aplicar**
🎮 Problemas práticos: `k4-pratica/p2-intermediarios.md`
- Problema 2.3: Implementar PCA
- Problema 2.4: Análise de estabilidade

### 4. **Avançar**
➡️ Próximo tópico: `t4-decomposicoes.md`
- SVD (generalização de autovetores)
- QR, Cholesky, LU
- Aplicações em compressão e ML

---

## 📚 Recursos Adicionais

### Vídeos
- 3Blue1Brown - Eigenvalues and Eigenvectors (Essence ch. 13-14)
- MIT OCW - Gilbert Strang Lecture 21

### Leitura
- Strang - Introduction to Linear Algebra (Cap. 6)
- Trefethen & Bau - Numerical Linear Algebra (Cap. 24-25)

### Ferramentas
- NumPy: `np.linalg.eig()`
- MATLAB/Octave: `eig(A)`
- Symbolab - Calculadora de autovalores

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
