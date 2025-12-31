# Exercícios: Cônicas e Superfícies Quádricas

## 🎯 Meta
Dominar equações de cônicas, classificação e aplicações em trajetórias, órbitas e ray tracing.

## ⏱️ Tempo Estimado: ~2h30-3h20

## 💪 XP Total Disponível: 490 XP

## 📊 Progresso
- [ ] Nível 1 (0/6) - 60 XP
- [ ] Nível 2 (0/5) - 100 XP  
- [ ] Nível 3 (0/5) - 150 XP
- [ ] Desafios (0/3) - 180 XP

---

## Nível 1: Equações Básicas

### 1.1: Circunferência
Escreva a equação:
a) Centro $(3, -2)$, raio 5
b) Diâmetro de $(0,0)$ a $(6,8)$
c) Passa por $(1,2)$, $(5,2)$, $(3,4)$

<details><summary>Gabarito</summary>
a) $(x-3)^2 + (y+2)^2 = 25$
b) Centro: $(3,4)$, raio: $5$ → $(x-3)^2+(y-4)^2=25$
c) Sistema de 3 equações → $(x-3)^2+(y-2)^2=4$
</details>

### 1.2: Elipse
Identifique centro, $a$, $b$, focos:
a) $\frac{x^2}{25} + \frac{y^2}{9} = 1$
b) $\frac{(x-2)^2}{16} + \frac{(y+1)^2}{4} = 1$

<details><summary>Gabarito</summary>
a) Centro $(0,0)$, $a=5$, $b=3$, $c=4$, focos $(\pm 4, 0)$
b) Centro $(2,-1)$, $a=4$, $b=2$, $c=2\sqrt{3}$, focos $(2\pm 2\sqrt{3}, -1)$
</details>

### 1.3: Parábola
Encontre vértice e foco:
a) $y = x^2$
b) $x^2 = 8y$
c) $y = -2(x-3)^2 + 4$

<details><summary>Gabarito</summary>
a) $x^2 = 4py$ com $4p=1$ → $p=1/4$, vértice $(0,0)$, foco $(0,1/4)$
b) $4p=8$ → $p=2$, foco $(0,2)$
c) Vértice $(3,4)$, abre para baixo
</details>

### 1.4: Hipérbole
Para $\frac{x^2}{9} - \frac{y^2}{16} = 1$:
a) Vértices
b) Focos
c) Assíntotas
d) Excentricidade

<details><summary>Gabarito</summary>
a) $(\pm 3, 0)$
b) $c=\sqrt{9+16}=5$, focos $(\pm 5, 0)$
c) $y = \pm \frac{4}{3}x$
d) $e = 5/3$
</details>

### 1.5: Classificação
Classifique ($\Delta = B^2-4AC$):
a) $x^2 + y^2 - 4x + 6y = 3$
b) $x^2 - 4y = 0$
c) $4x^2 - 9y^2 = 36$

<details><summary>Gabarito</summary>
a) $A=C=1, B=0$ → Circunferência
b) $A=1, C=0$ → Parábola ($\Delta=0$)
c) $A=4, C=-9$ → Hipérbole ($\Delta>0$)
</details>

### 1.6: Esfera
Equação da esfera:
a) Centro $(1,2,3)$, raio 4
b) Diâmetro de $(0,0,0)$ a $(4,4,4)$

<details><summary>Gabarito</summary>
a) $(x-1)^2+(y-2)^2+(z-3)^2=16$
b) Centro $(2,2,2)$, raio $2\sqrt{3}$ → $(x-2)^2+(y-2)^2+(z-2)^2=12$
</details>

---

## Nível 2: Interseções e Conversões

### 2.1: Reta-Circunferência
Interseção de $y=x+1$ com $(x-2)^2+y^2=5$?

<details><summary>Gabarito</summary>
Substituir: $(x-2)^2+(x+1)^2=5$  
$2x^2-2x+5=5$ → $x(x-1)=0$  
Pontos: $(0,1)$ e $(1,2)$
</details>

### 2.2: Ray-Sphere
Raio $(0,0,0) + t(1,1,1)$ intersecta esfera $x^2+y^2+z^2=3$?

<details><summary>Gabarito</summary>
$(t)^2+(t)^2+(t)^2=3$ → $3t^2=3$ → $t=\pm 1$  
$t=1$: ponto $(1,1,1)$
</details>

