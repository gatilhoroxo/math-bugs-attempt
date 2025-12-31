# Exercícios: Distâncias e Ângulos

## 🎯 Meta

Dominar cálculos de distâncias (ponto-reta, ponto-plano, reta-reta) e ângulos (vetores, retas, planos) com aplicações práticas.

---

## ⏱️ Tempo Estimado: ~2h-2h40

---

## 💪 Sistema de XP

**XP Total Disponível:** 470 XP

---

## 📊 Rastreamento de Progresso

- [ ] Nível 1 completo (0/5 exercícios) - 50 XP
- [ ] Nível 2 completo (0/5 exercícios) - 100 XP
- [ ] Nível 3 completo (0/5 exercícios) - 150 XP
- [ ] Desafios completos (0/3 exercícios) - 170 XP

**XP Conquistado:** ___ / 470 XP

---

## Nível 1: Distâncias Básicas

### Exercício 1.1: Distância Ponto-Reta (2D)
Calcule a distância do ponto à reta:

a) $P = (3, 4)$ e reta $3x - 4y + 5 = 0$
b) $P = (1, 1)$ e reta que passa por $(0, 0)$ e $(2, 2)$
c) $P = (-2, 5)$ e reta $y = 2x + 1$

<details>
<summary>💡 Gabarito</summary>

a) $d = \frac{|3(3) - 4(4) + 5|}{\sqrt{3^2+4^2}} = \frac{|9-16+5|}{5} = \frac{0}{5} = 0$ → Ponto **na reta**!

b) Equação da reta: $y = x$ → $x - y = 0$  
   $d = \frac{|1-1|}{\sqrt{2}} = 0$ → Ponto **na reta**!

c) $2x - y + 1 = 0$  
   $d = \frac{|2(-2) - 5 + 1|}{\sqrt{5}} = \frac{8}{\sqrt{5}} = \frac{8\sqrt{5}}{5}$

</details>

---

### Exercício 1.2: Distância Ponto-Plano
Calcule a distância:

a) $P = (1, 2, 3)$ ao plano $x + y + z = 0$
b) $P = (0, 0, 5)$ ao plano $2x - y + 2z = 6$
c) $P = (1, 1, 1)$ ao plano $xy$ (z=0)

<details>
<summary>💡 Gabarito</summary>

a) $d = \frac{|1+2+3|}{\sqrt{3}} = \frac{6}{\sqrt{3}} = 2\sqrt{3}$

b) $d = \frac{|0-0+10-6|}{\sqrt{4+1+4}} = \frac{4}{3}$

c) Plano $z=0$: distância = $|1-0| = 1$

</details>

---

### Exercício 1.3: Ângulos Entre Vetores
Calcule o ângulo:

a) $\vec{u} = (1, 0)$ e $\vec{v} = (1, 1)$
b) $\vec{u} = (3, 4)$ e $\vec{v} = (-4, 3)$
c) $\vec{u} = (1, 2, 2)$ e $\vec{v} = (2, 1, -2)$

<details>
<summary>💡 Gabarito</summary>

a) $\cos\theta = \frac{1}{\sqrt{2}} \Rightarrow \theta = 45°$

b) $\vec{u}\cdot\vec{v} = -12+12 = 0 \Rightarrow \theta = 90°$ (perpendiculares)

c) $\vec{u}\cdot\vec{v} = 2+2-4 = 0 \Rightarrow \theta = 90°$

</details>

---

### Exercício 1.4: Ângulos Entre Retas
Encontre o ângulo agudo:

a) $r_1: y = 2x$ e $r_2: y = -\frac{1}{2}x$
b) $r_1: (0,0) + t(1,1)$ e $r_2: (0,0) + s(1,-1)$

<details>
<summary>💡 Gabarito</summary>

a) Diretores: $(1,2)$ e $(1,-1/2) = (2,-1)$  
   $\cos\theta = \frac{|2-2|}{\sqrt{5}\sqrt{5}} = 0 \Rightarrow \theta = 90°$

b) $(1,1) \cdot (1,-1) = 0 \Rightarrow \theta = 90°$

</details>

---

### Exercício 1.5: Projeção Escalar
Calcule a projeção escalar de $\vec{a}$ sobre $\vec{b}$:

a) $\vec{a} = (3, 4)$, $\vec{b} = (1, 0)$
b) $\vec{a} = (2, 3)$, $\vec{b} = (4, 4)$
c) $\vec{a} = (1, 2, 2)$, $\vec{b} = (1, 0, 0)$

<details>
<summary>💡 Gabarito</summary>

a) $\text{comp}_{\vec{b}}\vec{a} = \frac{\vec{a}\cdot\vec{b}}{\|\vec{b}\|} = \frac{3}{1} = 3$

b) $= \frac{8+12}{\sqrt{32}} = \frac{20}{4\sqrt{2}} = \frac{5\sqrt{2}}{2}$

c) $= \frac{1}{1} = 1$

</details>

---

## Nível 2: Projeções e Aplicações

### Exercício 2.1: Projeção Vetorial
Calcule $\text{proj}_{\vec{b}}\vec{a}$:

a) $\vec{a} = (5, 12)$, $\vec{b} = (3, 4)$
b) $\vec{a} = (1, 1, 1)$, $\vec{b} = (1, 0, 0)$

<details>
<summary>💡 Gabarito</summary>

