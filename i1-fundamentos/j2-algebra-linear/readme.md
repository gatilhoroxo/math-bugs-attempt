# j2-algebra-linear - Álgebra Linear

> **"Álgebra Linear é a matemática dos dados."** — Gilbert Strang

---

## 📌 Navegação

- [Teoria](#teoria) - Conceitos fundamentais
- [Exercícios Matemática](#exercícios-matemática) - Matemática pura
- [Implementação](#implementação) - Código do zero
- [Exercícios Código](#exercícios-código) - Problemas de programação
- [Projeto Âncora](#projeto-âncora) - Engine de Transformações 2D/3D
- [Recursos](#recursos) - Links e materiais

---

## 🎯 Sobre Esta Matéria

Álgebra Linear é a **base nebulosa** que aparece em quase tudo em computação moderna. É a linguagem de:

- **Machine Learning**: Dados são vetores, modelos são matrizes
- **Computação Gráfica**: Toda transformação 3D é uma matriz
- **Robótica**: Posição e orientação de robôs
- **Compressão**: SVD é a base do JPEG
- **Redes Neurais**: Camadas de multiplicações matriciais

### Objetivos de Aprendizado

Ao completar esta matéria, você será capaz de:
- ✅ Implementar qualquer transformação linear do zero
- ✅ Explicar geometricamente o que matrizes fazem
- ✅ Usar álgebra linear em ML (PCA, SVD, gradientes)
- ✅ Entender completamente autovalores e autovetores
- ✅ Criar um engine básico de transformações 2D/3D

---

## 📚 Estrutura de Estudo

### 📖 k1-teoria/

Conteúdo teórico organizado por tópicos:

1. **[t1-vetores-espacos.md](k1-teoria/t1-vetores-espacos.md)**
   - Vetores como direção e magnitude
   - Espaços vetoriais e bases
   - Operações: soma, produto escalar, produto vetorial
   - Aplicações em CS

2. **[t2-transformacoes-lineares.md](k1-teoria/t2-transformacoes-lineares.md)**
   - Matrizes como transformações
   - Composição de transformações
   - Determinantes e inversas
   - Transformações em gráficos 2D/3D

3. **[t3-autovalores-autovetores.md](k1-teoria/t3-autovalores-autovetores.md)**
   - Direções que não mudam sob transformação
   - Cálculo e interpretação geométrica
   - Aplicações: PageRank, PCA

4. **[t4-decomposicoes.md](k1-teoria/t4-decomposicoes.md)**
   - LU, QR, SVD
   - Quando usar cada uma
   - Aplicações práticas

### ✏️ k2-exercicios/

Exercícios matemáticos puros com 3 níveis de dificuldade + desafios:

- **[e1-vetores-exercicios.md](k2-exercicios/e1-vetores-exercicios.md)** - Operações com vetores e espaços (460 XP)
- **[e2-transformacoes-exercicios.md](k2-exercicios/e2-transformacoes-exercicios.md)** - Matrizes e sistemas lineares (480 XP)
- **[e3-autovalores-exercicios.md](k2-exercicios/e3-autovalores-exercicios.md)** - Autovalores, autovetores e diagonalização (520 XP)
- **[e4-decomposicoes-exercicios.md](k2-exercicios/e4-decomposicoes-exercicios.md)** - LU, QR, Cholesky, SVD (595 XP)

### 💻 k3-implementacao/

Como implementar em código (C/C++ com pseudocódigo abstrato):

1. **[i1-vetores.md](k3-implementacao/i1-vetores.md)** - Estruturas e operações
2. **[i2-matrizes.md](k3-implementacao/i2-matrizes.md)** - Multiplicação e operações
3. **[i3-transformacoes.md](k3-implementacao/i3-transformacoes.md)** - Aplicações práticas
4. **[i4-decomposicoes.md](k3-implementacao/i4-decomposicoes.md)** - Algoritmos avançados

**Código-fonte:** [k3-implementacao/src/](k3-implementacao/src/)

### 🎯 k4-pratica/

Problemas de programação progressivos:

1. **[p1-basicos.md](k4-pratica/p1-basicos.md)** - Problemas básicos (3 problemas: biblioteca vetores, calculadora matricial, visualizador 2D - 300 XP)
2. **[p2-intermediarios.md](k4-pratica/p2-intermediarios.md)** - Problemas intermediários (3 problemas: physics engine, ray casting, collision detection - 360 XP)
3. **[p3-avancados.md](k4-pratica/p3-avancados.md)** - Desafios avançados (3 problemas: SVD, PCA, solvers iterativos - 400 XP)

**Soluções:** [k4-pratica/solucoes/](k4-pratica/solucoes/)

### 🚀 k5-projeto/

**[Projeto Âncora: Engine de Transformações 2D](k5-projeto/)** 

Projeto integrador que aplica TODOS os conceitos da matéria:
- Rotações, translações, escala
- Coordenadas homogêneas
- Visualização em tempo real
- Composição de transformações

### 📖 Recursos

**[recursos.md](recursos.md)** - Vídeos, livros e links específicos para Álgebra Linear

---

## 🔄 Fluxo de Estudo Recomendado

```
Para cada tópico (vetores, transformações, autovalores, decomposições):

1. Ler k1-teoria/tX-topico.md
2. Assistir vídeos recomendados em recursos.md
3. Resolver k2-exercicios/eX-topico-exercicios.md
4. Comparar com gabaritos (no próprio arquivo)
5. Ler k3-implementacao/codigo/iX-topico.md
6. Implementar você mesmo (pseudocódigo fornecido)
7. Resolver k4-pratica/pX-problemas.md
8. Aplicar no k5-projeto/
```

---

## 📊 Progresso

### Status Atual: 🟢 Estrutura Completa

- [ ] **Teoria (k1-teoria/)**
  - [ ] t1-vetores-espacos.md
  - [ ] t2-transformacoes-lineares.md
  - [ ] t3-autovalores-autovetores.md
  - [ ] t4-decomposicoes.md

- [ ] **Exercícios (k2-exercicios/)**
  - [ ] e1-vetores-exercicios.md (460 XP)
  - [ ] e2-transformacoes-exercicios.md (480 XP)
  - [ ] e3-autovalores-exercicios.md (520 XP)
  - [ ] e4-decomposicoes-exercicios.md (595 XP)

- [ ] **Implementação (k3-implementacao/codigo/)**
  - [ ] i1-vetores.md (50 XP - pseudocódigo português)
  - [ ] i2-matrizes.md (60 XP - pseudocódigo português)
  - [ ] i3-transformacoes.md (75 XP - pseudocódigo português)
  - [ ] i4-decomposicoes.md (90 XP - pseudocódigo português)

- [ ] **Prática (k4-pratica/)**
  - [ ] p1-basicos.md (300 XP - 3 problemas)
  - [ ] p2-intermediarios.md (360 XP - 3 problemas)
  - [ ] p3-avancados.md (400 XP - 3 problemas)

- [ ] **Projeto (k5-projeto/)**
  - [ ] Especificação completa (300 XP base)
  - [ ] README detalhado com XP system
  - [ ] Estrutura e controles definidos

---

## ⏱️ Tempo Estimado

**Duração:** 6-8 semanas (2-3h/semana)  
**Total:** 12-24 horas

---

## 🎓 Recursos Essenciais

### Vídeos Obrigatórios
- **3Blue1Brown - Essence of Linear Algebra** (série completa)
- **MIT 18.06 - Gilbert Strang** (aulas selecionadas)

### Livros
- *Introduction to Linear Algebra* - Gilbert Strang
- *Linear Algebra for CS* - Manoj Thulasidas

Veja [recursos.md](recursos.md) para lista completa.

---

## 🔗 Conexões

### Pré-requisitos
- Matemática básica do ensino médio
- Noções de programação (C/C++ ou Python)
- **j1-geometria-analitica** - Complementa com visão geométrica

### Próximos Passos
Após dominar Álgebra Linear, você estará pronto para:
- **j3-calculo** - Usa álgebra em otimização
- **i2-aplicacoes/j2-ml-basics** - Machine Learning do zero

---

**Última atualização:** 30 de dezembro de 2025  
**Status:** ✅ Conteúdo completo - Pronto para estudo  
**Total XP disponível:** 3.390 XP (Exercícios: 2.055 + Implementação: 275 + Prática: 1.060)

> "A beleza da álgebra linear está em ver o mundo através de transformações." — Grant Sanderson (3Blue1Brown)
