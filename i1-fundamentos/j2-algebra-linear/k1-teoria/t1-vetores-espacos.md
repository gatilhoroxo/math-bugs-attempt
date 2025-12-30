# Vetores e Espaços Vetoriais

## 🎯 Meta de Aprendizado

Ao completar este tópico, você será capaz de:
- Entender vetores como **entidades geométricas** (não apenas arrays)
- Calcular e interpretar produtos escalar e vetorial
- Determinar independência linear e bases de espaços vetoriais
- Aplicar vetores em problemas de CS (gráficos, ML, física)

---

## ⏱️ Tempo Estimado

- **Leitura ativa:** 45-60 min
- **Exercícios relacionados:** 30-45 min (`k2-exercicios/e1-vetores-exercicios.md`)
- **Implementação:** 60-90 min (`k3-implementacao/codigo/i1-vetores.md`)
- **Total:** ~2h30-3h15

---

## 📋 Pré-requisitos

- [ ] Geometria básica (ângulos, distâncias)
- [ ] Trigonometria (seno, cosseno)
- [ ] Álgebra (sistemas lineares simples)
- [ ] Nenhum tópico anterior de AL (este é o primeiro!)

---

## 🗺️ Mapa Mental

```
VETORES E ESPAÇOS
├── 1. Vetores (entidade geométrica)
│   ├── Operações básicas
│   │   ├── Soma/subtração
│   │   ├── Multiplicação por escalar
│   │   └── Magnitude e normalização
│   ├── Produtos
│   │   ├── Escalar (dot) → mede ângulo
│   │   └── Vetorial (cross) → perpendicular
│   └── Aplicações
│       ├── Computação gráfica (normais)
│       ├── Física (forças, velocidades)
│       └── ML (features são vetores)
│
└── 2. Espaços Vetoriais
    ├── Definição e axiomas
    ├── Subespaços
    ├── Combinação linear e span
    ├── Independência linear
    ├── Base e dimensão
    └── Base ortonormal → simplifica tudo!
```

---

## 📖 Conteúdo

### 1. Por que Álgebra Linear importa?

Álgebra Linear é a **linguagem da computação moderna**. Enquanto em Cálculo você trabalha com funções de uma variável (f(x)), em AL você trabalha com *múltiplas* variáveis simultaneamente.

**Aplicações diretas em CS:**

- **Machine Learning**: Cada dado é um vetor, cada modelo é uma transformação matricial
- **Computação Gráfica**: Toda rotação, movimento e projeção 3D é uma matriz
- **Robótica**: Posição e orientação de robôs são vetores/matrizes
- **Compressão**: SVD (decomposição de valores singulares) é a base do JPEG
- **Redes Neurais**: Literalmente camadas de multiplicações matriciais

> 💡 **Intuição:** Se você trabalha com dados (números múltiplos), trabalha com vetores. Se transforma dados, usa matrizes.

**Exemplo em código:**

```python
# Machine Learning - Regressão Linear
y = X @ w + b  # @ é multiplicação matricial
# X: matriz de dados (n_samples × n_features)
# w: vetor de pesos
# y: predições
```

```cpp
// Computação Gráfica - Rotação 3D
Vector3 rotated = rotationMatrix * originalPoint;
```

---

### 2. Vetores: Mais que "Listas de Números"

**❌ Visão limitada:** "Vetor é tipo um array [x, y, z]"

**✅ Visão correta:** "Vetor é uma **direção e magnitude** em um espaço"

#### Analogia 🏴‍☠️

Pensa no **Log Pose** do One Piece: ele aponta uma **direção** (vetor unitário) com uma certa **intensidade** de atração magnética (magnitude). O vetor não é só "coordenadas", é uma **entidade geométrica** que existe independente do sistema de coordenadas!

#### Duas Interpretações Importantes

**Geométrica (para entender):**
- Vetor é uma **seta** com direção e comprimento
- Independente de onde você desenha (pode transladar)
- Visualize no espaço físico

**Computacional (para implementar):**
- Vetor é um **array** de números: `[x, y, z]`
- Operações são loops e aritmética
- Pense em eficiência (cache, SIMD)

> ⚠️ **Armadilha Comum:** Confundir ponto com vetor. Ponto é uma **posição** (absoluta), vetor é um **deslocamento** (relativo).

---

### 3. Operações com Vetores

#### 3.1 Soma e Multiplicação por Escalar

**Soma:** `a + b` (regra do paralelogramo)
```
a = (1, 2)
b = (3, 1)
a + b = (4, 3)
```

