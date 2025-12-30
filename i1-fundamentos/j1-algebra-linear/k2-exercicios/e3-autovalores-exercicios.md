# Exercícios: Autovalores e Autovetores

## 🎯 Meta

Dominar cálculo e interpretação de autovalores/autovetores, diagonalização, e aplicações práticas (PCA, sistemas dinâmicos).

---

## ⏱️ Tempo Estimado

- **Nível 1 (Iniciante):** 30-40 min
- **Nível 2 (Intermediário):** 35-45 min
- **Nível 3 (Avançado):** 40-55 min
- **Desafios:** 25-35 min
- **Total:** ~2h10-2h55

---

## 📋 Quando Fazer

- **Após ler:** `k1-teoria/t3-autovalores-autovetores.md`
- **Antes de:** `k3-implementacao/codigo/i3-autovetores.md`
- **Pré-requisitos:** Transformações e determinantes (e2 completo)

---

## 💪 Sistema de XP

- **Nível 1 (Iniciante):** 15 XP por exercício
- **Nível 2 (Intermediário):** 25 XP por exercício
- **Nível 3 (Avançado):** 35 XP por exercício
- **Desafio:** 60 XP

**XP Total Disponível:** 475 XP

---

## 📊 Rastreamento de Progresso

- [ ] Nível 1 completo (0/5 exercícios) - 75 XP
- [ ] Nível 2 completo (0/5 exercícios) - 125 XP
- [ ] Nível 3 completo (0/4 exercícios) - 140 XP
- [ ] Desafios completos (0/3 exercícios) - 180 XP

**XP Conquistado:** ___ / 520 XP

---

## Nível 1: Conceitos Básicos

### Exercício 1.1: Verificar Autovetor
Verifique se `v` é autovetor de `A` e encontre o autovalor correspondente:

```
A = [3  0]    v = [1]
    [0  2]        [0]
```

a) Calcule `A * v`

b) É autovetor? Se sim, qual λ?

c) Verifique `w = [0, 1]ᵀ`

---

### Exercício 1.2: Autovalores de Matriz Diagonal
Para matriz diagonal:

```
D = [5  0  0]
    [0  -2 0]
    [0  0  3]
```

a) Quais são os autovalores?

b) Quais são os autovetores?

c) Por que matrizes diagonais facilitam tudo?

---

### Exercício 1.3: Polinômio Característico 2×2
Encontre o polinômio característico de:

```
A = [4  1]
    [2  3]
```

a) Calcule `det(A - λI)`

b) Resolva para λ (autovalores)

c) Verifique: tr(A) = λ₁ + λ₂ e det(A) = λ₁ * λ₂

---

### Exercício 1.4: Interpretação Geométrica
Matriz:

```
A = [2  0]
    [0  2]
```

a) Quais os autovalores?

b) Qual transformação A representa geometricamente?

c) Toda direção é autovetor? Por quê?

---

### Exercício 1.5: Autovalor Zero
Matriz:

```
A = [1  2]
    [2  4]
```

a) Encontre autovalores

b) Um autovalor é zero - o que isso significa geometricamente?

c) A é invertível?

---

## Nível 2: Cálculo de Autovetores e Diagonalização

### Exercício 2.1: Autovalores e Autovetores Completo (2×2)
Matriz:

```
A = [3  1]
    [0  2]
```

a) Encontre autovalores (resolva det(A - λI) = 0)

b) Para cada λ, encontre autovetor resolvendo (A - λI)v = 0

c) Normalize os autovetores

d) Verifique `A*v = λ*v` para cada par

---

### Exercício 2.2: Diagonalização 2×2
Usando matriz do exercício 2.1:

a) Monte matriz `P` com autovetores como colunas

b) Monte matriz diagonal `D` com autovalores

c) Calcule `P⁻¹`

d) Verifique que `A = PDP⁻¹`

---

### Exercício 2.3: Potência de Matriz via Diagonalização
Matriz:

```
A = [4  1]
    [0  3]
```

a) Diagonalize: encontre P e D

b) Use `A^n = PD^nP⁻¹` para calcular `A^5`

c) Compare com calcular A⁵ diretamente (multiplicação repetida)

---

### Exercício 2.4: Autovalores Complexos
Matriz de rotação:

```
R = [0  -1]
    [1   0]
```

a) Encontre polinômio característico

