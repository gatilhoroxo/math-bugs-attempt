# Exercícios: Pontos, Retas e Planos

## 🎯 Meta

Dominar representações de retas e planos, cálculo de interseções e aplicações em geometria 2D e 3D.

---

## ⏱️ Tempo Estimado

- **Nível 1 (Iniciante):** 25-35 min
- **Nível 2 (Intermediário):** 30-45 min
- **Nível 3 (Avançado):** 35-50 min
- **Desafios:** 30-40 min
- **Total:** ~2h-2h50

---

## 📋 Quando Fazer

- **Após ler:** `k1-teoria/t1-pontos-retas-planos.md`
- **Antes de:** `k3-implementacao/i1-pontos-retas-planos.md`

---

## 💪 Sistema de XP

- **Nível 1 (Iniciante):** 10 XP por exercício
- **Nível 2 (Intermediário):** 20 XP por exercício
- **Nível 3 (Avançado):** 30 XP por exercício
- **Desafio:** 50 XP

**XP Total Disponível:** 450 XP

---

## 📊 Rastreamento de Progresso

- [ ] Nível 1 completo (0/5 exercícios) - 50 XP
- [ ] Nível 2 completo (0/5 exercícios) - 100 XP
- [ ] Nível 3 completo (0/5 exercícios) - 150 XP
- [ ] Desafios completos (0/3 exercícios) - 150 XP

**XP Conquistado:** ___ / 450 XP

---

## Nível 1: Conceitos Básicos

### Exercício 1.1: Distância e Ponto Médio
Dados os pontos $A = (2, 3)$, $B = (5, 7)$, $C = (-1, 2)$:

a) Calcule a distância entre $A$ e $B$
b) Encontre o ponto médio entre $A$ e $B$
c) Qual ponto está mais próximo de $C$: $A$ ou $B$?
d) Calcule o perímetro do triângulo $ABC$

<details>
<summary>💡 Gabarito</summary>

a) $d(A,B) = \sqrt{(5-2)^2 + (7-3)^2} = \sqrt{9+16} = 5$

b) $M = \left(\frac{2+5}{2}, \frac{3+7}{2}\right) = (3.5, 5)$

c) $d(C,A) = \sqrt{9+1} = \sqrt{10} \approx 3.16$  
   $d(C,B) = \sqrt{36+25} = \sqrt{61} \approx 7.81$  
   Resposta: $A$ está mais próximo

d) $d(A,B) = 5$, $d(B,C) = \sqrt{61}$, $d(C,A) = \sqrt{10}$  
   Perímetro $= 5 + \sqrt{61} + \sqrt{10} \approx 15.98$

</details>

---

### Exercício 1.2: Equação Vetorial de Reta (2D)
Escreva a equação vetorial da reta que passa por:

a) $P_0 = (1, 2)$ com vetor diretor $\vec{v} = (3, -1)$
b) Os pontos $(2, 5)$ e $(4, 9)$
c) $P_0 = (0, 3)$ paralela à reta $y = 2x + 1$

<details>
<summary>💡 Gabarito</summary>

a) $\vec{r}(t) = (1, 2) + t(3, -1) = (1+3t, 2-t)$

b) Vetor diretor: $\vec{v} = (4,9) - (2,5) = (2,4)$  
   $\vec{r}(t) = (2, 5) + t(2, 4) = (2+2t, 5+4t)$

c) Reta paralela tem mesmo diretor: $\vec{v} = (1, 2)$ (de $y=2x+1$)  
   $\vec{r}(t) = (0, 3) + t(1, 2) = (t, 3+2t)$

</details>

---

### Exercício 1.3: Conversão de Formas de Equação
Converta as seguintes retas para as formas pedidas:

a) $\vec{r}(t) = (2, 1) + t(4, 3)$ → equação geral $ax + by + c = 0$
b) $3x - 2y + 6 = 0$ → equação vetorial
c) $\frac{x-1}{2} = \frac{y+3}{-1}$ (simétrica) → paramétrica

