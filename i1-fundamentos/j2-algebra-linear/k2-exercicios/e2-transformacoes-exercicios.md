# Exercícios: Transformações Lineares e Sistemas

## 🎯 Meta

Dominar transformações lineares, operações matriciais, e resolução de sistemas lineares através de prática intensiva.

---

## ⏱️ Tempo Estimado

- **Nível 1 (Iniciante):** 25-35 min
- **Nível 2 (Intermediário):** 30-40 min
- **Nível 3 (Avançado):** 35-50 min
- **Desafios:** 20-30 min
- **Total:** ~2h-2h30

---

## 📋 Quando Fazer

- **Após ler:** `k1-teoria/t2-transformacoes-lineares.md`
- **Antes de:** `k3-implementacao/codigo/i2-matrizes.md`
- **Pré-requisitos:** Vetores e operações básicas (e1 completo)

---

## 💪 Sistema de XP

- **Nível 1 (Iniciante):** 10 XP por exercício
- **Nível 2 (Intermediário):** 20 XP por exercício
- **Nível 3 (Avançado):** 30 XP por exercício
- **Desafio:** 50 XP

**XP Total Disponível:** 440 XP

---

## 📊 Rastreamento de Progresso

- [ ] Nível 1 completo (0/6 exercícios) - 60 XP
- [ ] Nível 2 completo (0/6 exercícios) - 120 XP
- [ ] Nível 3 completo (0/5 exercícios) - 150 XP
- [ ] Desafios completos (0/3 exercícios) - 150 XP

**XP Conquistado:** ___ / 480 XP

---

## Nível 1: Operações Básicas com Matrizes

### Exercício 1.1: Multiplicação Matriz-Vetor
Dada a matriz `A` e vetor `v`:

```
A = [2  1]    v = [3]
    [0  3]        [2]
```

a) Calcule `A * v`

b) Interprete geometricamente: o que a transformação faz?

c) Verifique que `A * (2v) = 2 * (A * v)` (linearidade)

---

### Exercício 1.2: Construir Matriz de Transformação
Uma transformação linear `T: ℝ² → ℝ²` satisfaz:

```
T((1, 0)) = (3, 1)
T((0, 1)) = (2, 4)
```

a) Construa a matriz de `T`

b) Calcule `T((5, 7))`

c) A transformação preserva área? (calcule o determinante)

---

### Exercício 1.3: Matriz Transposta
Dada:

```
A = [1  2  3]
    [4  5  6]
```

a) Calcule `Aᵀ`

b) Verifique que `(Aᵀ)ᵀ = A`

c) Se `B = [7  8]`, calcule `(AB)ᵀ` e compare com `BᵀAᵀ`
        [9  10]
        [11 12]

---

### Exercício 1.4: Matriz Identidade
a) Escreva a matriz identidade `I` de tamanho 3×3

b) Verifique que `I * v = v` para `v = (2, 5, -1)`

c) Calcule `I²` (I multiplicado por si mesmo)

---

### Exercício 1.5: Determinante 2×2
Calcule o determinante das matrizes:

a) `A = [3  2]`
       `[1  4]`

b) `B = [5  10]`
       `[2  4]`

c) `C = [-1  2]`
       `[3  -6]`

d) Qual(is) matriz(es) é/são invertível(is)? Por quê?

---

### Exercício 1.6: Multiplicação Matriz-Matriz
Calcule `A * B`:

```
A = [1  2]    B = [5  6]
    [3  4]        [7  8]
```

a) Calcule elemento por elemento

b) Verifique se `A * B = B * A` (teste comutatividade)

c) Calcule `det(A * B)` e compare com `det(A) * det(B)`

---

## Nível 2: Transformações Geométricas

### Exercício 2.1: Rotação 2D
Construa matriz de rotação `R(θ)` para:

a) θ = 90° (π/2 radianos)

b) θ = 180° (π radianos)

c) θ = -45° (-π/4 radianos)

d) Aplique R(90°) ao vetor `(1, 0)` e visualize o resultado

---

### Exercício 2.2: Escala 2D
a) Construa matriz que dobra x e triplica y

b) Aplique a transformação ao vetor `(2, 1)`

c) Qual o determinante? O que ele representa?

d) Construa matriz que inverte o sinal de y (reflexão sobre eixo x)

---

### Exercício 2.3: Composição de Transformações
Dadas:

```
R = Rotação de 45°
S = Escala por fator 2 em ambas direções
```

a) Calcule `S * R` (rotacionar depois escalar)

b) Calcule `R * S` (escalar depois rotacionar)

c) Aplique ambas a `v = (1, 0)` e compare resultados

d) Explique por que são diferentes

---

### Exercício 2.4: Cisalhamento (Shear)
Matriz de cisalhamento horizontal:

```
H = [1  k]
    [0  1]
```

a) Para `k = 2`, aplique a `(0, 1)`, `(1, 1)`, `(2, 1)`

b) Desenhe os pontos originais e transformados

c) Qual o determinante de `H`? O que isso significa?

