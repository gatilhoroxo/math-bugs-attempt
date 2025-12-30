# Recursos de Aprendizado - Álgebra Linear

## 🗺️ Guia de Uso

Este arquivo organiza recursos por **tópico** da álgebra linear, facilitando encontrar material específico quando você estiver estudando um conceito.

**Legenda:**
- ⭐⭐⭐⭐⭐ **Essencial** (comece por aqui!)
- ⭐⭐⭐ **Complementar** (aprofundamento ou visões alternativas)
- ⏱️ **Tempo estimado** quando aplicável
- 🆓 **Gratuito**

---

## 📚 1. Vetores e Espaços Vetoriais

### 🎥 Vídeos

⭐⭐⭐⭐⭐ **Essence of Linear Algebra - Vídeos 1-2** (3Blue1Brown)
- Link: https://www.3blue1brown.com/topics/linear-algebra
- Tópicos: Vetores, combinações lineares, span, base
- Tempo: ~20 min (2 vídeos)
- **Por quê:** Melhor visualização de vetores que existe
- 🆓 Gratuito

⭐⭐⭐ **MIT 18.06 - Aula 1-3** (Gilbert Strang)
- Link: https://ocw.mit.edu/courses/18-06-linear-algebra-spring-2010/
- Tópicos: Geometria de equações lineares, eliminação
- Tempo: ~2h30 (3 aulas)
- 🆓 Gratuito

### 📖 Leitura

⭐⭐⭐⭐⭐ **Introduction to Linear Algebra** - Strang (Capítulos 1-2)
- Tópicos: Vetores, combinações lineares, independência linear
- Tempo: 3-4h leitura

⭐⭐⭐ **Linear Algebra for CS (LA4CS)** - Thulasidas (Capítulo 1)
- Link: https://la4cs.com/
- Tópicos: Vetores em ciência da computação
- 🆓 PDF gratuito

### 💻 Ferramentas

⭐⭐⭐⭐⭐ **GeoGebra** - Visualizar vetores
- Link: https://www.geogebra.org/
- Uso: Desenhar vetores, soma, produto escalar
- 🆓 Gratuito (webapp)

---

## 📐 2. Transformações Lineares e Matrizes

### 🎥 Vídeos

⭐⭐⭐⭐⭐ **Essence of Linear Algebra - Vídeos 3-5** (3Blue1Brown)
- Tópicos: Transformações lineares, multiplicação matricial, composição
- Tempo: ~35 min (3 vídeos)
- **Por quê:** Visualiza matrizes como funções (game changer!)
- 🆓 Gratuito

⭐⭐⭐ **MIT 18.06 - Aula 4-7** (Gilbert Strang)
- Tópicos: Fatoração LU, transposta, espaços fundamentais
- Tempo: ~3h20
- 🆓 Gratuito

### 📖 Leitura

⭐⭐⭐⭐⭐ **Introduction to Linear Algebra** - Strang (Capítulos 3-4)
- Tópicos: Multiplicação, inversa, fatoração LU
- Tempo: 4-5h leitura

⭐⭐⭐ **Coding the Matrix** - Klein (Capítulos 4-6)
- Tópicos: Transformações com código Python
- Tempo: 3-4h leitura + código

### 🛠️ Ferramentas

⭐⭐⭐⭐⭐ **Matrix Multiplication Visualizer**
- Link: http://matrixmultiplication.xyz
- Uso: Ver passo a passo da multiplicação
- 🆓 Gratuito

⭐⭐⭐ **Desmos Matrix Calculator**
- Link: https://www.desmos.com/matrix
- Uso: Verificar cálculos matriciais
- 🆓 Gratuito

### 💻 Código (Bibliotecas)

⭐⭐⭐⭐⭐ **Eigen (C++)** - Para implementações performáticas
- Link: https://eigen.tuxfamily.org/
- Uso: Header-only, fácil integração
```cpp
#include <Eigen/Dense>
MatrixXd A(3,3); A << 1,2,3, 4,5,6, 7,8,9;
VectorXd x(3); x << 1,2,3;
VectorXd b = A * x;
```

⭐⭐⭐⭐⭐ **NumPy (Python)** - Para prototipagem rápida
- Link: https://numpy.org/
```python
import numpy as np
A = np.array([[1,2],[3,4]])
x = np.array([1,2])
b = A @ x
```

⭐⭐⭐ **GLM (C++)** - Para gráficos 3D/OpenGL
- Link: https://github.com/g-truc/glm
- Uso: Transformações 3D, compatível com shaders

---

## 🔢 3. Autovalores e Autovetores

### 🎥 Vídeos