b) Calcule autovalores (serão complexos!)

c) O que autovalores complexos significam geometricamente?

---

### Exercício 2.5: Matriz Simétrica
Matriz simétrica:

```
S = [2  1]
    [1  2]
```

a) Encontre autovalores

b) Encontre autovetores

c) Verifique que autovetores são ortogonais (produto escalar = 0)

d) Por que matrizes simétricas têm essa propriedade?

---

## Nível 3: Aplicações

### Exercício 3.1: Sistema Dinâmico Discreto
Sistema de população:

```
[jovens ]     [0    2  ] [jovens ]
[adultos]t+1 = [0.5  0.8] [adultos]t
```

a) Encontre autovalores da matriz de transição

b) Se λ₁ > 1, o que acontece com a população a longo prazo?

c) Se λ₁ < 1?

d) Encontre autovetor associado ao autovalor dominante (maior |λ|)

---

### Exercício 3.2: PCA Simplificado (2D)
Dados 2D com matriz de covariância:

```
C = [4  2]
    [2  3]
```

a) Encontre autovalores (variância nas direções principais)

b) Encontre autovetores (direções principais)

c) Qual direção tem maior variância?

d) Se quiser reduzir de 2D para 1D, qual direção escolher?

---

### Exercício 3.3: Estabilidade de Sistema Linear
Sistema dinâmico contínuo `dx/dt = Ax`:

```
A = [-1  0 ]
    [ 0  -2]
```

a) Encontre autovalores

b) Ambos são negativos - o que isso implica para estabilidade?

c) Qual direção decai mais rápido?

d) Esboce o comportamento de soluções ao longo do tempo

---

### Exercício 3.4: Matriz 3×3
Matriz:

```
A = [2  1  0]
    [1  2  0]
    [0  0  3]
```

a) Use estrutura de blocos para facilitar

b) Encontre autovalores

c) A é diagonalizável? (precisa de 3 autovetores LI)

---

## Desafios

### Desafio 1: PageRank Simplificado
Grafo de 3 páginas com matriz de transição:

```
M = [0    1/2  1]
    [1/2  0    0]
    [1/2  1/2  0]
```

a) Verifique que colunas somam 1 (matriz estocástica)

b) Encontre autovetor correspondente a λ = 1 (vetor de PageRank)

c) Normalize para que soma seja 1 (distribuição de probabilidade)

d) Qual página é mais importante?

---

### Desafio 2: Diagonalização de Matriz Singular
Matriz:

```
A = [1  1  1]
    [1  1  1]
    [1  1  1]
```

a) Encontre autovalores (um será 0)

b) Encontre autovetores

c) A é diagonalizável?

d) Qual o rank de A?

---

### Desafio 3: Exponencial de Matriz
Para matriz diagonalizável `A = PDP⁻¹`:

```
e^A = P * e^D * P⁻¹

onde e^D = [e^λ₁  0    ...  0   ]
           [ 0   e^λ₂  ...  0   ]
           [... ...    ... ...]
           [ 0    0    ... e^λₙ]
```

Calcule `e^A` para:

```
A = [1  1]
    [0  2]
```

a) Diagonalize A

b) Calcule `e^D`

c) Calcule `e^A = P * e^D * P⁻¹`

d) Verifique propriedade: `(e^A)ᵀ = e^(Aᵀ)`

---

## 📝 Gabarito

<details>
<summary>Exercício 1.1</summary>

a) `A*v = [3, 0]ᵀ`

b) Sim! `A*v = 3*v`, logo λ = 3

c) `w` também é autovetor com λ = 2

</details>

<details>
<summary>Exercício 1.2</summary>

a) Autovalores: 5, -2, 3 (diagonal)

b) Autovetores: e₁=(1,0,0), e₂=(0,1,0), e₃=(0,0,1)

c) Não há "torção", cada direção apenas escala

</details>

<details>
<summary>Exercício 1.3</summary>

a) `det(A - λI) = λ² - 7λ + 10`

b) λ = 5 ou λ = 2

c) tr(A) = 7 = 5+2 ✓, det(A) = 10 = 5*2 ✓

</details>

<details>
<summary>Exercício 1.4</summary>

a) λ₁ = λ₂ = 2

b) Escala uniforme por fator 2

c) Sim! Qualquer direção é autovetor (transformação isotrópica)

</details>