d) Encontre `H⁻¹` (inversa)

---

### Exercício 2.5: Matriz Inversa 2×2
Calcule a inversa de:

```
A = [3  1]
    [2  1]
```

a) Use a fórmula `A⁻¹ = (1/det(A)) * [d  -b]`
                                       `[-c  a]`

b) Verifique que `A * A⁻¹ = I`

c) Resolva `A * x = b` para `b = (5, 3)` usando `x = A⁻¹ * b`

---

### Exercício 2.6: Transformações 3D - Rotação em Z
Matriz de rotação 3D em torno do eixo Z:

```
Rz(θ) = [cos(θ)  -sin(θ)  0]
        [sin(θ)   cos(θ)  0]
        [  0        0     1]
```

a) Escreva `Rz(90°)`

b) Aplique a `v = (1, 0, 0)`

c) Aplique a `v = (0, 0, 1)` - o que acontece? Por quê?

---

## Nível 3: Sistemas Lineares

### Exercício 3.1: Sistema 2×2 - Método de Eliminação
Resolva por eliminação gaussiana:

```
2x + y = 5
x + 3y = 8
```

a) Escreva na forma matricial `Ax = b`

b) Reduza para forma escalonada

c) Resolva por substituição reversa

d) Verifique a solução

---

### Exercício 3.2: Sistema com Múltiplas Soluções
Analise o sistema:

```
x + 2y = 3
2x + 4y = 6
```

a) Escreva na forma matricial

b) Calcule `det(A)` - o que ele indica?

c) Qual o tipo de solução? (única/infinita/nenhuma)

d) Descreva o conjunto solução geometricamente

---

### Exercício 3.3: Sistema 3×3
Resolva:

```
x + y + z = 6
2x - y + z = 3
x + 2y - z = 2
```

a) Escreva na forma matricial

b) Use eliminação gaussiana

c) Substitua e verifique

---

### Exercício 3.4: Determinante 3×3
Calcule o determinante:

```
A = [1  2  3]
    [0  4  5]
    [1  0  6]
```

a) Use expansão por cofatores (primeira linha)

b) A matriz é invertível?

c) Se sim, qual o significado geométrico do determinante?

---

### Exercício 3.5: Aplicação - Circuito Elétrico
Usando leis de Kirchhoff, temos o sistema:

```
I₁ + I₂ = I₃
5I₁ + 10I₂ = 15
10I₂ + 20I₃ = 30
```

a) Resolva para `I₁`, `I₂`, `I₃`

b) Verifique as soluções nas equações originais

---

## Desafios

### Desafio 1: Matriz de Projeção
Construa matriz `P` que projeta vetores sobre a reta `y = x`.

**Dicas:**
- Vetor `(1, 1)` deve permanecer inalterado
- Vetor `(1, -1)` deve ir para zero (perpendicular à reta)

a) Encontre `P`

b) Verifique que `P² = P` (propriedade de projeção)

c) Qual o determinante de `P`? Por quê?

---

### Desafio 2: Coordenadas Homogêneas
Para fazer translação em 2D, usamos coordenadas homogêneas (3D):

```
[x']   [1  0  tx] [x]
[y'] = [0  1  ty] [y]
[1 ]   [0  0  1 ] [1]
```

a) Construa matriz para transladar por `(3, -2)`

b) Aplique a ponto `(1, 1)`

c) Componha com rotação de 45° (ordem: rotação depois translação)

d) Compare com ordem inversa

---

### Desafio 3: Potência de Matriz
Dada:

```
A = [0.8  0.3]
    [0.2  0.7]
```

a) Calcule `A²`, `A³`, `A⁴`

b) O que você observa quando `n → ∞` em `Aⁿ`?

c) Isso tem relação com autovalores? (conceito do próximo tópico!)

---

## 📝 Gabarito

<details>
<summary>Exercício 1.1</summary>

a) `A * v = [2*3 + 1*2] = [8]`
           `[0*3 + 3*2]   [6]`

b) Escala componente y por 3, adiciona metade de y ao x

c) `A*(2v) = A*[6] = [16] = 2*[8] = 2*(A*v)` ✓
              [4]   [12]     [6]

</details>

<details>
<summary>Exercício 1.2</summary>

a) `A = [3  2]` (colunas são imagens da base)
       `[1  4]`

b) `T((5,7)) = [3  2] [5] = [29]`
               `[1  4] [7]   [33]`

c) `det(A) = 12 - 2 = 10` → Não preserva (multiplica área por 10)

</details>

<details>
<summary>Exercício 1.3</summary>

a) `Aᵀ = [1  4]`
        `[2  5]`
        `[3  6]`

b) Transposing twice returns original ✓

c) `(AB)ᵀ = BᵀAᵀ` (propriedade fundamental)

</details>

<details>
<summary>Exercício 1.4</summary>

a) `I = [1  0  0]`
       `[0  1  0]`
       `[0  0  1]`

b) `I*v = v` sempre ✓

c) `I² = I` (identidade vezes identidade = identidade)