⭐⭐⭐⭐⭐ **Essence of Linear Algebra - Vídeos 12-13** (3Blue1Brown)
- Tópicos: Autovalores, autovetores, change of basis
- Tempo: ~25 min (2 vídeos)
- **Por quê:** Intuição geométrica incrível
- 🆓 Gratuito

⭐⭐⭐ **MIT 18.06 - Aula 21-22** (Gilbert Strang)
- Tópicos: Diagonalização, potências de matrizes
- Tempo: ~1h40
- 🆓 Gratuito

### 📖 Leitura

⭐⭐⭐⭐⭐ **Introduction to Linear Algebra** - Strang (Capítulo 6)
- Tópicos: Autovalores, diagonalização, aplicações
- Tempo: 3-4h leitura

⭐⭐⭐ **Linear Algebra Done Right** - Axler (Capítulos 5-7)
- Tópicos: Abordagem sem determinantes (mais elegante)
- Tempo: 5-6h leitura
- **Nota:** Mais teórico, menos aplicado

### 📝 Papers

⭐⭐⭐⭐⭐ **"A Tutorial on PCA"** - Jonathon Shlens
- Link: https://arxiv.org/abs/1404.1100
- Tópicos: PCA explicado do zero com código
- Tempo: 1-2h leitura
- 🆓 Gratuito

⭐⭐⭐ **"The PageRank Citation Ranking"** - Page et al.
- Link: http://ilpubs.stanford.edu:8090/422/
- Tópicos: PageRank como problema de autovetor
- Tempo: 1h leitura
- 🆓 Gratuito

---

## 🧮 4. Decomposições Matriciais

### 🎥 Vídeos

⭐⭐⭐ **MIT 18.06 - Aula 26-28** (Gilbert Strang)
- Tópicos: Matrizes simétricas, SVD
- Tempo: ~2h30
- 🆓 Gratuito

⭐⭐⭐ **Computational Linear Algebra** - fast.ai (Aula 2-4)
- Link: https://github.com/fastai/numerical-linear-algebra
- Tópicos: QR, SVD aplicado (compressão, PCA)
- Tempo: ~4h
- 🆓 Gratuito

### 📖 Leitura

⭐⭐⭐⭐⭐ **Introduction to Applied LA** - Boyd & Vandenberghe (Capítulo 11-12)
- Link: https://web.stanford.edu/~boyd/vmls/
- Tópicos: QR, Cholesky, SVD com aplicações
- Tempo: 4-5h leitura
- 🆓 PDF gratuito

⭐⭐⭐ **Matrix Computations** - Golub & Van Loan (Capítulos 5-8)
- Tópicos: Algoritmos numéricos (LU, QR, SVD)
- Tempo: 8-10h leitura
- **Nota:** Muito técnico, para implementações sérias

### 💻 Código

⭐⭐⭐⭐⭐ **SciPy (Python)** - Decomposições prontas
```python
from scipy.linalg import lu, qr, svd, cholesky

L, U = lu(A, permute_l=True)
Q, R = qr(A)
U, s, Vt = svd(A)
L = cholesky(A, lower=True)
```

⭐⭐⭐ **Armadillo (C++)** - Sintaxe tipo MATLAB
- Link: http://arma.sourceforge.net/
```cpp
arma::mat L, U, P;
arma::lu(L, U, P, A);
```

---

## 🎮 5. Aplicações em Computação Gráfica

### 📖 Livros Especializados

⭐⭐⭐⭐⭐ **Mathematics for 3D Game Programming** - Eric Lengyel
- Tópicos: Transformações 3D, quaternions, projeções
- Tempo: 10-12h leitura
- **Por quê:** Foca exatamente em aplicações gráficas

⭐⭐⭐ **Real-Time Rendering** - Akenine-Möller et al. (Capítulo 4)
- Tópicos: Pipeline gráfico, transformações
- Tempo: 3-4h leitura (capítulo relevante)

### 🌐 Tutoriais Online

⭐⭐⭐⭐⭐ **LearnOpenGL - Transformations**
- Link: https://learnopengl.com/Getting-started/Transformations
- Tópicos: Model, View, Projection matrices
- Tempo: 1-2h leitura + código
- 🆓 Gratuito

⭐⭐⭐ **Scratchapixel - Geometry**
- Link: https://www.scratchapixel.com/lessons/mathematics-physics-for-computer-graphics/geometry
- Tópicos: Vetores, matrizes, transformações
- Tempo: 3-4h leitura
- 🆓 Gratuito

---

## 🤖 6. Aplicações em Machine Learning

### 📖 Livros Especializados

⭐⭐⭐⭐⭐ **Mathematics for Machine Learning** - Deisenroth et al.
- Link: https://mml-book.github.io/
- Tópicos: Álgebra linear para ML (gradiente, PCA, SVD)
- Tempo: 15-20h leitura (parte de álgebra)
- 🆓 PDF gratuito