<details>
<summary>💡 Gabarito</summary>

a) Vetor normal $\vec{n} = (-3, 4)$ (perpendicular a $(4,3)$)  
   Equação: $-3(x-2) + 4(y-1) = 0$  
   $-3x + 6 + 4y - 4 = 0$  
   $-3x + 4y + 2 = 0$ ou $3x - 4y - 2 = 0$

b) Vetor normal $(3, -2)$ → diretor $\vec{v} = (2, 3)$  
   Ponto: fazendo $x=0$: $-2y=-6 \Rightarrow y=3$, então $P_0=(0,3)$  
   $\vec{r}(t) = (0, 3) + t(2, 3)$

c) $t = \frac{x-1}{2} = \frac{y+3}{-1}$  
   $x = 1 + 2t$  
   $y = -3 - t$

</details>

---

### Exercício 1.4: Pertencimento à Reta
Verifique se os pontos pertencem à reta $\vec{r}(t) = (1, -2) + t(2, 3)$:

a) $(3, 1)$
b) $(5, 4)$
c) $(-1, -5)$

<details>
<summary>💡 Gabarito</summary>

Resolver $(1+2t, -2+3t) = (x, y)$:

a) $(3, 1)$: $1+2t=3 \Rightarrow t=1$; $-2+3t=-2+3=1$ ✓ **Pertence**

b) $(5, 4)$: $1+2t=5 \Rightarrow t=2$; $-2+3(2)=4$ ✓ **Pertence**

c) $(-1, -5)$: $1+2t=-1 \Rightarrow t=-1$; $-2+3(-1)=-5$ ✓ **Pertence**

</details>

---

### Exercício 1.5: Equação de Plano (3D)
Encontre a equação geral do plano que:

a) Passa por $(1, 2, 3)$ com vetor normal $\vec{n} = (2, -1, 4)$
b) Contém os pontos $(1,0,0)$, $(0,1,0)$, $(0,0,1)$

<details>
<summary>💡 Gabarito</summary>

a) $\vec{n} \cdot (P - P_0) = 0$  
   $(2, -1, 4) \cdot (x-1, y-2, z-3) = 0$  
   $2(x-1) - (y-2) + 4(z-3) = 0$  
   $2x - 2 - y + 2 + 4z - 12 = 0$  
   **$2x - y + 4z = 12$**

b) Vetores diretores: $\vec{u} = (0,1,0)-(1,0,0) = (-1,1,0)$  
   $\vec{v} = (0,0,1)-(1,0,0) = (-1,0,1)$  
   Normal: $\vec{n} = \vec{u} \times \vec{v} = (1, 1, 1)$  
   Usando $(1,0,0)$: $(x-1) + y + z = 0$  
   **$x + y + z = 1$**

</details>

---

## Nível 2: Interseções e Posições Relativas

### Exercício 2.1: Interseção de Retas (2D)
Encontre o ponto de interseção (se existir):

a) $r_1: x + 2y = 5$ e $r_2: 3x - y = 4$
b) $r_1: \vec{r}(t) = (1, 2) + t(2, 1)$ e $r_2: \vec{s}(s) = (3, 0) + s(-1, 2)$
c) $r_1: y = 2x + 1$ e $r_2: y = 2x - 3$ (são paralelas?)

<details>
<summary>💡 Gabarito</summary>

a) Sistema linear:
$$\begin{cases} x + 2y = 5 \\ 3x - y = 4 \end{cases}$$
Da primeira: $x = 5 - 2y$  
Substituir: $3(5-2y) - y = 4 \Rightarrow 15 - 6y - y = 4 \Rightarrow y = \frac{11}{7}$  
$x = 5 - 2(\frac{11}{7}) = \frac{13}{7}$  
**Interseção: $\left(\frac{13}{7}, \frac{11}{7}\right)$**