**Multiplicação:** `k * v` (escala magnitude)
```
v = (2, 1)
2 * v = (4, 2)  (dobro do tamanho, mesma direção)
-1 * v = (-2, -1)  (inverte direção)
```

> 💡 **Intuição:** Soma = "navegue em direção a, depois em direção b". Escalar = "estica/comprime o vetor".

#### 3.2 Magnitude (Norma)

**Definição:** Comprimento do vetor
```
|v| = √(v_x² + v_y² + v_z²)
```

**Exemplo:**
```
v = (3, 4, 0)
|v| = √(9 + 16) = 5
```

**Normalização:** Vetor unitário (comprimento 1) na mesma direção
```
v̂ = v / |v|
```

> 🔗 **Conexão:** Em gráficos 3D, normais de superfície são sempre vetores unitários.

#### 3.3 Produto Escalar (Dot Product)

**Fórmula:**
```
a · b = a_x*b_x + a_y*b_y + a_z*b_z
a · b = |a| |b| cos(θ)  (θ = ângulo entre vetores)
```

**Interpretação geométrica:**
- `a · b > 0`: apontam para o mesmo lado
- `a · b = 0`: perpendiculares (ortogonais)
- `a · b < 0`: apontam para lados opostos

**Aplicações:**
- Calcular ângulo entre vetores: `θ = arccos((a·b) / (|a||b|))`
- Projetar vetor sobre outro: `proj_b(a) = ((a·b)/(b·b)) * b`
- Iluminação em CG: `intensidade = normal · luz`

**Exemplo:**
```
a = (1, 0, 0)  (eixo X)
b = (0, 1, 0)  (eixo Y)
a · b = 0  → perpendiculares ✓
```

> ⚠️ **Armadilha:** Produto escalar retorna um **número**, não um vetor!

#### 3.4 Produto Vetorial (Cross Product)

**Definição:** Gera vetor **perpendicular** a dois outros (só em 3D!)

**Fórmula:**
```
a × b = (a_y*b_z - a_z*b_y,
         a_z*b_x - a_x*b_z,
         a_x*b_y - a_y*b_x)
```

**Propriedades:**
- `|a × b| = |a| |b| sin(θ)` (área do paralelogramo)
- `a × b = -(b × a)` (anti-comutativo)
- `a × a = 0`

**Regra da mão direita:** Dedos de a para b, polegar aponta para a×b

**Aplicações:**
- Calcular normal de superfície (gráficos 3D)
- Torque em física: `τ = r × F`
- Determinar se três pontos são colineares

**Exemplo:**
```
e_x = (1, 0, 0)
e_y = (0, 1, 0)
e_x × e_y = (0, 0, 1) = e_z  ✓
```

> 🔗 **Conexão:** Em engines 3D, ao criar uma malha, você usa cross product para calcular normais dos triângulos.

---

### 4. Espaços Vetoriais

**Definição:** Um "universo" onde vetores vivem, com regras bem definidas.

#### Axiomas (resumo)

Para ser espaço vetorial, precisa:
1. **Fechamento:** `u + v ∈ V` e `k*v ∈ V`
2. **Comutatividade:** `u + v = v + u`
3. **Elemento neutro:** Existe `0` tal que `v + 0 = v`
4. **Elemento oposto:** Para cada `v`, existe `-v`
5. Distributividade e outras propriedades...

**Exemplos:**
- ℝ² (vetores 2D), ℝ³ (vetores 3D), ℝⁿ
- Polinômios de grau ≤ n
- Matrizes m × n
- Funções contínuas

> 💡 **Intuição:** Se você pode somar e escalar elementos seguindo regras "naturais", provavelmente é um espaço vetorial.

#### Subespaços

Subconjunto de um espaço vetorial que **também** é espaço vetorial.

**Teste rápido:**
1. Contém vetor zero?
2. Fechado sob adição?
3. Fechado sob multiplicação por escalar?

**Exemplos:**
- Uma reta passando pela origem em ℝ²
- Um plano passando pela origem em ℝ³
- {0} (subespaço trivial)

> ⚠️ **Armadilha:** Reta ou plano **não passando pela origem** NÃO é subespaço!

---

### 5. Combinação Linear, Span e Independência

#### Combinação Linear

```
v = c₁v₁ + c₂v₂ + ... + cₙvₙ
```

Onde `c₁, c₂, ..., cₙ` são escalares.

#### Span (Espaço Gerado)

**Conjunto de TODAS** as combinações lineares:

```
span(v₁, v₂, ..., vₙ) = {c₁v₁ + c₂v₂ + ... | c₁, c₂, ... ∈ ℝ}
```