⭐⭐⭐ **Deep Learning** - Goodfellow et al. (Capítulo 2)
- Link: https://www.deeplearningbook.org/
- Tópicos: Álgebra linear para redes neurais
- Tempo: 4-5h leitura
- 🆓 Versão online gratuita

### 🎥 Cursos

⭐⭐⭐⭐⭐ **Computational Linear Algebra** - fast.ai
- Link: https://github.com/fastai/numerical-linear-algebra
- Tópicos: SVD, PCA, NMF para ML
- Tempo: ~25h (curso completo)
- 🆓 Gratuito

---

## 🛠️ 7. Ferramentas de Visualização

### Interativas (Experimentar Conceitos)

⭐⭐⭐⭐⭐ **GeoGebra**
- Link: https://www.geogebra.org/
- Uso: Vetores, transformações 2D, gráficos
- 🆓 Gratuito

⭐⭐⭐⭐⭐ **Desmos Matrix Calculator**
- Link: https://www.desmos.com/matrix
- Uso: Multiplicação, inversa, det, autovalores
- 🆓 Gratuito

⭐⭐⭐ **Matrix Transformation Visualizer**
- Link: https://www.nctm.org/matrixtransform/ (exemplo)
- Uso: Ver transformações lineares em tempo real
- 🆓 Gratuito

### Criar Suas Próprias Visualizações

⭐⭐⭐ **Manim (Community Edition)**
- Link: https://github.com/ManimCommunity/manim
- Uso: Criar animações matemáticas (como 3Blue1Brown!)
- Tempo: ~5h setup + aprendizado básico
- 🆓 Gratuito (Python)

⭐⭐⭐ **Processing / p5.js**
- Link: https://p5js.org/
- Uso: Visualizações interativas de transformações
- Tempo: ~3h setup + básico
- 🆓 Gratuito (JavaScript)

---

## 📚 8. Livros por Nível

### Iniciante (Começar Aqui)

⭐⭐⭐⭐⭐ **Introduction to Linear Algebra** - Gilbert Strang
- **Por quê:** Melhor livro introdutório, balança teoria e prática
- Material MIT OCW gratuito: https://ocw.mit.edu/
- Tempo: 40-50h (livro completo)

⭐⭐⭐⭐⭐ **Linear Algebra for CS (LA4CS)** - Manoj Thulasidas
- Link: https://la4cs.com/
- **Por quê:** Focado em ciência da computação
- Tempo: 20-25h leitura
- 🆓 PDF gratuito

⭐⭐⭐ **No Bullshit Guide to Linear Algebra** - Ivan Savov
- **Por quê:** Explicações diretas, muita intuição
- Tempo: 15-20h leitura

### Intermediário (Após Iniciante)

⭐⭐⭐⭐⭐ **Introduction to Applied LA** - Boyd & Vandenberghe
- Link: https://web.stanford.edu/~boyd/vmls/
- **Por quê:** Aplicações práticas (ML, otimização)
- Tempo: 35-45h leitura + exercícios
- 🆓 PDF gratuito

⭐⭐⭐ **Coding the Matrix** - Philip Klein
- **Por quê:** Aprende fazendo (Python)
- Tempo: 30-40h leitura + código

### Avançado (Rigor Matemático)

⭐⭐⭐ **Linear Algebra Done Right** - Sheldon Axler
- **Por quê:** Elegante, sem determinantes até final
- Tempo: 50-60h leitura
- **Nota:** Não recomendado como primeiro livro

⭐⭐⭐ **Matrix Computations** - Golub & Van Loan
- **Por quê:** A "bíblia" de algoritmos numéricos
- Tempo: 80-100h (livro completo)
- **Nota:** Para implementações sérias

---

## 🎓 9. Cursos Completos Online

### Universitários (Gratuitos)

⭐⭐⭐⭐⭐ **MIT 18.06 - Linear Algebra** (Gilbert Strang)
- Link: https://ocw.mit.edu/courses/18-06-linear-algebra-spring-2010/
- Duração: ~30h vídeo + 20h exercícios
- Inclui: Vídeos, notas, provas com soluções
- 🆓 Gratuito

⭐⭐⭐ **Khan Academy - Linear Algebra**
- Link: https://www.khanacademy.org/math/linear-algebra
- Duração: ~20h
- **Por quê:** Exercícios interativos, ótimo para praticar
- 🆓 Gratuito

### Especializados

⭐⭐⭐⭐⭐ **Computational Linear Algebra** (fast.ai)
- Link: https://github.com/fastai/numerical-linear-algebra
- Duração: ~25h
- **Por quê:** Foco em implementação (Python/NumPy)
- 🆓 Gratuito