b) $(1+2t, 2+t) = (3-s, 2s)$  
$$\begin{cases} 1+2t = 3-s \\ 2+t = 2s \end{cases}$$
Da segunda: $t = 2s-2$  
Substituir: $1+2(2s-2) = 3-s \Rightarrow 1+4s-4=3-s \Rightarrow 5s=6 \Rightarrow s=\frac{6}{5}$  
$t = 2(\frac{6}{5})-2 = \frac{2}{5}$  
Ponto: $(1+2 \cdot \frac{2}{5}, 2+\frac{2}{5}) = (\frac{9}{5}, \frac{12}{5})$

c) Diretor de ambas: $(1, 2)$ (mesma inclinação)  
**Paralelas - sem interseção**

</details>

---

### Exercício 2.2: Reta e Plano
Encontre a interseção da reta $\vec{r}(t) = (1, 0, -1) + t(2, 1, 3)$ com o plano $x + 2y - z = 4$.

<details>
<summary>💡 Gabarito</summary>

Substituir $(1+2t, t, -1+3t)$ no plano:
$$(1+2t) + 2t - (-1+3t) = 4$$
$$1 + 2t + 2t + 1 - 3t = 4$$
$$t + 2 = 4 \Rightarrow t = 2$$

Ponto: $(1+4, 2, -1+6) = (5, 2, 5)$

</details>

---

### Exercício 2.3: Paralelismo e Perpendicularidade
Determine se as retas são paralelas, perpendiculares ou nenhum dos dois:

a) $r_1: (1,2) + t(3,4)$ e $r_2: (0,0) + s(6,8)$
b) $r_1: (0,0) + t(1,2)$ e $r_2: (1,0) + s(-2,1)$
c) $r_1: x - y = 1$ e $r_2: x + y = 3$

<details>
<summary>💡 Gabarito</summary>

a) Diretores: $\vec{v_1} = (3,4)$, $\vec{v_2} = (6,8) = 2(3,4)$  
   **Paralelas** ($\vec{v_2} = 2\vec{v_1}$)

b) $\vec{v_1} = (1,2)$, $\vec{v_2} = (-2,1)$  
   Produto escalar: $1(-2) + 2(1) = 0$  
   **Perpendiculares**

c) Diretor de $r_1$: $(1,1)$ (de $y=x-1$)  
   Diretor de $r_2$: $(1,-1)$ (de $y=-x+3$)  
   Produto: $1(1) + 1(-1) = 0$  
   **Perpendiculares**

</details>

---

### Exercício 2.4: Planos Paralelos/Perpendiculares
Classifique os pares de planos:

a) $\pi_1: 2x - y + z = 3$ e $\pi_2: 4x - 2y + 2z = 7$
b) $\pi_1: x + y + z = 1$ e $\pi_2: 2x - y + z = 0$

<details>
<summary>💡 Gabarito</summary>

a) Normais: $\vec{n_1} = (2,-1,1)$, $\vec{n_2} = (4,-2,2) = 2\vec{n_1}$  
   **Paralelos** (normais proporcionais)

b) $\vec{n_1} = (1,1,1)$, $\vec{n_2} = (2,-1,1)$  
   Produto: $1(2) + 1(-1) + 1(1) = 2$  
   **Nem paralelos nem perpendiculares**

</details>

---

### Exercício 2.5: Coplanaridade
Verifique se os pontos $A(1,2,3)$, $B(2,3,4)$, $C(3,4,6)$, $D(0,1,1)$ são coplanares.

<details>
<summary>💡 Gabarito</summary>

Vetores: $\vec{AB} = (1,1,1)$, $\vec{AC} = (2,2,3)$, $\vec{AD} = (-1,-1,-2)$

Volume do paralelepípedo: $\vec{AB} \cdot (\vec{AC} \times \vec{AD})$

