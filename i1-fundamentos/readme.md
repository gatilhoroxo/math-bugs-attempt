# Math for CS - Matemática para Ciência da Computação

> **"A matemática é a linguagem com a qual Deus escreveu o universo."**  
> — Galileu Galilei

Este repositório documenta minha jornada de revisão e aprofundamento em matemática focada em Ciência da Computação, com ênfase em **aplicações práticas** e **projetos hands-on**.

---

## 🎯 Objetivo

Revisar e solidificar conceitos matemáticos que são essenciais para:
- 🤖 Machine Learning
- 🎮 Computação Gráfica
- 🔐 Criptografia
- 🤖 Robótica e Navegação
- 🧮 Análise de Algoritmos
- 📊 Data Science

**Filosofia:** Aprender fazendo. Cada tópico matemático tem um **projeto prático** que força a aplicação dos conceitos.

---

## 📚 Estrutura do Repositório

```
math-for-cs/
├── 01-algebra-linear/
│   ├── 01-contexto-conceitos.md
│   ├── 02-implementacao.md
│   ├── 03-exercicios.md
│   ├── 04-recursos-conexoes.md
│   └── projeto-transformacoes-2d/
│       └── (código do projeto)
│
├── 02-geometria-analitica/
│   ├── 01-contexto-conceitos.md
│   ├── 02-implementacao.md
│   ├── 03-exercicios.md
│   ├── 04-recursos-conexoes.md
│   └── projeto-ray-tracer/
│
├── 03-calculo/
│   ├── 01-contexto-conceitos.md
│   ├── ...
│   └── projeto-gradient-descent/
│
├── 04-estatistica/
│   ├── ...
│   └── projeto-classificador-bayesiano/
│
├── 05-matematica-discreta/
│   ├── ...
│   └── projeto-algoritmos-grafo/
│
└── README.md (você está aqui!)
```

---

## 🗺️ Roadmap de Estudos

### Status Atual: 🟢 Álgebra Linear

| Módulo | Status | Tempo Estimado | Projeto |
|--------|--------|----------------|---------|
| **Álgebra Linear** | 🟢 Em Progresso | 6-8 semanas | Engine de Transformações 2D |
| **Geometria Analítica** | ⚪ Planejado | 3-4 semanas | Ray Tracer Simples |
| **Cálculo** | ⚪ Planejado | 4-5 semanas | Gradient Descent Visualizado |
| **Estatística** | ⚪ Planejado | 4-5 semanas | Classificador Bayesiano |
| **Matemática Discreta** | ⚪ Planejado | 3-4 semanas | PageRank / Grafos |

**Legenda:**
- 🟢 Em Progresso
- ⚪ Planejado
- ✅ Completo

---

## 📖 Como Usar Este Repositório

### Para Cada Módulo:

1. **Leia 01-contexto-conceitos.md**
   - Entenda POR QUE o tópico importa
   - Veja ONDE é usado em CS
   - Obtenha intuição geométrica/visual

2. **Estude 02-implementacao.md**
   - Veja COMO implementar em código
   - Exemplos práticos em C/C++
   - Otimizações e bibliotecas

3. **Pratique com 03-exercicios.md**
   - Exercícios progressivos (fácil → difícil)
   - Problemas contextualizados em CS
   - Links para mais prática

4. **Consulte 04-recursos-conexoes.md**
   - Livros, vídeos, papers recomendados
   - Como se conecta com outros tópicos
   - Próximos passos