**Exemplos:**
- `span((1,0))` em ℝ² = eixo X (reta)
- `span((1,0), (0,1))` em ℝ² = todo o plano ℝ²
- `span((1,2), (2,4))` em ℝ² = reta (vetores paralelos)

> 💡 **Intuição:** Span é "todos os lugares que você consegue chegar" combinando os vetores.

#### Independência Linear

Vetores são **linearmente independentes** se:

```
c₁v₁ + c₂v₂ + ... + cₙvₙ = 0  ⟹  c₁ = c₂ = ... = cₙ = 0
```

Ou seja: **nenhum vetor é combinação dos outros**.

**Teste prático:**
- ℝ²: dois vetores são LI se não são paralelos
- ℝ³: três vetores são LI se não são coplanares
- Geral: monte matriz com vetores como colunas, calcule det (se det ≠ 0, são LI)

**Exemplo:**
```
v₁ = (1, 0, 0)
v₂ = (0, 1, 0)
v₃ = (0, 0, 1)
→ Linearmente independentes (base canônica)

v₁ = (1, 2)
v₂ = (2, 4)  = 2*v₁
→ Linearmente dependentes
```

> ⚠️ **Armadilha:** Vetor zero sempre torna conjunto LD (linearmente dependente).

---

### 6. Base e Dimensão

**Base:** Conjunto de vetores que:
1. São **linearmente independentes**
2. **Geram** todo o espaço (span = espaço)

**Propriedade fundamental:** Todas as bases de um espaço têm o **mesmo número** de vetores!

**Dimensão:** Número de vetores em qualquer base.

```
dim(ℝⁿ) = n
```

**Base canônica de ℝ³:**
```
e₁ = (1, 0, 0)
e₂ = (0, 1, 0)
e₃ = (0, 0, 1)
```

Qualquer vetor `v = (x, y, z)` pode ser escrito:
```
v = x*e₁ + y*e₂ + z*e₃
```

> 💡 **Intuição:** Base é o "sistema de coordenadas" do espaço. Coordenadas são os coeficientes da combinação linear.

#### Base Ortonormal

Base onde vetores são:
- **Ortogonais:** `vᵢ · vⱼ = 0` se `i ≠ j`
- **Normalizados:** `|vᵢ| = 1`

**Por que é importante:**
- Simplifica cálculos (projeções = simples dot product)
- Muitos algoritmos assumem base ortonormal
- Facilita mudança de coordenadas

**Processo de Gram-Schmidt:** Transforma base qualquer em ortonormal.

> 🔗 **Conexão:** No próximo tópico (t2), veremos que matrizes ortogonais preservam ângulos e comprimentos!

---

## ✅ Checklist de Compreensão

Você entendeu vetores e espaços se consegue:

- [ ] Explicar a diferença entre vetor como "array" e como "entidade geométrica"
- [ ] Calcular produto escalar e **interpretar** o resultado (ângulo)
- [ ] Calcular produto vetorial e saber **quando** usar
- [ ] Determinar se vetores são linearmente independentes
- [ ] Calcular span de um conjunto de vetores
- [ ] Identificar se um conjunto é uma base
- [ ] Explicar o conceito de dimensão com suas palavras
- [ ] Dar exemplo de aplicação prática em CS

---

## 🎯 Próximos Passos

### 1. **Praticar Agora** (RECOMENDADO)
📝 Faça exercícios: `k2-exercicios/e1-vetores-exercicios.md`
- Nível 1: Fixar operações básicas
- Nível 2: Independência e bases
- Nível 3: Aplicações práticas

### 2. **Implementar**
💻 Veja código: `k3-implementacao/codigo/i1-vetores.md`
- Implementar struct Vec3
- Todas as operações (dot, cross, etc)
- Testar com casos conhecidos

### 3. **Aplicar**
🎮 Problemas práticos: `k4-pratica/p1-basicos.md`
- Problema 1.1: Biblioteca de vetores 3D
- Problema 1.4: Calcular ângulos

### 4. **Avançar**
➡️ Próximo tópico: `t2-transformacoes-lineares.md`
- Como matrizes transformam vetores
- Composição de transformações
- Aplicações em gráficos 2D/3D

---

## 📚 Recursos Adicionais

### Vídeos Recomendados
- 3Blue1Brown - Essence of Linear Algebra (capítulos 1-4)
- Khan Academy - Vectors and Spaces

### Leitura Complementar
- Gilbert Strang - Introduction to Linear Algebra (Cap. 1)
- Ver `../recursos.md` para lista completa

### Ferramentas Interativas
- GeoGebra - Visualizar vetores 3D
- Desmos - Plotar vetores 2D

---

## 💬 Notas de Aprendizado

**Espaço para suas anotações:**

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