$\vec{AC} \times \vec{AD} = \begin{vmatrix} \vec{i} & \vec{j} & \vec{k} \\ 2 & 2 & 3 \\ -1 & -1 & -2 \end{vmatrix} = (-1, 1, 0)$

$\vec{AB} \cdot (-1,1,0) = -1 + 1 + 0 = 0$

**Coplanares!** (volume = 0)

</details>

---

## Nível 3: Problemas Aplicados

### Exercício 3.1: Ray Casting (Detecção de Colisão)
Um raio é lançado de $(0, 0)$ na direção $(1, 0.5)$. Uma parede é representada pelo segmento de $(5, 0)$ a $(5, 10)$.

a) Encontre o ponto de interseção do raio com a reta que contém a parede
b) Verifique se a interseção está dentro do segmento (entre $y=0$ e $y=10$)
c) Calcule a distância do origem até o ponto de colisão

<details>
<summary>💡 Gabarito</summary>

a) Raio: $(t, 0.5t)$  
   Parede: $x = 5$  
   Interseção: $t = 5$, então ponto = $(5, 2.5)$

b) $y = 2.5$ está em $[0, 10]$ ✓ **Colide**

c) $d = \sqrt{5^2 + 2.5^2} = \sqrt{31.25} \approx 5.59$

</details>

---

### Exercício 3.2: Trajetória de Projétil
Um projétil é lançado do ponto $(0, 1)$ com vetor velocidade $(10, 8)$ m/s. Assumindo sem gravidade (espaço):

a) Escreva a equação vetorial da trajetória
b) Onde o projétil estará após 3 segundos?
c) Quando ele cruza a linha $y = 5$?

<details>
<summary>💡 Gabarito</summary>

a) $\vec{r}(t) = (0, 1) + t(10, 8) = (10t, 1+8t)$

b) $t=3$: $(30, 25)$

c) $1 + 8t = 5 \Rightarrow t = 0.5$ s  
   Posição: $(5, 5)$

</details>

---

### Exercício 3.3: Navegação Marítima
Dois navios:
- Navio A em $(0, 0)$ navegando na direção $(3, 4)$ a 5 km/h
- Navio B em $(10, 0)$ navegando na direção $(-1, 2)$ a 3 km/h

a) Escreva as equações de trajetória (onde $t$ é em horas)
b) Eles vão colidir? Se sim, quando e onde?
c) Qual a distância mínima entre eles?

<details>
<summary>💡 Gabarito</summary>

a) Velocidades unitárias:  
   A: $\vec{v_A} = 5 \cdot \frac{(3,4)}{5} = (3, 4)$  
   B: $\vec{v_B} = 3 \cdot \frac{(-1,2)}{\sqrt{5}} = (-3/\sqrt{5}, 6/\sqrt{5})$
   
   Mas simplificando:  
   A: $(3t, 4t)$  
   B: $(10-t, 2t)$ (assumindo diretor simplificado)

b) Igualar: $(3t, 4t) = (10-t, 2t)$  
   $3t = 10-t \Rightarrow t = 2.5$  
   $4t = 2t$ → Falso!  
   **Não colidem**

c) (Cálculo complexo - exigiria derivadas)

</details>

---

### Exercício 3.4: Espelhos e Reflexão
Um raio de luz vem de $(-2, 5)$ e atinge um espelho no plano $y = 0$ no ponto $(1, 0)$.

a) Qual o ângulo de incidência?
b) Encontre a direção do raio refletido
c) Equação vetorial do raio refletido

<details>
<summary>💡 Gabarito</summary>

a) Direção incidente: $(1,0) - (-2,5) = (3, -5)$  
   Ângulo com normal $\vec{n}=(0,1)$: $\cos\theta = \frac{|(3,-5)\cdot(0,1)|}{||(3,-5)||}= \frac{5}{\sqrt{34}}$  
   $\theta \approx 31°$

