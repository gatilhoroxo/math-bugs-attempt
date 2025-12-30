# Exercícios: Decomposições Matriciais

## 🎯 Meta

Dominar decomposições LU, QR, Cholesky e SVD, entendendo quando usar cada uma e suas aplicações práticas (compressão, ML, sistemas lineares).

---

## ⏱️ Tempo Estimado

- **Nível 1 (Iniciante):** 35-45 min
- **Nível 2 (Intermediário):** 40-55 min
- **Nível 3 (Avançado):** 45-60 min
- **Desafios:** 30-45 min
- **Total:** ~2h30-3h25

---

## 📋 Quando Fazer

- **Após ler:** `k1-teoria/t4-decomposicoes.md`
- **Antes de:** `k3-implementacao/codigo/i4-decomposicoes.md`
- **Pré-requisitos:** Autovalores e diagonalização (e3 completo)

---

## 💪 Sistema de XP

- **Nível 1 (Iniciante):** 15 XP por exercício
- **Nível 2 (Intermediário):** 30 XP por exercício
- **Nível 3 (Avançado):** 40 XP por exercício
- **Desafio:** 70 XP

**XP Total Disponível:** 545 XP

---

## 📊 Rastreamento de Progresso

- [ ] Nível 1 completo (0/5 exercícios) - 75 XP
- [ ] Nível 2 completo (0/5 exercícios) - 150 XP
- [ ] Nível 3 completo (0/4 exercícios) - 160 XP
- [ ] Desafios completos (0/3 exercícios) - 210 XP

**XP Conquistado:** ___ / 595 XP

---

## Nível 1: Decomposição LU

### Exercício 1.1: LU Manual (2×2)
Fatore em LU:

```
A = [4  3]
    [6  3]
```

a) Encontre L (triangular inferior) e U (triangular superior)

b) Verifique que `L * U = A`

c) Use LU para resolver `Ax = b` com `b = [10, 12]ᵀ`

---

### Exercício 1.2: LU com Pivotamento
Matriz:

```
A = [0  1]
    [1  1]
```

a) Tente LU direta - o que acontece?

b) Troque linhas (pivotamento) e refaça

c) Escreva na forma `PA = LU`

---

### Exercício 1.3: LU para Matriz 3×3
Fatore:

```
A = [2  1  1]
    [4  3  3]
    [8  7  9]
```

a) Use eliminação gaussiana para encontrar U

b) Registre multiplicadores para montar L

c) Verifique `L * U = A`

---

### Exercício 1.4: Resolver Múltiplos Sistemas com LU
Dada fatoração `A = LU` do exercício 1.3, resolva `Ax = b` para:

a) `b₁ = [1, 2, 3]ᵀ`

b) `b₂ = [0, 1, 0]ᵀ`

c) Por que LU é vantajoso aqui?

---

### Exercício 1.5: Determinante via LU
Use fatoração LU para calcular det(A):

```
A = [3  1]
    [2  2]
```

**Dica:** `det(A) = det(L) * det(U)` e determinante de triangular = produto da diagonal

---

## Nível 2: Decomposição QR

### Exercício 2.1: Gram-Schmidt (2 vetores)
Ortonormalize vetores:

```
a₁ = [3, 4]ᵀ
a₂ = [1, 0]ᵀ
```

a) Calcule `q₁ = a₁ / ||a₁||`

b) Projete a₂ em q₁: `proj = (a₂·q₁)q₁`

c) Calcule `u₂ = a₂ - proj` e normalize: `q₂ = u₂ / ||u₂||`

d) Verifique que `q₁ · q₂ = 0`

---

### Exercício 2.2: Decomposição QR (2×2)
Matriz:

```
A = [3  1]
    [4  0]
```

a) Use Gram-Schmidt nas colunas para obter Q

b) Calcule `R = Qᵀ * A`

c) Verifique que `A = Q * R`

d) Verifique que `QᵀQ = I`

---

### Exercício 2.3: Resolver Sistema com QR
Use QR do exercício 2.2 para resolver `Ax = b` com `b = [7, 8]ᵀ`:

a) Calcule `c = Qᵀb`

b) Resolva `Rx = c` (triangular superior)

c) Compare com resolver por inversão direta

---

### Exercício 2.4: Projeção de Mínimos Quadrados
Dados pontos `(0,1), (1,2), (2,2)`, ajustar reta `y = a + bx`:

