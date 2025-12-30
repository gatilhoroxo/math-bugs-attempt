# Exercícios: Vetores e Espaços Vetoriais

## 🎯 Meta

Dominar operações com vetores (soma, produto escalar, produto vetorial, normalização) e entender conceitos de base, dependência linear e projeção.

---

## ⏱️ Tempo Estimado

- **Nível 1 (Iniciante):** 30-40 min
- **Nível 2 (Intermediário):** 35-50 min
- **Nível 3 (Avançado):** 40-55 min
- **Desafios:** 25-35 min
- **Total:** ~2h10-3h

---

## 📋 Quando Fazer

- **Após ler:** `k1-teoria/t1-vetores-espacos.md`
- **Antes de:** `k3-implementacao/codigo/i1-vetores.md`
- **Pré-requisitos:** Aritmética básica, trigonometria (sen, cos)

---

## 💪 Sistema de XP

- **Nível 1 (Iniciante):** 10 XP por exercício
- **Nível 2 (Intermediário):** 20 XP por exercício
- **Nível 3 (Avançado):** 30 XP por exercício
- **Desafio:** 50 XP

**XP Total Disponível:** 430 XP

---

## 📊 Rastreamento de Progresso

- [ ] Nível 1 completo (0/6 exercícios) - 60 XP
- [ ] Nível 2 completo (0/5 exercícios) - 100 XP
- [ ] Nível 3 completo (0/5 exercícios) - 150 XP
- [ ] Desafios completos (0/3 exercícios) - 150 XP

**XP Conquistado:** ___ / 460 XP

---

## Nível 1: Operações Básicas

### Exercício 1.1: Soma e Subtração de Vetores
Dados os vetores:
```
u = (2, 3)
v = (-1, 4)
w = (5, -2)
```

Calcule:
a) `u + v`
b) `u - v`
c) `u + v + w`
d) `2u - 3v`

---

### Exercício 1.2: Magnitude (Comprimento) de Vetores
Calcule a magnitude (norma) dos vetores:

a) `v = (3, 4)`
b) `u = (1, 1)`
c) `w = (5, 12)`
d) `a = (0, -7)`

**Fórmula:** `||v|| = √(x² + y²)`

---

### Exercício 1.3: Vetores Unitários
Normalize os seguintes vetores (encontre o vetor unitário na mesma direção):

a) `v = (3, 4)`
b) `u = (5, 0)`
c) `w = (1, 1)`
d) `a = (-6, 8)`

**Fórmula:** `v̂ = v / ||v||`

---

### Exercício 1.4: Produto Escalar (2D)
Calcule o produto escalar:

a) `(2, 3) · (4, 5)`
b) `(1, 0) · (0, 1)`
c) `(-2, 3) · (3, 2)`
d) `(5, -2) · (2, 5)`

**Fórmula:** `u · v = u_x * v_x + u_y * v_y`

---

### Exercício 1.5: Perpendicularidade
Verifique se os pares de vetores são perpendiculares (produto escalar = 0):

a) `(1, 2)` e `(-2, 1)`
b) `(3, 4)` e `(4, 3)`
c) `(5, 0)` e `(0, 3)`
d) `(1, 1)` e `(1, -1)`

---

### Exercício 1.6: Multiplicação por Escalar
Dado `v = (2, -3, 1)`, calcule:

a) `3v`
b) `-2v`
c) `0.5v`
d) `v + 2v - v`

---

## Nível 2: Produto Vetorial e Ângulos

### Exercício 2.1: Produto Vetorial em 3D
Calcule `u × v` para:

a) `u = (1, 0, 0)`, `v = (0, 1, 0)`
b) `u = (2, 3, 4)`, `v = (5, 6, 7)`
c) `u = (1, 2, 3)`, `v = (-1, -2, -3)`

**Fórmula:** 
```
u × v = (u_y*v_z - u_z*v_y, 
         u_z*v_x - u_x*v_z, 
         u_x*v_y - u_y*v_x)
```

---