<details>
<summary>Exercício 1.5</summary>

a) λ₁ = 0, λ₂ = 5

b) Colapsa uma dimensão (projeção)

c) Não (det = 0)

</details>

<details>
<summary>Exercício 2.1</summary>

a) λ₁ = 3, λ₂ = 2

b) v₁ = (1, 0), v₂ = (-1, 1) (ou múltiplos)

c) v₁ já normalizado, v₂_norm = (-1/√2, 1/√2)

d) Verificação confirma `Av = λv` ✓

</details>

<details>
<summary>Exercício 2.2</summary>

`P = [1  -1]`, `D = [3  0]`, `P⁻¹ = [0  1]`
    `[0   1]`      `[0  2]`        `[0  1]`

Verificação: `PDP⁻¹ = A` ✓

</details>

<details>
<summary>Exercício 2.3</summary>

a) P e D encontrados via autovalores

b) `D^5 = [3⁵  0 ] = [243  0]`
         `[0  3⁵]   [0  243]`
   
   `A^5 = PD^5P⁻¹` (muito mais fácil!)

c) Diagonalização economiza 4 multiplicações matriciais

</details>

<details>
<summary>Exercício 2.4</summary>

a) `λ² + 1 = 0`

b) λ = ±i (puramente imaginários)

c) Rotação pura (sem componente de escala)

</details>

<details>
<summary>Exercício 2.5</summary>

a) λ₁ = 3, λ₂ = 1

b) v₁ = (1, 1), v₂ = (1, -1)

c) v₁ · v₂ = 1*1 + 1*(-1) = 0 ✓

d) Teorema espectral: matrizes simétricas reais têm autovetores ortogonais

</details>

<details>
<summary>Exercício 3.1</summary>

a) λ₁ ≈ 1.3, λ₂ ≈ -0.5

b) População cresce exponencialmente (λ₁ > 1)

c) População decai para zero

d) Autovetor dominante determina distribuição de equilíbrio

</details>

<details>
<summary>Exercício 3.2</summary>

a) λ₁ ≈ 5.83, λ₂ ≈ 1.17

b) v₁ ≈ (0.85, 0.53), v₂ ≈ (-0.53, 0.85)

c) v₁ (autovalor maior)

d) Projetar em v₁ mantém ~83% da variância

</details>

<details>
<summary>Exercício 3.3</summary>

a) λ₁ = -1, λ₂ = -2

b) Sistema estável (autovalores negativos → decai para zero)

c) Direção e₂ (λ₂ = -2, mais negativo)

d) Decaimento exponencial em ambas direções

</details>

<details>
<summary>Exercício 3.4</summary>

a) Bloco 2×2 superior separado de escalar 3

b) λ₁ = 1, λ₂ = 3 (multiplicidade 2)

c) Sim, 3 autovetores LI existem

</details>

<details>
<summary>Desafio 1</summary>

a) Colunas somam 1 ✓ (conservação de probabilidade)

b) v ≈ (0.4, 0.2, 0.4) (normalizado)

c) Páginas 1 e 3 empatadas em importância

d) PageRank real usa "damping factor" para convergência

</details>

<details>
<summary>Desafio 2</summary>

a) λ₁ = 3, λ₂ = λ₃ = 0

b) v₁ = (1,1,1), espaço nulo 2D

c) Sim, mas dois autovalores zero

d) rank(A) = 1

</details>

<details>
<summary>Desafio 3</summary>

a) λ₁ = 1, λ₂ = 2

b) `e^D = [e¹  0 ] = [e   0 ]`
         `[0  e²]   [0  e²]`

c) `e^A ≈ [2.72  2.72]`
         `[0     7.39]`

d) Verificação confirma propriedade

</details>

---

## 🎯 Próximos Passos

### Após completar estes exercícios:

1. ✅ **Implementar:** `k3-implementacao/codigo/i3-autovetores.md`
2. ✅ **Praticar código:** `k4-pratica/p2-intermediarios.md` (Problemas 2.3, 2.4)
3. ➡️ **Avançar teoria:** `k1-teoria/t4-decomposicoes.md`
4. ➡️ **Mais exercícios:** `e4-decomposicoes-exercicios.md`

---

**Total XP disponível:** 520 XP  
**Tempo total estimado:** 2h10-2h55  
**Dificuldade:** ⭐⭐⭐⭐☆ (Avançado)