```
Sistema sobre-determinado:
[1  0] [a]   [1]
[1  1] [b] = [2]
[1  2]       [2]
```

a) Forme `AᵀAx = Aᵀb` (equações normais)

b) Resolva para a, b

c) Alternativamente, use QR de A

---

### Exercício 2.5: Propriedades de Q
Matriz ortogonal:

```
Q = [cos(θ)  -sin(θ)]
    [sin(θ)   cos(θ)]
```

a) Verifique que `QᵀQ = I`

b) Calcule `det(Q)`

c) Mostre que `||Qx|| = ||x||` (preserva norma)

---

## Nível 3: Cholesky e SVD

### Exercício 3.1: Cholesky (2×2)
Fatore matriz simétrica positiva definida:

```
A = [4  2]
    [2  3]
```

a) Encontre L tal que `A = LLᵀ`

b) Verifique resultado

c) Compare custo com LU

---

### Exercício 3.2: Teste de Positividade Definida
Verifique se matrizes são positivas definidas (todos autovalores > 0):

a) `A = [2  1]`
       `[1  2]`

b) `B = [1  2]`
       `[2  1]`

c) Qual(is) pode(m) usar Cholesky?

---

### Exercício 3.3: SVD (2×2)
Calcule SVD de:

```
A = [3  0]
    [0  2]
```

a) Calcule `AᵀA` e seus autovalores

b) Valores singulares σᵢ = √λᵢ

c) Encontre V (autovetores de AᵀA)

d) Calcule U via `uᵢ = (1/σᵢ)Avᵢ`

e) Monte `A = UΣVᵀ`

---

### Exercício 3.4: Rank via SVD
Matriz:

```
A = [1  2  3]
    [2  4  6]
```

a) Calcule valores singulares

b) Quantos são não-zero?

c) Qual o rank de A?

d) Como SVD revela rank?

---

## Desafios

### Desafio 1: Compressão de Imagem SVD
Matriz 4×4 representando imagem em escala de cinza:

```
I = [10  12  14  16]
    [12  14  16  18]
    [14  16  18  20]
    [16  18  20  22]
```

a) Calcule SVD: `I = UΣVᵀ`

b) Aproxime mantendo apenas k=2 maiores valores singulares

c) Calcule erro: `||I - I_aprox||`

d) Qual porcentagem da informação foi retida?

---

### Desafio 2: Pseudo-Inversa
Matriz não-quadrada:

```
A = [1  0]
    [0  1]
    [1  1]
```

a) Calcule SVD

b) Construa pseudo-inversa: `A⁺ = VΣ⁺Uᵀ`

   onde `Σ⁺` inverte valores singulares não-zero

c) Verifique propriedades:
   - `AA⁺A = A`
   - `A⁺AA⁺ = A⁺`

d) Use A⁺ para resolver `Ax = b` no sentido de mínimos quadrados

---

### Desafio 3: Sistema de Recomendação Simplificado
Matriz usuários × filmes (valores faltando = 0):

```
R = [5  3  0  1]
    [4  0  0  1]
    [1  1  0  5]
    [1  0  0  4]
```

a) Calcule SVD

b) Aproxime com rank k=2: `R ≈ UₖΣₖVₖᵀ`

c) "Preveja" valores faltantes

d) Qual a limitação dessa abordagem?

---

## 📝 Gabarito

<details>
<summary>Exercício 1.1</summary>

a) `L = [1  0]`, `U = [4  3]`
       `[1.5  1]`      `[0  -1.5]`

b) Multiplicação confirma ✓

c) Resolver `Ly=b` depois `Ux=y`: `x = [1, 2]ᵀ`

</details>

<details>
<summary>Exercício 1.2</summary>

a) Pivô zero causa falha

b) Trocar linhas: `P = [0 1; 1 0]`

c) `PA = [1 1; 0 1] = LU`

</details>

<details>
<summary>Exercício 1.3</summary>

Eliminação gaussiana produz:

`L = [1  0  0]`, `U = [2  1  1]`
    `[2  1  0]`      `[0  1  1]`
    `[4  3  1]`      `[0  0  2]`

</details>

<details>
<summary>Exercício 1.4</summary>

a/b) Resolver `Ly=b` e `Ux=y` para cada b

c) Reutiliza fatoração (economiza O(n³) por sistema extra)