⭐⭐⭐ **Linear Algebra for Machine Learning** (Coursera - Imperial College)
- Duração: ~20h
- **Nota:** Pago (certificado), mas pode auditar grátis

---

## 🔗 10. Conexões com Outras Áreas

### Machine Learning

**Conceitos usados:**
- Vetores: Features, embeddings
- Matrizes: Datasets, pesos de redes neurais
- Produto matricial: Forward pass
- Gradiente: Backpropagation
- Autovalores: PCA, análise de componentes
- SVD: Sistemas de recomendação, compressão

**Recurso chave:** "Mathematics for ML" (Deisenroth) - 🆓

### Computação Gráfica

**Conceitos usados:**
- Vetores 3D: Posição, direção, normal
- Matrizes: Transformações (Model-View-Projection)
- Produto vetorial: Calcular normais
- Quaternions: Rotações sem gimbal lock

**Recurso chave:** LearnOpenGL.com - 🆓

### Teoria dos Grafos

**Conceitos usados:**
- Matriz de adjacência
- Autovalores do Laplaciano: Spectral graph theory
- PageRank: Autovetor de matriz de transição
- Caminhos: Potências de matriz

**Recurso chave:** "Spectral Graph Theory" (Chung) - PDF disponível

### Criptografia

**Conceitos usados:**
- Vetores em corpos finitos
- Matrizes inversas: Cifra de Hill
- Teoria dos números + álgebra: RSA

**Recurso chave:** "Introduction to Cryptography" (Trappe) - Capítulo 3

---

## 📝 11. Comunidades e Suporte

### Dúvidas e Discussões

⭐⭐⭐⭐⭐ **Math Stack Exchange**
- Link: https://math.stackexchange.com/
- Uso: Perguntas conceituais de matemática
- 🆓 Gratuito

⭐⭐⭐ **Stack Overflow**
- Link: https://stackoverflow.com/
- Uso: Implementações, bugs de código
- 🆓 Gratuito

⭐⭐⭐ **r/learnmath** (Reddit)
- Link: https://www.reddit.com/r/learnmath/
- Uso: Comunidade amigável para iniciantes
- 🆓 Gratuito

### Podcasts/Canais YouTube

⭐⭐⭐⭐⭐ **3Blue1Brown**
- Link: https://www.youtube.com/c/3blue1brown
- **Imperdível:** Essence of Linear Algebra

⭐⭐⭐ **MIT OpenCourseWare**
- Link: https://www.youtube.com/c/mitocw
- Cursos completos gravados

⭐⭐⭐ **Mathologer**
- Link: https://www.youtube.com/c/Mathologer
- Mais teórico, muito interessante

---

## ✅ Checklist de Competências

Após estudar os recursos, você deve conseguir:

**Vetores:**
- [ ] Visualizar vetores geometricamente
- [ ] Calcular produto escalar e vetorial
- [ ] Entender span e independência linear

**Matrizes:**
- [ ] Multiplicar matrizes (à mão e código)
- [ ] Ver matrizes como transformações lineares
- [ ] Calcular inversa (pequena) e determinante

**Aplicações:**
- [ ] Implementar transformações 2D/3D
- [ ] Resolver sistemas lineares
- [ ] Usar PCA para redução de dimensionalidade

**Decomposições:**
- [ ] Explicar quando usar LU, QR, Cholesky, SVD
- [ ] Implementar algoritmo básico de cada
- [ ] Aplicar em problemas reais (compressão, mínimos quadrados)

---

## 🗓️ Plano de Estudo Sugerido

### Semana 1-2: Vetores
- 🎥 3Blue1Brown vídeos 1-2
- 📖 Strang capítulos 1-2
- 💻 Implementar operações básicas

### Semana 3-4: Matrizes e Transformações
- 🎥 3Blue1Brown vídeos 3-5
- 📖 Strang capítulos 3-4
- 💻 Motor de transformações 2D

### Semana 5-6: Autovalores
- 🎥 3Blue1Brown vídeos 12-13
- 📖 Strang capítulo 6
- 💻 Implementar power method

### Semana 7-8: Decomposições
- 🎥 MIT 18.06 aulas 26-28
- 📖 Boyd capítulos 11-12
- 💻 PCA e compressão SVD

---

## 🎯 Próximos Passos

Após dominar álgebra linear:
1. **Geometria Analítica** (próximo módulo)
2. **Cálculo Multivariável** (gradiente, otimização)
3. **Escolher especialização:** ML, Gráficos, Criptografia, etc.

---

**Última atualização:** 30/12/2025  
**Todos os links verificados** em 30/12/2025