### 2.3: Completar Quadrados
Reescreva na forma canônica:  
$x^2 + y^2 - 6x + 4y = 12$

<details><summary>Gabarito</summary>
$(x^2-6x+9) + (y^2+4y+4) = 12+9+4$  
$(x-3)^2 + (y+2)^2 = 25$ → Circunferência
</details>

### 2.4: Parametrização
Parametrize:
a) Elipse $\frac{x^2}{4}+\frac{y^2}{9}=1$
b) Hipérbole $x^2-y^2=1$ (ramo direito)

<details><summary>Gabarito</summary>
a) $x=2\cos t$, $y=3\sin t$
b) $x=\cosh t$, $y=\sinh t$ (ou $x=\sec t, y=\tan t$)
</details>

### 2.5: Tangente à Cônica
Encontre a reta tangente a $x^2+y^2=25$ no ponto $(3,4)$.

<details><summary>Gabarito</summary>
Gradiente (normal): $(2x, 2y) = (6, 8)$ em $(3,4)$  
Tangente perpendicular: diretor $(4, -3)$  
Equação: $3x + 4y = 25$
</details>

---

## Nível 3: Aplicações Práticas

### 3.1: Trajetória Balística
Projétil lançado de $(0,0)$ com $v_0=20$ m/s a $45°$, $g=10$ m/s².  
a) Equação da trajetória
b) Alcance máximo
c) Altura máxima

<details><summary>Gabarito</summary>
a) $y = x - \frac{gx^2}{2v_0^2\cos^2\theta} = x - \frac{x^2}{40}$  
b) $x_{max} = \frac{v_0^2\sin(2\theta)}{g} = 40$ m  
c) $y_{max} = \frac{v_0^2\sin^2\theta}{2g} = 10$ m
</details>

### 3.2: Órbita Elíptica
Satélite tem semi-eixos $a=8000$ km, $b=7000$ km. Terra no foco.  
a) Excentricidade
b) Distância mínima (perigeu)
c) Distância máxima (apogeu)

<details><summary>Gabarito</summary>
a) $c=\sqrt{64-49}\times 10^6 = \sqrt{15}\times 10^3$, $e=\sqrt{15}/8 \approx 0.48$  
b) $a-c \approx 4134$ km  
c) $a+c \approx 11866$ km
</details>

### 3.3: Antena Parabólica
Parábola $y=\frac{x^2}{16}$ (medidas em cm). Onde colocar receptor (foco)?

<details><summary>Gabarito</summary>
$x^2 = 16y = 4py$ → $p=4$ cm  
Foco em $(0, 4)$
</details>

### 3.4: Colisão Círculo-Círculo
Círculos se intersectam?  
$C_1: (0,0), r=5$  
$C_2: (6,8), r=3$

<details><summary>Gabarito</summary>
$d = \sqrt{36+64} = 10$  
$r_1+r_2 = 8 < 10$ → **Não se tocam**
</details>

### 3.5: Elipsoide Terrestre
Latitude $\phi=30°$, longitude $\lambda=0°$, altitude $h=0$. Coordenadas ECEF?  
(Use $a=6378$ km, $e^2=0.0067$)

<details><summary>Gabarito</summary>
$N = a/\sqrt{1-e^2\sin^2\phi} \approx 6382$ km  
$x = N\cos\phi\cos\lambda \approx 5527$ km  
$y = 0$  
$z = N(1-e^2)\sin\phi \approx 3165$ km
</details>

---

## Desafios

### Desafio 1: Ray Tracer com Elipsoide (60 XP)
Implemente interseção raio-elipsoide $\frac{x^2}{4}+\frac{y^2}{9}+\frac{z^2}{1}=1$.

### Desafio 2: Navegação LORAN (60 XP)
Duas estações em $(\pm 100, 0)$ km. Sinal de A chega 0.5ms antes de B (c=300 km/ms).  
Em qual hipérbole está o receptor?

### Desafio 3: Rotação de Cônica (60 XP)
Elimine termo $xy$ de $x^2 + 2xy + y^2 - 4 = 0$ por rotação.

---

**Última atualização:** 30 de dezembro de 2025  
**XP Total:** 490 XP
