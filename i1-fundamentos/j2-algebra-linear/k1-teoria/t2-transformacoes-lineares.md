# Transformações Lineares e Sistemas

## 🎯 Meta de Aprendizado

Ao completar este tópico, você será capaz de:
- Ver matrizes como **transformações geométricas** (não apenas tabelas)
- Construir matrizes para transformações específicas (rotação, escala, etc.)
- Compor transformações e entender ordem de aplicação
- Resolver sistemas lineares computacionalmente
- Escolher método apropriado para diferentes tipos de problemas

---

## ⏱️ Tempo Estimado

- **Leitura ativa:** 50-65 min
- **Exercícios relacionados:** 40-50 min (`k2-exercicios/e2-transformacoes-exercicios.md`)
- **Implementação:** 70-100 min (`k3-implementacao/codigo/i2-matrizes.md`)
- **Total:** ~2h40-3h35

---

## 📋 Pré-requisitos

- [x] **t1-vetores-espacos.md** (ESSENCIAL - precisa entender vetores bem!)
- [ ] Álgebra básica (resolver equações)
- [ ] Geometria (ângulos, seno, cosseno)
- [ ] Noção de funções

---

## 🗺️ Mapa Mental

```
TRANSFORMAÇÕES E SISTEMAS
├── 1. Matrizes como Transformações
│   ├── Definição de transformação linear
│   ├── Representação matricial
│   ├── Como construir matriz (colunas = base transformada)
│   └── Visualização geométrica
│
├── 2. Transformações Básicas 2D/3D
│   ├── Rotação
│   ├── Escala (uniforme/não-uniforme)
│   ├── Reflexão
│   ├── Cisalhamento (shear)
│   └── Projeção
│
├── 3. Propriedades de Matrizes
│   ├── Identidade (I)
│   ├── Transposta (Aᵀ)
│   ├── Inversa (A⁻¹) → desfaz transformação
│   └── Determinante (det) → escala área/volume
│
├── 4. Composição
│   ├── Multiplicação matriz-matriz
│   ├── Ordem importa! (A*B ≠ B*A)
│   └── Complexidade O(n³)
│
└── 5. Sistemas Lineares
    ├── Forma matricial (Ax = b)
    ├── Tipos de soluções (única/infinita/nenhuma)
    ├── Métodos diretos (Gaussiana, LU)
    └── Métodos iterativos (grandes sistemas)
```

---

## 📖 Conteúdo

### 1. Matrizes: Transformações, não Tabelas

**❌ Visão limitada:** "Matriz é uma tabela de números organizados"

**✅ Visão correta:** "Matriz é uma **função linear que transforma vetores**"

#### O que é uma Transformação Linear?

Uma função `T: ℝⁿ → ℝᵐ` é **linear** se satisfaz:

1. **Preserva adição:** `T(u + v) = T(u) + T(v)`
2. **Preserva escala:** `T(k*v) = k*T(v)`

**Consequência automática:** `T(0) = 0` (sempre mapeia origem para origem)

> 💡 **Intuição:** Transformações lineares mantêm linhas retas como linhas retas e a origem fixa. Pensa em rotações, escalas e espelhamentos - tudo linear!

#### Representação Matricial

**Teorema fundamental:** Toda transformação linear pode ser representada como:

```
y = A * x
```

- `x`: vetor de entrada (dimensão n)
- `A`: matriz da transformação (m × n)
- `y`: vetor de saída (dimensão m)

#### Como Construir a Matriz

**Regra de ouro:** As **colunas** de A são as imagens dos vetores da base canônica.

**Exemplo em ℝ²:**
```
Se T(e₁) = (2, 1) e T(e₂) = (0, 3), então:

A = [2  0]  ← primeira coluna = T(e₁)
    [1  3]  ← segunda coluna = T(e₂)
```

**Verificação:**
```
T((1, 0)) = [2 0] [1] = [2] ✓
             [1 3] [0]   [1]
```

> ⚠️ **Armadilha:** Não confunda **linhas** com **colunas**. As colunas carregam o significado geométrico!

---

### 2. Transformações Básicas em 2D

#### Identidade

```
I = [1  0]
    [0  1]

I * v = v  (não faz nada)
```

#### Rotação (θ radianos, anti-horário)

```
R(θ) = [cos(θ)  -sin(θ)]
       [sin(θ)   cos(θ)]
```

**Exemplo - Rotação de 90°:**
```
θ = π/2
R = [0  -1]
    [1   0]

(1,0) → (0,1)  ✓
(0,1) → (-1,0) ✓
```