5. **Construa o projeto/**
   - Projeto prático que integra os conceitos
   - Instruções passo a passo
   - Extensões opcionais

### Tempo de Dedicação

Com ~2 horas por semana:
- **Leitura/estudo:** 1 hora
- **Implementação/projeto:** 1 hora

**Total por módulo:** 6-8 semanas

**Dica:** É melhor estudar consistentemente (2h/semana) do que em rajadas irregulares!

---

## 🛠️ Stack Tecnológica

### Linguagens Principais
- **C/C++**: Para projetos de performance e baixo nível
- **Python**: Para prototipagem rápida e ML

### Bibliotecas Usadas
- **Álgebra Linear**: Eigen (C++), NumPy (Python)
- **Gráficos**: SDL2, OpenGL
- **ML**: TensorFlow/PyTorch (futuramente)
- **Visualização**: Matplotlib, Manim

### Ferramentas
- **Build**: Make, CMake
- **Controle de versão**: Git
- **Documentação**: Markdown
- **Visualização**: GeoGebra, Desmos

---

## 🎓 Princípios de Aprendizado

### 1. **Implementar para Entender**
> "I hear and I forget. I see and I remember. I do and I understand." — Confúcio

Cada conceito matemático é implementado em código. Não basta ler sobre matrizes - você precisa multiplicá-las no seu próprio código.

### 2. **Visualizar é Compreender**
Use visualizações sempre que possível. Matemática abstrata fica concreta quando você VÊ o que está acontecendo.

### 3. **Conectar com Aplicações Reais**
Não estude "no vácuo". Todo tópico está ligado a aplicações práticas em CS:
- Álgebra Linear → ML, Gráficos 3D
- Cálculo → Otimização, Física
- Estatística → Data Science, ML
- Matemática Discreta → Algoritmos, Criptografia

### 4. **Projetos como Âncoras**
Cada módulo tem um projeto prático. Projetos forçam você a:
- Integrar múltiplos conceitos
- Enfrentar problemas reais
- Criar algo tangível
- Debugar e iterar

### 5. **Analogias e Intuição**
Use analogias do seu contexto (programação, One Piece, etc.) para tornar conceitos abstratos mais concretos.

---

## 📊 Progresso

### Álgebra Linear
- [x] Contexto e conceitos fundamentais
- [x] Implementação em C/C++
- [x] Exercícios estruturados
- [x] Recursos e conexões
- [ ] Projeto: Engine de Transformações 2D
  - [ ] Etapa 0: Setup
  - [ ] Etapa 1: Estruturas básicas
  - [ ] Etapa 2: Desenhar formas
  - [ ] Etapa 3: Transformações básicas
  - [ ] Etapa 4: Visualização em tempo real
  - [ ] Etapa 5: Composição
  - [ ] Etapa 6: Features avançadas

---

## 🎯 Metas de Longo Prazo

**Ao final deste repositório, eu serei capaz de:**

### Álgebra Linear
- [ ] Implementar qualquer transformação linear do zero
- [ ] Explicar geometricamente o que matrizes fazem
- [ ] Usar álgebra linear em ML (PCA, SVD, gradientes)
- [ ] Entender completamente autovalores/autovetores

### Cálculo
- [ ] Implementar gradient descent do zero
- [ ] Otimizar funções multivariáveis
- [ ] Entender backpropagation profundamente
- [ ] Aplicar cálculo em física/robótica

### Estatística
- [ ] Implementar classificadores do zero (Naive Bayes, etc.)
- [ ] Entender inferência estatística
- [ ] Fazer análise exploratória de dados
- [ ] Aplicar estatística em ML

### Matemática Discreta
- [ ] Implementar algoritmos de grafos eficientemente
- [ ] Analisar complexidade rigorosamente
- [ ] Entender combinatória e contagem
- [ ] Aplicar em criptografia

---

## 🔗 Conexões entre Tópicos

Este diagrama mostra como os tópicos se conectam:

```
    Álgebra Linear ←→ Geometria Analítica
           ↓                    ↓
        Cálculo  ←→  Estatística
           ↓                    ↓
      Otimização ←→  Machine Learning
           ↓                    ↓
    Matemática Discreta  ←→  Algoritmos
```

**Exemplo de integração:** 
- PageRank usa **grafos** (discreta) + **autovalores** (álgebra) + **probabilidade** (estatística)

---

## 📝 Como Contribuir (para mim mesmo no futuro)

### Ao adicionar um novo módulo:
1. Crie a estrutura de pastas
2. Escreva os 4 arquivos markdown
3. Implemente o projeto âncora
4. Faça pelo menos 3 exercícios
5. Documente lições aprendidas
6. Atualize este README

### Ao revisar:
- Adicione analogias melhores
- Melhore explicações confusas
- Adicione mais exemplos visuais
- Corrija erros

---

## 🌟 Recursos Favoritos

### Vídeos/Canais
1. **3Blue1Brown** - Visualizações incríveis de conceitos matemáticos
2. **MIT OpenCourseWare** - Cursos completos gratuitos
3. **Khan Academy** - Base sólida com exercícios

### Livros
1. **"Introduction to Linear Algebra"** - Gilbert Strang
2. **"Linear Algebra for CS"** - Manoj Thulasidas (GRÁTIS!)
3. **"Mathematics for Machine Learning"** - Deisenroth et al. (GRÁTIS!)

### Ferramentas
1. **GeoGebra** - Visualizar transformações
2. **Desmos** - Calculadora gráfica interativa
3. **Manim** - Criar animações matemáticas

---

## 💡 Lições Aprendidas

### O que funcionou bem:
- ✅ Implementar conceitos em código solidifica MUITO o entendimento
- ✅ Visualizações fazem conceitos abstratos ficarem concretos
- ✅ Projetos práticos motivam e integram conhecimento
- ✅ Exercícios progressivos (fácil → difícil) constroem confiança

### O que não funcionou:
- ❌ Tentar aprender muito de uma vez (sobrecarga cognitiva)
- ❌ Pular direto para exercícios sem entender conceitos
- ❌ Não revisar periodicamente (esquecimento)

### Ajustes para o futuro:
- 🔄 Revisar módulos anteriores mensalmente
- 🔄 Fazer mini-projetos integradores entre módulos
- 🔄 Criar resumos visuais (mind maps)

---

## 🎮 Gamificação (opcional)

Para tornar o estudo mais engajante:

### Sistema de XP:
- Completar arquivo de contexto: 10 XP
- Implementar estrutura básica: 20 XP
- Fazer 1 exercício: 5 XP
- Completar projeto básico: 50 XP
- Completar projeto com bônus: 100 XP

### Achievements:
- 🏆 **First Blood:** Primeiro conceito implementado
- 🏆 **Bug Hunter:** Encontrar e corrigir 10 bugs
- 🏆 **Architect:** Completar um projeto inteiro
- 🏆 **Speed Demon:** Completar módulo em < 5 semanas
- 🏆 **Perfectionist:** Fazer todos os exercícios de um módulo

---

## 📅 Timeline

- **Início:** [Data]
- **Meta:** Completar Álgebra Linear até [Data + 8 semanas]
- **Revisão 1:** [Data + 4 semanas]
- **Revisão 2:** [Data + 8 semanas]

---

## 🤝 Agradecimentos

### Inspirações:
- **3Blue1Brown** - Por tornar matemática linda
- **Gilbert Strang** - Por décadas de ensino excelente
- **Comunidade de CS/Math** - Por compartilhar conhecimento livremente

### Fontes:
- MIT OpenCourseWare
- Stanford CS231n
- Deep Learning Book (Goodfellow et al.)
- E todos os recursos listados em cada módulo

#### 📚 Recursos Gratuitos que você DEVE usar

Estes são totalmente gratuitos e de altíssima qualidade:

 - **3Blue1Brown** - Essence of Linear Algebra 
    - [YouTube](https://www.3blue1brown.com/topics/linear-algebra): As melhores visualizações que existem
 - **Linear Algebra for CS (LA4CS)**
    - [PDF](https://la4cs.com/): Livro completo grátis!
 - **MIT 18.06 - Curso completo do Gilbert Strang** 
    - [Link](https://ocw.mit.edu/): Vídeos + notas + exercícios
 - **Introduction to Applied Linear Algebra** - Boyd & Vandenberghe 
    - [PDF](https://web.stanford.edu/~boyd/vmls/): livro disponível

---

## 📬 Contato

Se você encontrou este repositório e quer conversar sobre matemática para CS:
- Abra uma issue para discussões
- Compartilhe seus próprios projetos
- Sugira melhorias

---

## 📜 Licença

Este repositório é para fins educacionais. Todo o código é de domínio público (Unlicense). Use, modifique e aprenda livremente!

---

**Última atualização:** [Data]  
**Status atual:** 🟢 Estudando Álgebra Linear  
**Próximo marco:** Completar projeto de Transformações 2D

---

> **Lembre-se:** O objetivo não é apenas "passar pela matéria", mas **dominar os conceitos** a ponto de poder aplicá-los confortavelmente em projetos reais. Qualidade > Quantidade. 🚀