a) $\text{proj} = \frac{15+48}{25}(3,4) = \frac{63}{25}(3,4) = (7.56, 10.08)$

b) $= 1 \cdot (1,0,0) = (1, 0, 0)$

</details>

---

### Exercício 2.2: Decomposição de Vetor
Decomponha $\vec{a} = (6, 8)$ em componentes paralela e perpendicular a $\vec{b} = (1, 0)$.

<details>
<summary>💡 Gabarito</summary>

Paralela: $\text{proj}_{\vec{b}}\vec{a} = (6, 0)$  
Perpendicular: $\vec{a} - \text{proj} = (0, 8)$

</details>

---

### Exercício 2.3: Distância Reta-Reta (3D)
Calcule a distância entre:
$$r_1: (2,0,0) + t(1,0,0)$$
$$r_2: (0,1,1) + s(0,1,0)$$

<details>
<summary>💡 Gabarito</summary>

$\vec{v_1} = (1,0,0)$, $\vec{v_2} = (0,1,0)$, $\vec{P_1P_2} = (-2,1,1)$

$\vec{v_1} \times \vec{v_2} = (0,0,1)$

$d = \frac{|(-2,1,1)\cdot(0,0,1)|}{1} = |1| = 1$

</details>

---

### Exercício 2.4: Campo de Visão (FOV)
Um observador em $(0,0)$ olha na direção $(1,0)$ com FOV de 60°. O ponto $(2, 1)$ está visível?

<details>
<summary>💡 Gabarito</summary>

Direção ao alvo: $(2,1)$ normalizado = $(2/\sqrt{5}, 1/\sqrt{5})$

$\cos\theta = (1,0) \cdot (2/\sqrt{5}, 1/\sqrt{5}) = 2/\sqrt{5}$

$\theta = \arccos(2/\sqrt{5}) \approx 26.6°$

$26.6° < 30°$ (metade de 60°) → **Visível!**

</details>

---

### Exercício 2.5: Iluminação Difusa
Uma superfície tem normal $\vec{n} = (0, 1, 0)$. Luz vem de $\vec{L} = (1, 1, 0)$ (normalizado). Calcule intensidade.

<details>
<summary>💡 Gabarito</summary>

$\hat{L} = (1/\sqrt{2}, 1/\sqrt{2}, 0)$

Intensidade $= \max(0, \vec{n} \cdot \hat{L}) = 1/\sqrt{2} \approx 0.707$

</details>

---

## Nível 3: Problemas Avançados

### Exercício 3.1: Ponto Mais Próximo na Reta
Encontre o ponto da reta $r: (1,2) + t(3,-1)$ mais próximo de $P=(4,5)$.

<details>
<summary>💡 Gabarito</summary>

Vetor $\vec{QP}$ onde $Q=(1,2)$: $(3,3)$  
Projeção: $t^* = \frac{(3,3)\cdot(3,-1)}{10} = \frac{6}{10} = 0.6$  
Ponto: $(1,2) + 0.6(3,-1) = (2.8, 1.4)$

</details>

---

### Exercício 3.2: Reflexão de Vetor
Reflita $\vec{v} = (4, 3)$ sobre a reta $y = 0$ (eixo x).

<details>
<summary>💡 Gabarito</summary>

Normal: $\vec{n} = (0, 1)$

Reflexão: $\vec{v} - 2(\vec{v}\cdot\vec{n})\vec{n} = (4,3) - 6(0,1) = (4, -3)$

</details>

---

### Exercício 3.3: Navegação GPS
Calcule distância e bearing de São Paulo $(-23.55°, -46.63°)$ a Rio $(-22.91°, -43.17°)$.

<details>
<summary>💡 Gabarito</summary>

Use fórmula Haversine: $d \approx 365$ km

Bearing: $\approx 55°$ (nordeste)

</details>

---

### Exercício 3.4: Sombra Projetada
Uma luz em $(0, 10)$ ilumina um poste em $(5, 0)$ de altura 3. Onde termina a sombra no chão ($y=0$)?

<details>
<summary>💡 Gabarito</summary>

Topo do poste: $(5, 3)$  
Raio de luz: $(0,10) + t[(5,3)-(0,10)] = (0,10) + t(5,-7)$

Intersecção com $y=0$: $10-7t=0 \Rightarrow t = 10/7$

$x = 5t = 50/7 \approx 7.14$

</details>

---

### Exercício 3.5: Colisão Esfera-Plano
Esfera centrada em $(1, 2, 3)$ com raio 2. Ela intersecta o plano $x+y+z=5$?

<details>
<summary>💡 Gabarito</summary>

Distância centro ao plano: $\frac{|1+2+3-5|}{\sqrt{3}} = \frac{1}{\sqrt{3}} \approx 0.577$

$0.577 < 2$ → **Sim, intersecta!**

</details>

---

## Desafios

### Desafio 1: Distância Mínima Ponto-Segmento (60 XP)
Encontre a distância de $P=(3,4)$ ao segmento de $(0,0)$ a $(6,0)$.

### Desafio 2: Ângulo Diedro (55 XP)
Calcule o ângulo entre os planos $x+y+z=1$ e $2x-y=0$.

### Desafio 3: Trajetória Otimizada (55 XP)
Um barco em $(0,0)$ quer alcançar $(10,10)$. Corrente empurra com vetor $(-1,2)$. Qual direção remar?

---

**Última atualização:** 30 de dezembro de 2025  
**Total de XP disponível:** 470 XP