> 🔗 **Conexão:** Em jogos 2D, toda rotação de sprite usa essa matriz!

#### Escala (sx em x, sy em y)

```
S = [sx  0 ]
    [0   sy]
```

**Escala uniforme (k):**
```
S = [k  0]
    [0  k]
```

**Exemplo:**
```
[2 0] [1]   [2]    Dobra em x
[0 3] [1] = [3]    Triplica em y
```

#### Reflexão

**Sobre eixo X:**
```
Fx = [1   0]
     [0  -1]
```

**Sobre eixo Y:**
```
Fy = [-1  0]
     [0   1]
```

**Sobre linha y = x:**
```
F45 = [0  1]
      [1  0]
```

> 💡 **Intuição:** Reflexão sobre y=x é como "trocar x e y" → transposta!

#### Cisalhamento (Shear)

**Horizontal:**
```
Hx = [1  k]
     [0  1]
```

**Exemplo com k=1:**
```
[1 1] [0]   [1]    Ponto (0,1) vai para (1,1)
[0 1] [1] = [1]    
```

**Vertical:**
```
Hy = [1  0]
     [k  1]
```

> 🔗 **Conexão:** Efeito "itálico" em texto usa cisalhamento horizontal!

#### Projeção

**Sobre eixo X:**
```
Px = [1  0]
     [0  0]
```

**Sobre eixo Y:**
```
Py = [0  0]
     [0  1]
```

> ⚠️ **Armadilha:** Projeções **destroem** informação (determinante = 0, não inversível)!

---

### 3. Transformações em 3D

#### Rotação em torno dos Eixos

**Rotação em X (pitch):**
```
Rx(θ) = [1    0        0    ]
        [0  cos(θ)  -sin(θ) ]
        [0  sin(θ)   cos(θ) ]
```

**Rotação em Y (yaw):**
```
Ry(θ) = [ cos(θ)  0  sin(θ) ]
        [   0     1    0    ]
        [-sin(θ)  0  cos(θ) ]
```

**Rotação em Z (roll):**
```
Rz(θ) = [cos(θ)  -sin(θ)  0]
        [sin(θ)   cos(θ)  0]
        [  0        0     1]
```

> 🔗 **Conexão:** Aviões e drones usam ângulos de Euler (pitch, yaw, roll)!

---

### 4. Propriedades de Matrizes

#### Transposta (Aᵀ)

**Operação:** Espelha pela diagonal principal (linhas ↔ colunas)

```
A = [1 2 3]      Aᵀ = [1 4]
    [4 5 6]           [2 5]
                      [3 6]
```

**Propriedades:**
- `(Aᵀ)ᵀ = A`
- `(AB)ᵀ = BᵀAᵀ` (ordem inverte!)
- `(A + B)ᵀ = Aᵀ + Bᵀ`

> 💡 **Intuição:** Transposta de rotação = rotação inversa (em matrizes ortogonais).

#### Determinante (det)

**O que mede:**
- Fator de escala de **área** (2D) ou **volume** (3D)
- Se transformação é invertível (det ≠ 0)

**Fórmula 2×2:**
```
det([a b]) = ad - bc
    [c d]
```

**Fórmula 3×3:**
```
det([a b c])
    [d e f] = a(ei-fh) - b(di-fg) + c(dh-eg)
    [g h i]
```

**Interpretações:**
- `det > 0`: preserva orientação
- `det < 0`: inverte orientação (espelhamento)
- `det = 0`: colapsa dimensão (não invertível)
- `|det| = 2`: dobra área/volume

**Exemplos:**
```
Identidade: det(I) = 1 (preserva área)
Escala 2×: det([2 0; 0 2]) = 4 (quadruplica área!)
Reflexão: det([-1 0; 0 1]) = -1 (inverte orientação)
```

> ⚠️ **Armadilha:** `det(2A) = 2ⁿ det(A)` para matriz n×n, não 2*det(A)!

**Propriedades:**
- `det(AB) = det(A) * det(B)`
- `det(Aᵀ) = det(A)`
- `det(A⁻¹) = 1 / det(A)`

#### Matriz Inversa (A⁻¹)

**Definição:** "Desfaz" a transformação de A

```
A * A⁻¹ = A⁻¹ * A = I
```

**Quando existe?**
- **Somente se** `det(A) ≠ 0`
- Transformação é bijetiva (1-para-1 e onto)
- Não perde informação