</details>

<details>
<summary>Exercício 1.5</summary>

a) `det(A) = 12 - 2 = 10` → **invertível**

b) `det(B) = 20 - 20 = 0` → **não invertível**

c) `det(C) = 6 - 6 = 0` → **não invertível**

d) Apenas A é invertível (det ≠ 0)

</details>

<details>
<summary>Exercício 1.6</summary>

a) `A*B = [19  22]`
         `[43  50]`

b) `B*A = [23  34]` → `A*B ≠ B*A` (não comutativa!)
         `[31  46]`

c) `det(A*B) = det(A) * det(B)` (propriedade do determinante)

</details>

<details>
<summary>Exercício 2.1</summary>

a) `R(90°) = [0  -1]`
            `[1   0]`

b) `R(180°) = [-1  0]`
             `[0  -1]`

c) `R(-45°) = [√2/2   √2/2]`
             `[-√2/2  √2/2]`

d) `R(90°)*(1,0) = (0,1)` → gira 90° anti-horário

</details>

<details>
<summary>Exercício 2.2</summary>

a) `S = [2  0]`
       `[0  3]`

b) `S*(2,1) = (4,3)`

c) `det(S) = 6` → multiplica área por 6

d) `Fx = [1   0]`
        `[0  -1]`

</details>

<details>
<summary>Exercício 2.3</summary>

a/b) `S*R ≠ R*S` em geral

c) Resultados diferentes demonstram não-comutatividade

d) Ordem de transformações importa! Rotacionar-depois-escalar ≠ Escalar-depois-rotacionar

</details>

<details>
<summary>Exercício 2.4</summary>

a) `(0,1)→(2,1)`, `(1,1)→(3,1)`, `(2,1)→(4,1)` (desloca horizontalmente proporcional a y)

b) Pontos formam paralelogramo

c) `det(H) = 1` → preserva área

d) `H⁻¹ = [1  -k]`
         `[0   1]`

</details>

<details>
<summary>Exercício 2.5</summary>

a) `det(A) = 1`, `A⁻¹ = [1  -1]`
                        `[-2  3]`

b) Verificação confirma `A*A⁻¹ = I` ✓

c) `x = A⁻¹*b = [2]`
                `[1]`

</details>

<details>
<summary>Exercício 2.6</summary>

a) `Rz(90°) = [0  -1  0]`
             `[1   0  0]`
             `[0   0  1]`

b) `(1,0,0) → (0,1,0)`

c) `(0,0,1) → (0,0,1)` (eixo de rotação não muda!)

</details>

<details>
<summary>Exercício 3.1</summary>

Solução: `x = 1, y = 3`

Forma matricial: `[2  1] [x] = [5]`
                 `[1  3] [y]   [8]`

</details>

<details>
<summary>Exercício 3.2</summary>

a) `A = [1  2]`, `b = [3]`
       `[2  4]`       `[6]`

b) `det(A) = 0` → sistema não tem solução única

c) **Infinitas soluções** (segunda equação é múltipla da primeira)

d) Geometricamente: duas retas coincidentes

</details>

<details>
<summary>Exercício 3.3</summary>

Solução: `x = 1, y = 2, z = 3`

(Use eliminação gaussiana passo a passo)

</details>

<details>
<summary>Exercício 3.4</summary>

a) `det(A) = 1(24-0) - 2(0-5) + 3(0-4) = 24 + 10 - 12 = 22`

b) Sim, invertível (det ≠ 0)

c) Transformação escala volume por fator 22

</details>

<details>
<summary>Exercício 3.5</summary>

Solução: `I₁ = 0.5A, I₂ = 1A, I₃ = 1.5A`

(Sistema linear simples)

</details>

<details>
<summary>Desafio 1</summary>

`P = [0.5  0.5]`
    `[0.5  0.5]`

Propriedades: `P² = P` ✓, `det(P) = 0` (colapsa dimensão)

</details>

<details>
<summary>Desafio 2</summary>

a) `T = [1  0  3]`
       `[0  1  -2]`
       `[0  0  1]`

b) `(1,1) → (4,-1)`

c/d) Ordem de composição importa muito em coord. homogêneas!

</details>

<details>
<summary>Desafio 3</summary>

a/b) Matriz converge para estado estacionário

c) Sim! Autovalor dominante determina comportamento assintótico

</details>

---

## 🎯 Próximos Passos

### Após completar estes exercícios:

1. ✅ **Implementar:** `k3-implementacao/codigo/i2-matrizes.md`
2. ✅ **Praticar código:** `k4-pratica/p1-basicos.md` (Problemas 1.2, 1.5)
3. ➡️ **Avançar teoria:** `k1-teoria/t3-autovalores-autovetores.md`
4. ➡️ **Mais exercícios:** `e3-autovalores-exercicios.md`

---

**Total XP disponível:** 480 XP  
**Tempo total estimado:** 2h-2h30  
**Dificuldade:** ⭐⭐⭐☆☆ (Intermediário)