### Exercício 2.2: Área de Paralelogramo
Encontre a área do paralelogramo formado pelos vetores:

a) `u = (3, 0)`, `v = (0, 4)`
b) `u = (2, 1)`, `v = (1, 3)`
c) `u = (5, 2)`, `v = (1, 4)`

**Dica:** Área = `||u × v||` (em 2D, trate como vetores 3D com z=0)

---

### Exercício 2.3: Ângulo Entre Vetores
Encontre o ângulo (em graus) entre os vetores:

a) `u = (1, 0)`, `v = (1, 1)`
b) `u = (3, 4)`, `v = (4, 3)`
c) `u = (1, 0)`, `v = (-1, 0)`

**Fórmula:** `cos(θ) = (u · v) / (||u|| ||v||)`

---

### Exercício 2.4: Projeção de Vetores
Calcule a projeção de `u` sobre `v`:

a) `u = (3, 4)`, `v = (1, 0)` (projetar sobre eixo x)
b) `u = (5, 5)`, `v = (1, 1)`
c) `u = (2, 3)`, `v = (4, 0)`

**Fórmula:** `proj_v(u) = ((u · v) / (v · v)) * v`

---

### Exercício 2.5: Componente Perpendicular
Dado `u = (5, 2)` e `v = (1, 0)`, encontre:

a) Componente de `u` paralela a `v`
b) Componente de `u` perpendicular a `v`
c) Verifique que `u = u_paralelo + u_perpendicular`

---

## Nível 3: Base e Dependência Linear

### Exercício 3.1: Combinação Linear
Expresse `w = (7, 11)` como combinação linear de `u = (1, 2)` e `v = (3, 4)`:
```
w = au + bv
```
Encontre `a` e `b`.

---

### Exercício 3.2: Dependência Linear
Determine se os conjuntos de vetores são linearmente dependentes:

a) `{(1, 2), (2, 4)}`
b) `{(1, 0), (0, 1)}`
c) `{(1, 2), (3, 4), (5, 6)}`
d) `{(1, 0, 0), (0, 1, 0), (0, 0, 1)}`

---

### Exercício 3.3: Base de R²
Verifique se os conjuntos formam uma base de R²:

a) `{(1, 0), (0, 1)}`
b) `{(1, 1), (1, -1)}`
c) `{(2, 3), (4, 6)}`
d) `{(3, 4), (-4, 3)}`

**Critério:** 2 vetores LI (linearmente independentes) em R²

---

### Exercício 3.4: Mudança de Base
Dado o vetor `v = (5, 3)` na base canônica, expresse-o na base:
```
B = {(1, 1), (1, -1)}
```

**Processo:** Resolver sistema `v = a*b₁ + b*b₂`

---

### Exercício 3.5: Produto Triplo Escalar
Calcule `u · (v × w)` para:
```
u = (1, 2, 3)
v = (4, 5, 6)
w = (7, 8, 9)
```

**Interpretação:** Volume do paralelepípedo formado pelos 3 vetores.

---

## Desafios

### Desafio 1: Distância de Ponto a Reta
Encontre a distância do ponto `P = (3, 4)` à reta que passa por `A = (0, 0)` e `B = (1, 0)`.

**Método:**
1. Vetor diretor da reta: `d = B - A`
2. Vetor `AP = P - A`
3. Componente perpendicular de `AP` em relação a `d`
4. Distância = magnitude do componente perpendicular

---

### Desafio 2: Reflexão de Vetor
Dado o vetor `v = (3, 4)` e o vetor normal `n = (1, 0)` (normal à superfície), calcule o vetor refletido `r`.

**Fórmula:** `r = v - 2(v · n)n`

**Aplicação:** Física de colisões, ray tracing

---

### Desafio 3: Centro de Massa
Três partículas têm:
- Massa `m₁ = 2kg` na posição `p₁ = (1, 2)`
- Massa `m₂ = 3kg` na posição `p₂ = (4, 5)`
- Massa `m₃ = 5kg` na posição `p₃ = (7, 1)`