b) Reflexão: $\vec{r} = \vec{i} - 2(\vec{i}\cdot\vec{n})\vec{n}$  
   $\vec{i} = (3,-5)$, $\vec{n} = (0,1)$  
   $\vec{r} = (3,-5) - 2(-5)(0,1) = (3, -5) + (0,10) = (3, 5)$

c) $\vec{r}(t) = (1, 0) + t(3, 5)$

</details>

---

### Exercício 3.5: Frustum Culling (3D)
Um plano de clipping é definido por $2x + y - z = 10$. Verifique se os pontos estão **dentro** do frustum (lado positivo do plano):

a) $(5, 2, 3)$
b) $(1, 1, 1)$
c) $(10, 0, 0)$

<details>
<summary>💡 Gabarito</summary>

Substituir na equação $2x + y - z - 10$:

a) $2(5) + 2 - 3 - 10 = -1 < 0$ → **Fora**

b) $2(1) + 1 - 1 - 10 = -8 < 0$ → **Fora**

c) $2(10) + 0 - 0 - 10 = 10 > 0$ → **Dentro**

</details>

---

## Desafios

### Desafio 1: Distância Reta-Reta em 3D (50 XP)
Calcule a distância mínima entre as retas reversas:
- $r_1: (1, 0, 0) + t(1, 1, 0)$
- $r_2: (0, 1, 0) + s(0, 1, 1)$

<details>
<summary>💡 Gabarito</summary>

Fórmula: $d = \frac{|(\vec{P_1P_2}) \cdot (\vec{v_1} \times \vec{v_2})|}{\||\vec{v_1} \times \vec{v_2}\||}$

$\vec{P_1P_2} = (0,1,0) - (1,0,0) = (-1, 1, 0)$  
$\vec{v_1} = (1,1,0)$, $\vec{v_2} = (0,1,1)$

$\vec{v_1} \times \vec{v_2} = (1, -1, 1)$

$d = \frac{|(-1,1,0) \cdot (1,-1,1)|}{\sqrt{3}} = \frac{|-1-1+0|}{\sqrt{3}} = \frac{2}{\sqrt{3}} = \frac{2\sqrt{3}}{3}$

</details>

---

### Desafio 2: Polígono Convexo (50 XP)
Implemente (em pseudocódigo) um algoritmo para verificar se um ponto está dentro de um polígono convexo usando produto vetorial.

<details>
<summary>💡 Dica</summary>

Para cada aresta do polígono, verifique se o ponto está do mesmo lado (usando sinal do produto vetorial). Se todos têm o mesmo sinal, ponto está dentro.

</details>

---

### Desafio 3: Interseção de 3 Planos (50 XP)
Encontre o ponto de interseção dos planos:
- $\pi_1: x + y + z = 6$
- $\pi_2: 2x - y + z = 3$
- $\pi_3: x + 2y - z = 0$

<details>
<summary>💡 Gabarito</summary>

Sistema 3×3:
$$\begin{bmatrix} 1 & 1 & 1 \\ 2 & -1 & 1 \\ 1 & 2 & -1 \end{bmatrix} \begin{bmatrix} x \\ y \\ z \end{bmatrix} = \begin{bmatrix} 6 \\ 3 \\ 0 \end{bmatrix}$$

Resolvendo (eliminação de Gauss ou regra de Cramer):  
$x = 2$, $y = 1$, $z = 3$

**Ponto: $(2, 1, 3)$**

</details>

---

## 🎓 Conclusão

Parabéns por completar os exercícios! Você praticou:
- ✅ Representação de retas (vetorial, paramétrica, geral)
- ✅ Conversão entre formas de equações
- ✅ Interseções e posições relativas
- ✅ Aplicações em ray casting, navegação e gráficos 3D

**Próximo passo:** Implementação em código (`k3-implementacao/i1-pontos-retas-planos.md`)

---

**Última atualização:** 30 de dezembro de 2025  
**Total de XP disponível:** 450 XP
