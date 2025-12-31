# j2-geometria-analitica - Geometria Analítica

> **"A geometria é a arte de pensar bem e desenhar mal."** — Henri Poincaré

---

## 📌 Navegação

- [Teoria](#teoria) - Conceitos fundamentais
- [Exercícios Matemática](#exercícios-matemática) - Matemática pura
- [Implementação](#implementação) - Código do zero
- [Exercícios Código](#exercícios-código) - Problemas de programação
- [Projeto Âncora](#projeto-âncora) - Sistema de Navegação + GPS
- [Recursos](#recursos) - Links e materiais

---

## 🎯 Sobre Esta Matéria

Geometria Analítica é a **ponte entre álgebra e geometria**. É a linguagem de:

- **Navegação**: GPS, sistemas de coordenadas, ortodromia
- **Gráficos 3D**: Ray tracing, colisões, frustum culling
- **Física de Jogos**: Trajetórias, detecção de colisões
- **Robótica**: Planejamento de trajetórias, cinemática
- **Cartografia**: Projeções de mapas, sistemas geodésicos

### Objetivos de Aprendizado

Ao completar esta matéria, você será capaz de:
- ✅ Representar e manipular pontos, retas e planos em 2D/3D
- ✅ Calcular distâncias, ângulos e projeções
- ✅ Trabalhar com cônicas e superfícies quádricas
- ✅ Converter entre sistemas de coordenadas (polar, esférica, GPS)
- ✅ Aplicar transformações geométricas em gráficos e robótica
- ✅ Implementar algoritmos de navegação e colisão

---

## 📚 Estrutura de Estudo

### 📖 k1-teoria/

Conteúdo teórico organizado por tópicos:

1. **[t1-pontos-retas-planos.md](k1-teoria/t1-pontos-retas-planos.md)**
   - Representações de retas (vetorial, paramétrica, geral)
   - Equações de planos
   - Interseções e posições relativas
   - Aplicações em ray casting e navegação

2. **[t2-distancias-angulos.md](k1-teoria/t2-distancias-angulos.md)**
   - Distâncias (ponto-reta, ponto-plano, reta-reta)
   - Ângulos entre vetores, retas e planos
   - Projeções vetoriais
   - Aplicações em física e iluminação 3D

3. **[t3-conicas-superficies.md](k1-teoria/t3-conicas-superficies.md)**
   - Cônicas (circunferência, elipse, parábola, hipérbole)
   - Superfícies quádricas (esfera, elipsoide, paraboloides)
   - Trajetórias balísticas e órbitas
   - Ray tracing com quádricas

4. **[t4-coordenadas-transformacoes.md](k1-teoria/t4-coordenadas-transformacoes.md)**
   - Sistemas de coordenadas (polar, cilíndrica, esférica, GPS)
   - Transformações 2D/3D (translação, rotação, escala)
   - Coordenadas homogêneas
   - Aplicações em câmeras e robótica

### ✏️ k2-exercicios/

Exercícios matemáticos puros com 3 níveis de dificuldade + desafios:

- **[e1-pontos-retas-planos-exercicios.md](k2-exercicios/e1-pontos-retas-planos-exercicios.md)** - Representações e interseções (450 XP)
- **[e2-distancias-angulos-exercicios.md](k2-exercicios/e2-distancias-angulos-exercicios.md)** - Métricas geométricas (470 XP)
- **[e3-conicas-superficies-exercicios.md](k2-exercicios/e3-conicas-superficies-exercicios.md)** - Cônicas e quádricas (490 XP)
- **[e4-coordenadas-transformacoes-exercicios.md](k2-exercicios/e4-coordenadas-transformacoes-exercicios.md)** - Conversões e transformações (480 XP)

### 💻 k3-implementacao/

Como implementar em código (C/C++ com pseudocódigo abstrato):

1. **[i1-pontos-retas-planos.md](k3-implementacao/i1-pontos-retas-planos.md)** - Estruturas e algoritmos de interseção
2. **[i2-distancias-angulos.md](k3-implementacao/i2-distancias-angulos.md)** - Funções de distância e ângulo
3. **[i3-conicas-superficies.md](k3-implementacao/i3-conicas-superficies.md)** - Ray tracing e cônicas
4. **[i4-coordenadas-transformacoes.md](k3-implementacao/i4-coordenadas-transformacoes.md)** - Conversões e matrizes

**Código-fonte:** [k3-implementacao/src/](k3-implementacao/src/)

### 🎯 k4-pratica/

Problemas de programação progressivos:

1. **[p1-basicos.md](k4-pratica/p1-basicos.md)** - Biblioteca geométrica 2D, colisões, GPS (280 XP)
2. **[p2-intermediarios.md](k4-pratica/p2-intermediarios.md)** - Ray tracer 2D, transformações, pathfinding (340 XP)
3. **[p3-avancados.md](k4-pratica/p3-avancados.md)** - Frustum culling, navegação marítima, robótica (380 XP)

**Soluções:** [k4-pratica/solucoes/](k4-pratica/solucoes/)

### 🚀 k5-projeto/

**[Projeto Âncora: Sistema de Navegação Marítima + GPS](k5-projeto/)** 

Projeto integrador que aplica TODOS os conceitos da matéria:
- Conversão de coordenadas GPS (lat/lon ↔ cartesianas)
- Cálculo de rotas ortodrômicas
- Detecção de colisões com obstáculos
- Visualização em mapa 2D com transformações
- Sistema de waypoints e pathfinding

### 📖 Recursos

**[recursos.md](recursos.md)** - Vídeos, livros e links específicos para Geometria Analítica

---

## 🔄 Fluxo de Estudo Recomendado

```
Para cada tópico (pontos/retas, distâncias, cônicas, coordenadas):

1. Ler k1-teoria/tX-topico.md
2. Assistir vídeos recomendados em recursos.md
3. Resolver k2-exercicios/eX-topico-exercicios.md
4. Comparar com gabaritos (no próprio arquivo)
5. Ler k3-implementacao/iX-topico.md
6. Implementar você mesmo (pseudocódigo fornecido)
7. Resolver k4-pratica/pX-problemas.md
8. Aplicar no k5-projeto/
```

---

## 📊 Progresso

### Status Atual: 🟢 Estrutura Completa

- [x] **Teoria (k1-teoria/)**
  - [x] t1-pontos-retas-planos.md
  - [x] t2-distancias-angulos.md
  - [x] t3-conicas-superficies.md
  - [x] t4-coordenadas-transformacoes.md

- [x] **Exercícios (k2-exercicios/)**
  - [x] e1-pontos-retas-planos-exercicios.md (450 XP)
  - [x] e2-distancias-angulos-exercicios.md (470 XP)
  - [x] e3-conicas-superficies-exercicios.md (490 XP)
  - [x] e4-coordenadas-transformacoes-exercicios.md (480 XP)

- [ ] **Implementação (k3-implementacao/)**
  - [ ] i1-pontos-retas-planos.md
  - [ ] i2-distancias-angulos.md
  - [ ] i3-conicas-superficies.md
  - [ ] i4-coordenadas-transformacoes.md

- [ ] **Prática (k4-pratica/)**
  - [ ] p1-basicos.md (280 XP)
  - [ ] p2-intermediarios.md (340 XP)
  - [ ] p3-avancados.md (380 XP)

- [ ] **Projeto (k5-projeto/)**
  - [ ] Especificação completa (350 XP base)
  - [ ] README detalhado com XP system
  - [ ] Estrutura e funcionalidades definidas

---

## ⏱️ Tempo Estimado

**Duração:** 5-7 semanas (2-3h/semana)  
**Total:** 10-21 horas

---

## 🎓 Recursos Essenciais

### Vídeos Obrigatórios
- **3Blue1Brown - Essence of Linear Algebra** (cap. sobre transformações)
- **Khan Academy - Analytic Geometry**

### Livros
- *Analytic Geometry* - Gordon Fuller
- *Geometry for Computer Graphics* - John Vince

Veja [recursos.md](recursos.md) para lista completa.

---

## 🔗 Conexões

### Pré-requisitos
- **j1-algebra-linear** (vetores, produto escalar/vetorial, matrizes)
- Trigonometria básica
- Sistemas lineares

### Próximos Passos
Após dominar Geometria Analítica, você estará pronto para:
- **j3-calculo** - Derivadas para otimização geométrica
- **i2-aplicacoes/j3-computacao-grafica** - Renderização 3D
- **i3-projetos/j2-ray-tracer** - Ray tracer completo

---

**Última atualização:** 30 de dezembro de 2025  
**Status:** ✅ Conteúdo completo - Pronto para estudo