Encontre a posição do centro de massa.

**Fórmula:** `CM = (Σ mᵢpᵢ) / (Σ mᵢ)`

---

## 📝 Gabarito

<details>
<summary>Exercício 1.1</summary>

a) `u + v = (2-1, 3+4) = (1, 7)`

b) `u - v = (2-(-1), 3-4) = (3, -1)`

c) `u + v + w = (1, 7) + (5, -2) = (6, 5)`

d) `2u - 3v = (4, 6) - (-3, 12) = (7, -6)`

</details>

<details>
<summary>Exercício 1.2</summary>

a) `||v|| = √(3² + 4²) = √25 = 5`

b) `||u|| = √(1² + 1²) = √2 ≈ 1.414`

c) `||w|| = √(5² + 12²) = √169 = 13`

d) `||a|| = √(0² + (-7)²) = 7`

</details>

<details>
<summary>Exercício 1.3</summary>

a) `||v|| = 5`, então `v̂ = (3/5, 4/5) = (0.6, 0.8)`

b) `||u|| = 5`, então `û = (1, 0)`

c) `||w|| = √2`, então `ŵ = (1/√2, 1/√2) ≈ (0.707, 0.707)`

d) `||a|| = 10`, então `â = (-0.6, 0.8)`

</details>

<details>
<summary>Exercício 1.4</summary>

a) `(2)(4) + (3)(5) = 8 + 15 = 23`

b) `(1)(0) + (0)(1) = 0` (perpendiculares!)

c) `(-2)(3) + (3)(2) = -6 + 6 = 0` (perpendiculares!)

d) `(5)(2) + (-2)(5) = 10 - 10 = 0` (perpendiculares!)

</details>

<details>
<summary>Exercício 1.5</summary>

a) `(1)(-2) + (2)(1) = -2 + 2 = 0` ✓ Perpendiculares

b) `(3)(4) + (4)(3) = 12 + 12 = 24` ✗ Não perpendiculares

c) `(5)(0) + (0)(3) = 0` ✓ Perpendiculares (eixos ortogonais)

d) `(1)(1) + (1)(-1) = 1 - 1 = 0` ✓ Perpendiculares

</details>

<details>
<summary>Exercício 1.6</summary>

a) `3v = (6, -9, 3)`

b) `-2v = (-4, 6, -2)`

c) `0.5v = (1, -1.5, 0.5)`

d) `v + 2v - v = 2v = (4, -6, 2)`

</details>

<details>
<summary>Exercício 2.1</summary>

a) `(1,0,0) × (0,1,0) = (0, 0, 1)` (regra da mão direita)

b) Usando fórmula:
```
x = (3)(7) - (4)(6) = 21 - 24 = -3
y = (4)(5) - (2)(7) = 20 - 14 = 6
z = (2)(6) - (3)(5) = 12 - 15 = -3
Resultado: (-3, 6, -3)
```

c) Vetores colineares → `u × v = (0, 0, 0)` (paralelos!)

</details>

<details>
<summary>Exercício 2.2</summary>

a) Área = `||(3, 0, 0) × (0, 4, 0)|| = ||(0, 0, 12)|| = 12`

b) `(2, 1, 0) × (1, 3, 0) = (0, 0, 5)` → Área = 5

c) `(5, 2, 0) × (1, 4, 0) = (0, 0, 18)` → Área = 18

</details>

<details>
<summary>Exercício 2.3</summary>

a) `cos(θ) = 1 / (1 * √2) = 1/√2` → θ = 45°

b) `cos(θ) = (12+12) / (5*5) = 24/25 = 0.96` → θ ≈ 16.26°

c) `cos(θ) = -1 / (1*1) = -1` → θ = 180° (opostos)

</details>

<details>
<summary>Exercício 2.4</summary>

a) `proj = ((3*1 + 4*0) / (1*1)) * (1, 0) = 3 * (1, 0) = (3, 0)`

