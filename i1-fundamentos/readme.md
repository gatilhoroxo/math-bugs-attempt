# i1-fundamentos - Fundamentos Matemáticos

> **"A matemática é a linguagem com a qual Deus escreveu o universo."** — Galileu Galilei

Este módulo contém os **6 pilares matemáticos** essenciais para ciência da computação. Cada matéria combina estudo teórico rigoroso com implementações práticas e projetos aplicados.

---

## 📌 Índice

- [Visão Geral](#visão-geral)
- [Estrutura de Cada Matéria](#estrutura-de-cada-matéria)
- [Metodologia de Estudo](#metodologia-de-estudo)
- [Roadmap e Progresso](#roadmap-e-progresso)
- [Como Estudar](#como-estudar)
- [Recursos Gerais](#recursos-gerais)

---

## 🎯 Visão Geral

### As 6 Matérias Fundamentais

| # | Matéria | Por que estudar? | Aplicações em CS |
|---|---------|------------------|------------------|
| **j1** | [Geometria Analítica](j1-geometria-analitica/) | Representações geométricas e espaciais | Navegação, Colisões, Renderização |
| **j2** | [Álgebra Linear](j2-algebra-linear/) | Base nebulosa que aparece em quase tudo | ML, Gráficos 3D, Robótica, Compressão |
| **j3** | [Cálculo](j3-calculo/) | Ferramentas de otimização e mudança | ML (gradientes), Física, Controle |
| **j4** | [Estatística](j4-estatistica/) | Análise de dados e incerteza | ML, Data Science, Testes A/B |
| **j5** | [Matemática Discreta](j5-matematica-discreta/) | Estruturas discretas e lógica | Algoritmos, Grafos, Criptografia |
| **j6** | [Física](j6-fisica/) | Modelagem de sistemas dinâmicos | Simulações, Jogos, Robótica |

### Conexões Entre as Matérias

```
Álgebra Linear ←→ Geometria Analítica
      ↓                    ↓
   Cálculo ←→ Estatística & Probabilidade
      ↓                    ↓
Matemática Discreta ←→ Física
      ↓                    ↓
         Aplicações em CS
```

---

## 📁 Estrutura de Cada Matéria

Todas as matérias seguem a **mesma estrutura padronizada**:

```
jX-nome-da-materia/
│
├── readme.md                        # Índice e visão geral da matéria
├── recursos.md                      # Links, vídeos, livros específicos
│
├── k1-teoria/                          # Conteúdo teórico
│   ├── t1-topico-1.md               # Contexto, intuição, definições
│   ├── t2-topico-2.md
│   └── ...
│
├── k2-exercicios/           # Matemática pura
│   ├── e1-topico-exercicios.md      # 4 níveis de dificuldade
│   ├── e1-topico-solucoes.md        # Soluções passo a passo
│   ├── e2-topico-exercicios.md
│   └── ...
│
├── k3-implementacao/                   # Código e implementações
│   ├── i1-topico.md                 # Como implementar
│   ├── i2-topico.md
│   ├── src/
│   └── tests/
│
├── k4-pratica/               # Problemas de programação
│   ├── p1-basicos.md
│   ├── p2-intermediarios.md
│   ├── p3-avancados.md
│   └── solucoes/
│
└── k5-projeto/                  # Projeto integrador
    ├── readme.md
    ├── especificacao.md
    ├── src/
    ├── tests/
    └── exemplos/
 
```

---

## 📚 Metodologia de Estudo

### Estrutura Detalhada de Cada Tipo de Arquivo

#### 📖 k1-teoria/tX-topico.md
Cada arquivo de teoria contém:

1. **Contexto e Motivação**
   - Por que este tópico importa?
   - Onde é usado em computação? (exemplos reais)
   
2. **Intuição Visual**
   - Diagramas e visualizações
   - Analogias do cotidiano
   - Explicações geométricas

3. **Definições Formais**
   - Teoria mínima necessária
   - Notação matemática padrão
   - Teoremas principais (sem provas longas)

4. **Exemplos Resolvidos**
   - 2-3 exemplos detalhados passo a passo
   - Diferentes níveis de complexidade

5. **Conexões**
   - Como se relaciona com outros tópicos
   - Aplicações práticas em CS

#### ✏️ k2-exercicios/

**Arquivos de exercícios** (eX-topico-exercicios.md):
- **Nível 1 - Básico**: Aplicação direta de definições (10-15 exercícios)
- **Nível 2 - Intermediário**: Combinação de conceitos (8-10 exercícios)
- **Nível 3 - Avançado**: Problemas desafiadores, provas (5-7 exercícios)
- **Nível 4 - Olimpíada/Competição**: Problemas muito difíceis (3-5 exercícios)

**Arquivos de soluções** (eX-topico-solucoes.md):
- Resposta completa passo a passo
- Explicação do raciocínio
- Métodos alternativos quando aplicável
- Dicas sobre erros comuns
- Insights e padrões importantes

#### 💻 k3-implementacao/iX-topico.md

Cada arquivo de implementação contém:

1. **Representação em Código**
   - Como modelar o conceito (structs, classes, tipos)
   - Design de API

2. **Implementação Comentada**
   - Código do zero, SEM usar bibliotecas externas
   - Explicação linha por linha das partes críticas

3. **Análise de Complexidade**
   - Big O de tempo e espaço
   - Trade-offs de cada operação

4. **Casos de Teste**
   - Exemplos práticos para validar
   - Edge cases importantes

5. **Comparação com Bibliotecas**
   - Como bibliotecas profissionais fazem (NumPy, Eigen, etc.)
   - Otimizações avançadas
   - Quando usar biblioteca vs implementação própria

6. **Armadilhas Comuns**
   - Bugs frequentes
   - Erros de precisão numérica
   - Como evitá-los

#### 🎯 k4-pratica/

**Problemas progressivos de programação:**

- **p1-basicos.md**: 5-10 problemas
  - Implementar operações simples
  - Testar compreensão básica
  
- **p2-intermediarios.md**: 5-8 problemas
  - Algoritmos que usam os conceitos
  - Combinação de múltiplas operações
  
- **p3-avancados.md**: 3-5 problemas
  - Otimizações avançadas
  - Casos complexos do mundo real
  
- **Links Externos**: LeetCode, Codeforces, Project Euler relacionados

#### 🚀 k5-projeto/

O projeto âncora integra TODOS os conceitos da matéria:

- **readme.md**: Visão geral, objetivos de aprendizado
- **especificacao.md**: 
  - O que implementar (requisitos funcionais)
  - Etapas do projeto (do básico ao avançado)
  - Critérios de sucesso
- **src/**: Código-fonte completo e documentado
- **tests/**: Testes automatizados para cada funcionalidade
- **exemplos/**: Visualizações, outputs, screenshots, demos

---

## 🔄 Fluxo de Estudo Recomendado

Para cada tópico dentro de uma matéria:

```
┌─────────────────────────────────────────┐
│ 1. Ler k1-teoria/tX-topico.md           │
│    ↓                                    │
│ 2. Assistir vídeos em recursos.md       │
│    ↓                                    │
│ 3. Resolver k2-exercicios/              │ ← MATEMÁTICA PURA
│    (começar do Nível 1)                 │
│    ↓                                    │
│ 4. Comparar com soluções                │
│    ↓                                    │
│ 5. Ler k2-implementacao/iX-topico.md    │
│    ↓                                    │
│ 6. Implementar você mesmo               │ ← CÓDIGO
│    ↓                                    │
│ 7. Resolver k4-pratica/                 │
│    ↓                                    │
│ 8. Trabalhar no k5-projeto/             │ ← INTEGRAÇÃO
└─────────────────────────────────────────┘
```

### ⏱️ Tempo Estimado por Matéria

Com dedicação de **2-3 horas por semana**:

| Matéria | Duração | Total de Horas |
|---------|---------|----------------|
| Geometria Analítica | 3-4 semanas | 6-12h |
| Álgebra Linear | 6-8 semanas | 12-24h |
| Cálculo | 6-8 semanas | 12-24h |
| Estatística | 4-6 semanas | 8-18h |
| Matemática Discreta | 5-7 semanas | 10-21h |
| Física | 3-5 semanas | 6-15h |
| **TOTAL** | **~30-40 semanas** | **~60-120h** |

---

## 🗺️ Roadmap e Progresso

### Status Atual: 🟢 Álgebra Linear - Planejamento

| Matéria | Status | Próximo Marco | Projeto |
|---------|--------|---------------|---------|
| **j1-geometria-analitica** | 🟡 Estruturando | - | Sistema Navegação |
| **j2-algebra-linear** | 🟡 Estruturando | Completar teoria de vetores | Engine 2D/3D |
| **j3-calculo** | ⚪ Planejado | - | Gradient Descent |
| **j4-estatistica** | ⚪ Planejado | - | Naive Bayes |
| **j5-matematica-discreta** | ⚪ Planejado | - | Grafos + SAT |
| **j6-fisica** | ⚪ Planejado | - | Motor Física 2D |

**Legenda:**
- 🟢 Em andamento ativo
- 🟡 Estruturando/Preparando
- ⚪ Planejado
- ✅ Completo

### Progresso Detalhado: Álgebra Linear

- [ ] **Teoria**
  - [ ] 01-vetores-espacos.md
  - [ ] 02-transformacoes-lineares.md
  - [ ] 03-autovalores-autovetores.md
  - [ ] 04-decomposicoes.md

- [ ] **Exercícios Matemática**
  - [ ] Vetores (exercícios + soluções)
  - [ ] Matrizes (exercícios + soluções)
  - [ ] Transformações (exercícios + soluções)
  - [ ] Autovalores (exercícios + soluções)

- [ ] **Implementação**
  - [ ] Vetores e operações
  - [ ] Matrizes e multiplicação
  - [ ] Transformações lineares
  - [ ] Decomposições

- [ ] **Projeto: Engine 2D/3D**
  - [ ] Setup básico
  - [ ] Estruturas de dados
  - [ ] Transformações básicas
  - [ ] Visualização em tempo real
  - [ ] Features avançadas

---

## 💡 Dicas de Estudo

### ✅ O que FAZER:

1. **Matemática antes de código**
   - Resolva exercícios matemáticos puros ANTES de programar
   - Entenda profundamente o conceito abstrato

2. **Visualize sempre**
   - Desenhe no papel
   - Use GeoGebra, Desmos
   - Crie visualizações simples

3. **Implemente do zero primeiro**
   - Só depois compare com NumPy/Eigen
   - "Sentir a dor" ajuda a apreciar otimizações

4. **Não pule exercícios**
   - Comece do Nível 1 mesmo parecendo fácil
   - Construa confiança progressivamente

5. **Use analogias**
   - Conecte com seu contexto (One Piece, jogos, etc.)
   - Crie suas próprias analogias

6. **Projetos são essenciais**
   - Não pule o projeto âncora
   - É onde tudo se integra

### ❌ O que EVITAR:

1. ~~Pular direto para o código~~
2. ~~Ignorar exercícios matemáticos puros~~
3. ~~Tentar aprender muito de uma vez~~
4. ~~Usar bibliotecas antes de implementar~~
5. ~~Não revisar periodicamente~~
6. ~~Pular visualizações~~

---

## 📖 Recursos Gerais para Fundamentos

### 🎥 Vídeos Essenciais

1. **3Blue1Brown** - Essence of Linear Algebra, Calculus
   - As MELHORES visualizações que existem
   - Assista ANTES de estudar cada tópico

2. **MIT OpenCourseWare**
   - 18.06 (Álgebra Linear - Gilbert Strang)
   - 18.01 (Cálculo)
   - 6.042 (Matemática Discreta)

3. **Khan Academy**
   - Base sólida para revisar conceitos
   - Muitos exercícios práticos

### 📚 Livros (Todos Gratuitos!)

**Álgebra Linear:**
- *Introduction to Linear Algebra* - Gilbert Strang
- *Linear Algebra for CS* - Manoj Thulasidas ([PDF](https://la4cs.com/))
- *Introduction to Applied Linear Algebra* - Boyd & Vandenberghe ([PDF](https://web.stanford.edu/~boyd/vmls/))

**Cálculo:**
- *Calculus* - Gilbert Strang (MIT OCW)
- *Single Variable Calculus* - MIT OCW

**Estatística:**
- *Introduction to Probability* - Blitzstein & Hwang
- *Think Stats* - Allen Downey

**Matemática Discreta:**
- *Mathematics for Computer Science* - Lehman, Leighton, Meyer (MIT)

**Geral:**
- *Mathematics for Machine Learning* - Deisenroth et al. ([PDF](https://mml-book.github.io/))

### 🛠️ Ferramentas

- **Visualização**: GeoGebra, Desmos, Manim
- **Computação**: Python, NumPy, Matplotlib
- **Build**: Make, CMake (para projetos C/C++)
- **Edição**: VS Code, Jupyter Notebooks

---

## 🎮 Sistema de Acompanhamento (Opcional)

### XP por Atividade:
- Completar arquivo de teoria: **10 XP**
- Resolver conjunto de exercícios (todos os níveis): **30 XP**
- Implementar estrutura de dados/algoritmo: **20 XP**
- Completar mini-projeto de exercícios: **50 XP**
- Completar projeto âncora: **100 XP**

### Achievements:
- 🏆 **First Steps**: Primeiro arquivo de teoria lido
- 🏆 **Mathematician**: 100 exercícios matemáticos resolvidos
- 🏆 **Coder**: Primeira implementação do zero
- 🏆 **Architect**: Projeto âncora completo
- 🏆 **Master**: Matéria completamente dominada (teoria + exercícios + projeto)

### Meta Total de XP: ~1000 XP (todas as 6 matérias)

---

## 🔗 Conexões e Próximos Passos

### Depois de Completar i1-fundamentos:

1. **i2-aplicacoes/** - Aplicar fundamentos em domínios específicos
   - Algoritmos avançados
   - ML do zero
   - Computação gráfica
   - Criptografia

2. **i3-projetos/** - Projetos integradores complexos
   - Mini ML Framework
   - Ray Tracer
   - Sistema de navegação marítima
   - E mais...

---

## 📅 Timeline Sugerida

**Ordem Recomendada:**

1. **Geometria Analítica** (3-4 semanas)
   - Começa aqui - base para quase tudo

2. **Álgebra Linear** (6-8 semanas)
   - Complementa geometria analítica

3. **Cálculo** (6-8 semanas)
   - Usa conceitos de álgebra

4. **Estatística** (4-6 semanas)
   - Usa cálculo e álgebra

5. **Matemática Discreta** (5-7 semanas)
   - Pode ser feita em paralelo com outras

6. **Física** (3-5 semanas)
   - Integra cálculo e álgebra

**Total estimado: 30-40 semanas (~8-10 meses com 2-3h/semana)**

---

**Última atualização:** 31 de dezembro de 2025  
**Status:** 🟡 Estruturando Álgebra Linear  
**Próximo marco:** Completar primeiro arquivo de teoria

> **Lembre-se:** Consistência > Intensidade. Melhor estudar 2h/semana todo semana do que 10h em um fim de semana e depois parar. 🚀
