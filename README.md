# math-bugs-attempt
Learning about math in cs. 

## Mapa de estudos

 - **Álgebra Linear** → é a base para Machine Learning, Computação Gráfica, Robótica (transformações, sistemas de coordenadas)
 - **Geometria Analítica** → 
 - **Cálculo** → conecta com ML (otimização, gradientes), Robótica (cinemática, controle), Gráficos (curvas, superfícies)
 - **Estatística & Probabilidade** → essencial para ML, Análise de Algoritmos (complexidade esperada), Criptografia
 - **Matemática Discreta & Lógica** → Algoritmos, Estruturas de Dados, Criptografia, Análise de Complexidade

Estrutura do repositório

``` 
math-for-cs/
├── i1-fundamentos/
│   ├── j1-algebra-linear/
│   │   ├── 01-contexto-conceitos.md
│   │   ├── 02-implementacao.md
│   │   ├── 03-exercicios.md
│   │   ├── 04-recursos-conexoes.md
│   │   └── projeto-transformacoes-2d/
│   ├── j2-geometria-analitica/
│   │   └──
│   ├── j3-calculo/
│   │   └──
│   ├── j4-estatistica/
│   │   └──
│   └── j5-matematica-discreta/
│       └──
├── i2-aplicacoes/
│   ├── j1-algoritmos/
│   │   └──
│   ├── j2-ml-basics/
│   │   └──
│   ├── j3-computacao-grafica/
│   │   └──
│   └── j4-criptografia/
│       └──
├── i3-projetos/
│   └── j1-stuff/
└── recursos.md
```

## 💡 Dicas Importantes

 1. **Não pule o 3Blue1Brown**: Esses vídeos vão transformar sua compreensão visual. Assista ANTES de mergulhar fundo nos exercícios.
 2. **Implemente tudo do zero primeiro**: Só depois compare com bibliotecas como Eigen. Você precisa "sentir a dor" para apreciar as otimizações.
 3. **Use analogias**: Quando algo não fizer sentido, tente criar sua própria analogia (One Piece, jogos, navegação, etc.).
 4. **Visualize sempre**: Desenhe no papel, use GeoGebra, veja geometricamente o que está acontecendo.
 5. **Não se preocupe em fazer TUDO**: Se um exercício não te interessa, pule. O importante é entender os conceitos fundamentais.

## Plano de Estudos

### Fase 1: Algebra Linear

É a base mais nebulosa e aparece em quase tudo que vai ser feito aqui. 

**Conceitos-chave para dominar:**
 - Vetores e espaços vetorias
 - Matrizes como transformações lineares
 - Autovalores e autovetores
 - Decomposições

#### Projeto âncora: Implementar um mini-engine de transformações 2D/3D
 - Rotações, translações, escala
 - Projeções (ortográfica e perspectiva)
 - Visualizar como matrizes transformam objetos
 - Analogia One Piece: Imagina implementar as transformações do Gear 4 do Luffy - cada matriz é um tipo de transformação que muda a "forma" dos vetores!


### Fase 2: Geometria Analítica

### Fase 3: Cálculo Aplicado

Entender derivadas e integrais como ferramentas, não só a mecânica de resolver. 

**Conceitos-chave:**
 - Derivadas como taxas de variação e aproximações locais
 - Gradiente e direção de máxima variação
 - Integrais como acumulação
 - Otimização (gradiente descendente)

#### Projeto âncora: Implementar Gradient Descent do zero
 - Visualizar como a derivada "aponta" a direção
 - Aplicar em problemas simples
 - Ver como o learning rate afeta a convergência


### Fase 4: Estatística & Probabilidade 

Com álgebra linear e cálculo, é possível entender ML de verdade. 

**Conceitos-chave:**
 - Probabilidade básica e distribuições
 - Estatística descritiva (média, variância, covariância)
 - Inferência e testes de hipótese
 - Teorema de Bayes

#### Projeto âncora: Sistema de recomendação simples ou classificador bayesiano
 - Implementar Naive Bayes do zero
 - Aplicar em dados reais (ex: classificar texto, spam detection)

### Fase 5: Matemática Discreta Avançada 

**Conceitos-chave:**
 - Teoria dos grafos (BFS, DFS, caminhos mínimos)
 - Combinatória e contagem
 - Relações de recorrência
 - Teoria dos números básica (para criptografia)

#### Projeto âncora: Implementar algoritmos de grafo + análise de Complexidade
 - Visualizar grafos
 - Implementar Dijstra, A*
 - Análise rigorosa de complexidade

### Fase 6: Projetos Integradores
Junta:
1. **Mini ML Framework**: Implementar rede neural do zero (álgebra + cálculo + estatística)
2. **Render 2D básico**: Ray tracing simples (álgebra = geometria)
3. **Implementação de RSA**: Criptografia de chave pública (teoria dos números + discreta)
4. **Simulador de robô**: Cinemática e controle (álgebra + cálculo)

## Estrutura de cada .md de estudo

Para cada tópico:

``` md
# [Tópico]

## Contexto e Motivação
- Por que isso importa?
- Onde é usado em CS?

## Conceitos Fundamentais
- Teoria mínima necessária
- Analogias e intuição
```

``` md
## Implementação
- Como codificar isso?
- Exemplos práticos
```

``` md
## Exercícios
- Problemas progressivos
- Links para prática
```

``` md
## Recursos
- Referências (livros, vídeos, papers)
- Links úteis

## Conexões
- Como se conecta com outros tópicos?

```