</details>

<details>
<summary>Exercício 1.5</summary>

`det(L) = 1` (triangular inferior com 1s na diagonal)

`det(U) = 3*2 = 6`

`det(A) = 1*6 = 6` ✓

</details>

<details>
<summary>Exercício 2.1</summary>

a) `q₁ = [3/5, 4/5]ᵀ`

b) `proj = (3/5)q₁ = [9/25, 12/25]ᵀ`

c) `u₂ = [16/25, -12/25]ᵀ`, `q₂ = [4/5, -3/5]ᵀ`

d) `q₁·q₂ = 12/25 - 12/25 = 0` ✓

</details>

<details>
<summary>Exercício 2.2</summary>

a) `Q = [3/5  -4/5]`
       `[4/5   3/5]`

b) `R = [5  3/5]`
       `[0  4/5]`

c/d) Verificações confirmam ✓

</details>

<details>
<summary>Exercício 2.3</summary>

a) `c = Qᵀb`

b) Substituição reversa em sistema triangular

c) QR é mais estável numericamente

</details>

<details>
<summary>Exercício 2.4</summary>

a) `AᵀAx = Aᵀb` produz sistema 2×2

b) Solução: `a ≈ 1.33, b ≈ 0.5`

c) QR evita formar `AᵀA` (melhor condicionamento)

</details>

<details>
<summary>Exercício 2.5</summary>

a) Identidade trigonométrica: `cos²+sin² = 1`

b) `det(Q) = 1` (rotação preserva orientação e área)

c) Norma preservada por ortogonalidade

</details>

<details>
<summary>Exercício 3.1</summary>

a) `L = [2  0]`
       `[1  √2]`

b) `LLᵀ = A` ✓

c) Cholesky: O(n³/3), LU: O(2n³/3)

</details>

<details>
<summary>Exercício 3.2</summary>

a) Autovalores: 3, 1 → positiva definida ✓

b) Autovalores: 3, -1 → NÃO positiva definida ✗

c) Apenas A pode usar Cholesky

</details>

<details>
<summary>Exercício 3.3</summary>

a) `AᵀA = [9 0; 0 4]` → λ₁=9, λ₂=4

b) σ₁=3, σ₂=2

c/d/e) `U=V=I`, `Σ=[3 0; 0 2]`

</details>

<details>
<summary>Exercício 3.4</summary>

a/b) Apenas σ₁ ≠ 0

c) rank = 1

d) Número de valores singulares não-zero = rank

</details>

<details>
<summary>Desafio 1</summary>

a/b) SVD com k=2 maiores valores singulares

c) Erro pequeno (matriz quase rank-2)

d) ~95% da informação em 2 componentes (matriz muito estruturada)

</details>

<details>
<summary>Desafio 2</summary>

a/b) Pseudo-inversa construída via SVD

c) Propriedades verificadas

d) Solução de mínimos quadrados para sistemas sobre-determinados

</details>

<details>
<summary>Desafio 3</summary>

a/b) Aproximação rank-2 captura padrões principais

c) Valores faltantes preenchidos com aproximação

d) Assume padrões de baixo rank (nem sempre verdade)

</details>

---

## 🔗 Comparação de Decomposições

| Decomposição | Quando Usar | Vantagem | Custo |
|--------------|-------------|----------|-------|
| **LU** | Sistemas múltiplos (mesmo A) | Reutiliza fatoração | O(n³) |
| **Cholesky** | A simétrica positiva def. | Metade do custo de LU | O(n³/3) |
| **QR** | Mínimos quadrados | Estabilidade numérica | O(mn²) |
| **SVD** | **Qualquer matriz!** | Mais versátil | O(mn²) |

---

## 🎯 Próximos Passos

### Após completar estes exercícios:

1. ✅ **Implementar:** `k3-implementacao/codigo/i4-decomposicoes.md`
2. ✅ **Praticar código:** `k4-pratica/p3-avancados.md` (PCA, SVD, compressão)
3. ✅ **Projeto final:** `k5-projeto/` - Integrar todos os conceitos
4. 🎉 **Parabéns:** Você dominou Álgebra Linear!

---

**Total XP disponível:** 595 XP  
**Tempo total estimado:** 2h30-3h25  
**Dificuldade:** ⭐⭐⭐⭐⭐ (Muito Avançado)
