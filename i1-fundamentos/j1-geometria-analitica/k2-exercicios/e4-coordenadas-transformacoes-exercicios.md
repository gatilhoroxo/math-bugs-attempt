# Exercícios: Sistemas de Coordenadas e Transformações

## 🎯 Meta
Dominar conversões entre sistemas de coordenadas e aplicar transformações geométricas 2D/3D.

## ⏱️ Tempo Estimado: ~2h20-3h10

## 💪 XP Total Disponível: 480 XP

## 📊 Progresso
- [ ] Nível 1 (0/6) - 60 XP
- [ ] Nível 2 (0/5) - 100 XP
- [ ] Nível 3 (0/5) - 150 XP
- [ ] Desafios (0/3) - 170 XP

---

## Nível 1: Conversões de Coordenadas

### 1.1: Cartesiana ↔ Polar
a) $(3, 3)$ → polar
b) $(r=5, \theta=60°)$ → cartesiana
c) $(-4, 0)$ → polar

<details><summary>Gabarito</summary>
a) $r=3\sqrt{2}$, $\theta=45°$  
b) $(2.5, 4.33)$  
c) $r=4$, $\theta=180°$
</details>

### 1.2: Cartesiana ↔ Esférica
a) $(0, 0, 5)$ → esférica
b) $(\rho=10, \theta=0°, \phi=90°)$ → cartesiana

<details><summary>Gabarito</summary>
a) $\rho=5$, $\theta$ indefinido, $\phi=0°$  
b) $(10, 0, 0)$
</details>

### 1.3: Cartesiana ↔ Cilíndrica
a) $(3, 4, 5)$ → cilíndrica
b) $(r=5, \theta=53°, z=2)$ → cartesiana

<details><summary>Gabarito</summary>
a) $r=5$, $\theta=53.13°$, $z=5$  
b) $\approx (3, 4, 2)$
</details>

### 1.4: Distância GPS (Haversine)
De São Paulo $(-23.55°, -46.63°)$ a Brasília $(-15.79°, -47.89°)$. Distância?

<details><summary>Gabarito</summary>
$\approx 875$ km
</details>

### 1.5: Bearing (Ângulo de Navegação)
De $(0°, 0°)$ para $(10°N, 10°E)$. Qual o bearing?

<details><summary>Gabarito</summary>
$\approx 45°$ (nordeste)
</details>

### 1.6: Coordenadas Geográficas
Ponto em lat $45°N$, lon $0°$, alt $0$. Coordenadas ECEF?

<details><summary>Gabarito</summary>
$x \approx 4517$ km, $y=0$, $z \approx 4487$ km
</details>

---

## Nível 2: Transformações 2D

### 2.1: Translação
Mova ponto $(3, 2)$ por vetor $(5, -1)$.

<details><summary>Gabarito</summary>
$(8, 1)$
</details>

### 2.2: Rotação ao Redor da Origem
Rotacione $(1, 0)$ por $90°$ anti-horário.

<details><summary>Gabarito</summary>
$(0, 1)$
</details>

### 2.3: Rotação ao Redor de Ponto
Rotacione $(4, 2)$ por $180°$ ao redor de $(2, 2)$.

<details><summary>Gabarito</summary>
Translação: $(2, 0)$ relativo ao centro  
Rotação: $(-2, 0)$  
De volta: $(0, 2)$
</details>

### 2.4: Escala
Escale triângulo $\{(0,0), (2,0), (0,3)\}$ por fator $(2, 1.5)$.

<details><summary>Gabarito</summary>
$\{(0,0), (4,0), (0,4.5)\}$
</details>

### 2.5: Reflexão
Reflita $(3, 4)$ sobre:
a) Eixo $x$
b) Eixo $y$
c) Linha $y=x$

<details><summary>Gabarito</summary>
a) $(3, -4)$  
b) $(-3, 4)$  
c) $(4, 3)$
</details>

---

## Nível 3: Transformações Compostas

### 3.1: Matrizes de Transformação
Escreva matriz 3×3 (homogênea) para:
a) Translação $(5, 3)$
b) Rotação $45°$
c) Escala $(2, 3)$

<details><summary>Gabarito</summary>
a) $\begin{bmatrix}1&0&5\\0&1&3\\0&0&1\end{bmatrix}$

b) $\begin{bmatrix}0.707&-0.707&0\\0.707&0.707&0\\0&0&1\end{bmatrix}$

c) $\begin{bmatrix}2&0&0\\0&3&0\\0&0&1\end{bmatrix}$
</details>

### 3.2: Composição (TRS)
Ponto $(1, 0)$: escale por 2, rode 90°, translate $(3, 0)$. Resultado?

<details><summary>Gabarito</summary>
Escala: $(2, 0)$  
Rotação: $(0, 2)$  
Translação: $(3, 2)$
</details>

### 3.3: Matriz Composta
Calcule $M = T(2,1) \cdot R(90°)$ e aplique em $(1, 0)$.

<details><summary>Gabarito</summary>
$M = \begin{bmatrix}0&-1&2\\1&0&1\\0&0&1\end{bmatrix}$

$M \begin{bmatrix}1\\0\\1\end{bmatrix} = \begin{bmatrix}2\\2\\1\end{bmatrix}$ → $(2, 2)$
</details>

### 3.4: Câmera 2D
Mundo tem objeto em $(5, 3)$. Câmera em $(2, 1)$ olhando para direita. Coordenadas de visualização?

<details><summary>Gabarito</summary>
Relativo à câmera: $(3, 2)$
</details>

### 3.5: Hierarquia de Transformações
Braço robótico: ombro em origem, cotovelo a 2m ($\theta_1=30°$), pulso a +1.5m ($\theta_2=45°$).  
Posição da mão?

<details><summary>Gabarito</summary>
Ombro: rotação $30°$  
Cotovelo: translação $(2, 0)$ + rotação $45°$  
Pulso: translação $(1.5, 0)$  
Composição: $\approx (2.8, 2.1)$
</details>

---

## Desafios

### Desafio 1: Mudança de Base (60 XP)
Crie matriz `look_at` 2D: câmera em $(5, 5)$ olhando para $(10, 10)$, up = $(0, 1)$.

### Desafio 2: Rotação 3D (Euler) (55 XP)
Aplique yaw=$90°$, pitch=$45°$, roll=$0°$ em vetor $(1, 0, 0)$.

### Desafio 3: Navegação Marítima (55 XP)
Barco em $(0°, 0°)$ navega bearing $45°$ por 100km. Nova posição lat/lon?

---

**Última atualização:** 30 de dezembro de 2025  
**XP Total:** 480 XP