**Fórmula 2×2:**
```
A = [a b]
    [c d]

A⁻¹ = (1 / det(A)) * [ d  -b]
                      [-c   a]
```

**Exemplo:**
```
A = [2 1]        det = 6 - 2 = 4
    [2 3]

A⁻¹ = (1/4) * [ 3  -1] = [ 0.75  -0.25]
              [-2   2]   [-0.5    0.5 ]
```

**Verificação:**
```
A * A⁻¹ = [2 1] [ 0.75  -0.25]   [1 0]
          [2 3] [-0.5    0.5 ] = [0 1] ✓
```

**Aplicações:**
- Resolver sistemas: `x = A⁻¹ * b`
- Inverter transformações (ex: câmera → mundo)
- Criptografia

> ⚠️ **Armadilha:** NÃO calcule inversa para resolver sistemas! Use métodos mais eficientes (LU, Cholesky).

---

### 5. Composição de Transformações

#### Multiplicação Matriz-Matriz

Para aplicar `B` **primeiro**, depois `A`:

```
C = A * B
y = C * x = A * (B * x)
```

**⚠️ ORDEM INVERSA DA APLICAÇÃO!**

**Exemplo:**
```
1. Rotação 90° (R)
2. Escala 2× (S)

Matriz composta = S * R  (não R * S!)

R = [0 -1]    S = [2 0]
    [1  0]        [0 2]

T = S*R = [2 0] [0 -1] = [0 -2]
          [0 2] [1  0]   [2  0]
```

**Verificação passo a passo:**
```
Vetor inicial: (1, 0)

1. Aplica R: [0 -1] [1] = [0]
             [1  0] [0]   [1]

2. Aplica S: [2 0] [0] = [0]
             [0 2] [1]   [2]

Usando T direto: [0 -2] [1] = [0]
                 [2  0] [0]   [2]  ✓
```

> 💡 **Intuição:** Leia transformações da **direita para esquerda** como funções compostas: `f(g(x))`.

#### Propriedades

- **Não comutativa:** `AB ≠ BA` (ordem importa!)
- **Associativa:** `(AB)C = A(BC)`
- **Distributiva:** `A(B + C) = AB + AC`
- **Identidade:** `AI = IA = A`

#### Complexidade Computacional

Multiplicar matrizes `m×n` por `n×p`:

```
Algoritmo ingênuo: O(mnp)
Para n×n: O(n³)
```

**Otimizações práticas:**
- Strassen: O(n^2.807)
- BLAS/LAPACK: Altamente otimizados para CPU
- cuBLAS: GPU (paralelização massiva)
- Para grandes n (>1000), use bibliotecas!

---

### 6. Sistemas Lineares

#### Forma Matricial

Sistema:
```
a₁₁x₁ + a₁₂x₂ + ... = b₁
a₂₁x₁ + a₂₂x₂ + ... = b₂
...
```

Vira:
```
A * x = b
```

- `A`: matriz de coeficientes (m × n)
- `x`: vetor de incógnitas (n × 1)
- `b`: vetor de resultados (m × 1)

#### Tipos de Soluções

**1. Solução única** (sistema quadrado, det(A) ≠ 0):
```
x = A⁻¹ * b  (teoricamente)
```

**2. Infinitas soluções** (sistema subdeterminado):
- Mais variáveis que equações
- Ou linhas linearmente dependentes

**3. Sem solução** (sistema inconsistente):
- Equações contraditórias

> 🔗 **Conexão:** Em ML, sistema com infinitas soluções → regularização (Ridge, Lasso) escolhe uma!

#### Métodos de Solução

##### 1. Inversão de Matriz (❌ NÃO USE NA PRÁTICA!)

```
x = A⁻¹ * b
```

**Por que NÃO?**
- Custoso: O(n³) para calcular A⁻¹
- Numericamente instável
- Mais rápido/preciso usar outros métodos

##### 2. Eliminação Gaussiana

**Ideia:** Transformar em forma triangular superior

```
Antes:                Depois:
[a b c | d]           [a  b   c  | d ]
[e f g | h]     →     [0  f'  g' | h']
[i j k | l]           [0  0   k" | l"]
```

Resolve por **substituição reversa** (de baixo pra cima).

**Complexidade:** O(n³)

**Código (pseudocódigo):**
```python
# Eliminação
for k in range(n):
    for i in range(k+1, n):
        factor = A[i][k] / A[k][k]
        for j in range(k, n):
            A[i][j] -= factor * A[k][j]
        b[i] -= factor * b[k]

# Substituição reversa
x[n-1] = b[n-1] / A[n-1][n-1]
for i in range(n-2, -1, -1):
    x[i] = (b[i] - sum(A[i][j]*x[j] for j in range(i+1, n))) / A[i][i]
```