b) `proj = ((5+5) / 2) * (1, 1) = 5 * (1, 1) = (5, 5)` (mesmo vetor!)

c) `proj = ((2*4) / 16) * (4, 0) = 0.5 * (4, 0) = (2, 0)`

</details>

<details>
<summary>Exercício 2.5</summary>

a) `u_paralelo = proj_v(u) = (5, 0)`

b) `u_perpendicular = u - u_paralelo = (5, 2) - (5, 0) = (0, 2)`

c) `u_paralelo + u_perpendicular = (5, 0) + (0, 2) = (5, 2) = u` ✓

</details>

<details>
<summary>Exercício 3.1</summary>

Sistema:
```
1a + 3b = 7
2a + 4b = 11
```

Resolvendo: `a = 2.5`, `b = 1.5`

Verificação: `2.5(1,2) + 1.5(3,4) = (2.5, 5) + (4.5, 6) = (7, 11)` ✓

</details>

<details>
<summary>Exercício 3.2</summary>

a) **Dependentes** - (2,4) = 2*(1,2) (múltiplo escalar)

b) **Independentes** - vetores canônicos, nenhum é múltiplo do outro

c) **Dependentes** - 3 vetores em R² sempre dependentes (dim = 2)

d) **Independentes** - base canônica de R³

</details>

<details>
<summary>Exercício 3.3</summary>

a) ✓ Base canônica (LI e span R²)

b) ✓ Não são múltiplos escalares, formam base

c) ✗ (4,6) = 2*(2,3), vetores dependentes

d) ✓ Perpendiculares (produto escalar = 0), LI

</details>

<details>
<summary>Exercício 3.4</summary>

Resolver:
```
(5, 3) = a(1, 1) + b(1, -1)
5 = a + b
3 = a - b
```

Somando: `2a = 8` → `a = 4`
Então: `b = 1`

Resposta: `v = 4b₁ + 1b₂` na base B

</details>

<details>
<summary>Exercício 3.5</summary>

1. `v × w = (5*9-6*8, 6*7-4*9, 4*8-5*7) = (-3, 6, -3)`

2. `u · (v × w) = (1)(-3) + (2)(6) + (3)(-3) = -3 + 12 - 9 = 0`

Volume = 0 → Vetores coplanares!

</details>

<details>
<summary>Desafio 1</summary>

1. Diretor da reta: `d = (1, 0) - (0, 0) = (1, 0)`

2. `AP = (3, 4) - (0, 0) = (3, 4)`

3. Projeção: `proj = 3 * (1, 0) = (3, 0)`

4. Perpendicular: `perp = (3, 4) - (3, 0) = (0, 4)`

5. Distância = `||perp|| = 4`

</details>

<details>
<summary>Desafio 2</summary>

1. `v · n = (3)(1) + (4)(0) = 3`

2. `r = v - 2(v·n)n = (3, 4) - 2(3)(1, 0) = (3, 4) - (6, 0) = (-3, 4)`

Verificação: Componente X inverteu (reflexão sobre eixo Y)

</details>

<details>
<summary>Desafio 3</summary>

1. Massa total: `M = 2 + 3 + 5 = 10kg`

2. Numerador: 
   ```
   Σ mᵢpᵢ = 2(1,2) + 3(4,5) + 5(7,1)
          = (2,4) + (12,15) + (35,5)
          = (49, 24)
   ```

3. Centro de massa: `CM = (49, 24) / 10 = (4.9, 2.4)`

</details>

---

## 🔗 Próximos Passos

### Após completar estes exercícios:

1. ✅ **Implementar:** `k3-implementacao/codigo/i1-vetores.md`
2. ✅ **Programar:** `k4-pratica/p1-basicos.md` (Problema 1: Biblioteca de Vetores)
3. ✅ **Teoria avançada:** `k1-teoria/t2-transformacoes-matrizes.md`

---

**Total XP disponível:** 460 XP  
**Tempo total estimado:** 2h10-3h  
**Dificuldade:** ⭐⭐ Iniciante-Intermediário