##### 3. Fatoração LU

**Ideia:** Decompor `A = L * U`
- `L`: triangular inferior (Lower)
- `U`: triangular superior (Upper)

```
Ax = b
LUx = b
```

Resolve em 2 etapas:
1. `Ly = b` (substituição direta)
2. `Ux = y` (substituição reversa)

**Vantagem:** Reutiliza L e U para múltiplos valores de b!

**Quando usar:** Resolver `Ax = b` para muitos b diferentes.

##### 4. Métodos Iterativos (para sistemas grandes)

**Jacobi:**
```
x^(k+1) = D⁻¹(b - (L + U)x^k)
```

**Gauss-Seidel:**
Usa valores já atualizados (geralmente converge mais rápido).

**Gradient Descent / Conjugate Gradient:**
Para sistemas `Ax = b` com A simétrica positiva definida.

**Quando usar:**
- Matrizes **muito grandes** (milhões de variáveis)
- Matrizes **esparsas** (muitos zeros)
- Não precisa de solução exata

> 🔗 **Conexão:** Em simulações físicas (FEM, CFD), sistemas têm milhões de variáveis!

---

### 7. Aplicações Práticas

#### Computação Gráfica (Pipeline 3D)

```cpp
// Transformar vértice 3D para tela
Vector4 vertex;
Matrix4 model, view, projection;

Vector4 clip = projection * view * model * vertex;
Vector3 screen = clip.xyz / clip.w;  // Divisão perspectiva
```

#### Regressão Linear (ML)

```
Minimizar ||Xw - y||²

Solução normal: w = (XᵀX)⁻¹Xᵀy
```

> ⚠️ **Armadilha:** Não calcule inversa! Use decomposição QR ou SVD.

#### Análise de Circuitos

Leis de Kirchhoff:
```
[Resistências] * [Correntes] = [Tensões]
```

#### PageRank (Google)

```
r = M * r  (autovetor com autovalor 1)
```

- `r`: vetor de ranks das páginas
- `M`: matriz de transição (links)

---

## ✅ Checklist de Compreensão

Você domina transformações lineares se consegue:

- [ ] Visualizar geometricamente o que uma matriz faz com vetores
- [ ] Construir matriz de uma transformação específica
- [ ] Aplicar transformações básicas (rotação, escala, reflexão) manualmente
- [ ] Explicar por que ordem de multiplicação importa (com exemplo)
- [ ] Calcular determinante 2×2 e 3×3
- [ ] Interpretar determinante (área, inversibilidade, orientação)
- [ ] Calcular inversa 2×2
- [ ] Saber quando sistema tem solução única/infinita/nenhuma
- [ ] Escolher método apropriado para resolver sistema
- [ ] Dar exemplos práticos de aplicações

---

## 🎯 Próximos Passos

### 1. **Praticar Agora** (RECOMENDADO)
📝 Faça exercícios: `k2-exercicios/e2-transformacoes-exercicios.md`
- Nível 1: Operações matriciais básicas
- Nível 2: Transformações geométricas
- Nível 3: Sistemas lineares

### 2. **Implementar**
💻 Veja código: `k3-implementacao/codigo/i2-matrizes.md`
- Struct Matrix (2×2, 3×3, 4×4)
- Multiplicação eficiente
- Determinante e inversa
- Transformações básicas

### 3. **Aplicar**
🎮 Problemas práticos: `k4-pratica/p1-basicos.md`
- Problema 1.2: Operações matriciais
- Problema 1.3: Resolver sistemas

### 4. **Projeto Principal**
🚀 Use no projeto: `k5-projeto/`
- Implementar transformações 2D
- Compor rotação + escala + translação

### 5. **Avançar**
➡️ Próximo tópico: `t3-autovalores-autovetores.md`
- Direções especiais que não mudam sob transformação
- Diagonalização
- Aplicações em PCA e sistemas dinâmicos

---

## 📚 Recursos Adicionais

### Vídeos
- 3Blue1Brown - Linear Transformations (Essence ch. 3-8)
- Khan Academy - Matrix transformations

### Leitura
- Gilbert Strang - Introduction to Linear Algebra (Cap. 2-3)
- Sheldon Axler - Linear Algebra Done Right

### Ferramentas
- GeoGebra - Visualizar transformações 2D/3D
- Matrix Calculator (online)

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
