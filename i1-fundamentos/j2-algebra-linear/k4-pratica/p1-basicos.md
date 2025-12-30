# Problemas de Programação: Básicos

## 🎯 Meta

Implementar estruturas de dados e funções fundamentais de álgebra linear, criando ferramentas reutilizáveis para vetores e matrizes.

---

## ⏱️ Tempo Estimado

- **Problema 1 (Biblioteca Vetores):** 1h-1h30
- **Problema 2 (Calculadora Matricial):** 1h15-1h45
- **Problema 3 (Visualizador 2D):** 1h30-2h
- **Total:** ~4h-5h15

---

## 📋 Pré-requisitos

- **Teoria:** `k1-teoria/t1-vetores-espacos.md`, `t2-transformacoes-matrizes.md`
- **Exercícios:** `k2-exercicios/e1-vetores-exercicios.md`, `e2-transformacoes-exercicios.md` (Nível 1-2)
- **Implementação:** `k3-implementacao/codigo/i1-vetores.md`, `i2-matrizes.md` (leitura)
- **Linguagem:** C ou C++

---

## 🎚️ Dificuldade

⭐⭐⭐ Intermediário

---

## 💪 Sistema de XP

- **Problema 1:** 90 XP
- **Problema 2:** 100 XP  
- **Problema 3:** 110 XP

**XP Total Disponível:** 300 XP

---

## 📊 Rastreamento de Progresso

- [ ] Problema 1: Biblioteca de Vetores 3D (0/1) - 90 XP
- [ ] Problema 2: Calculadora Matricial CLI (0/1) - 100 XP
- [ ] Problema 3: Visualizador de Vetores 2D (0/1) - 110 XP

**XP Conquistado:** ___ / 300 XP

---

## Problema 1: Biblioteca de Vetores 3D

### 🎯 Objetivo

Criar uma biblioteca C completa para operações com vetores 3D, incluindo header file, implementação e programa de testes.

### 📐 Contexto Teórico

Vetores são entidades fundamentais com:
- **Magnitude:** Comprimento do vetor `||v|| = √(x² + y² + z²)`
- **Direção:** Representada pelo vetor normalizado `v̂ = v/||v||`
- **Operações:** Soma, escala, produto escalar, produto vetorial

**Aplicações:** Física (velocidade, força), gráficos 3D (posição, normal), IA (direção movimento)

### 🛠️ Especificação

**Estrutura de dados:**
```c
typedef struct {
    double x, y, z;
} Vec3;
```

**Funcionalidades obrigatórias:**

**Construtores:**
- `Vec3 vec3_create(double x, double y, double z)`
- `Vec3 vec3_zero(void)` - Retorna (0, 0, 0)
- `Vec3 vec3_one(void)` - Retorna (1, 1, 1)

**Operações aritméticas:**
- `Vec3 vec3_add(Vec3 a, Vec3 b)` - Soma
- `Vec3 vec3_sub(Vec3 a, Vec3 b)` - Subtração
- `Vec3 vec3_scale(Vec3 v, double k)` - Multiplicação por escalar
- `Vec3 vec3_negate(Vec3 v)` - Negação (-v)

**Métricas:**
- `double vec3_length(Vec3 v)` - Magnitude
- `double vec3_length_squared(Vec3 v)` - Quadrado da magnitude (mais rápido)
- `double vec3_distance(Vec3 a, Vec3 b)` - Distância entre dois pontos

**Normalização:**
- `Vec3 vec3_normalize(Vec3 v)` - Vetor unitário (retornar zero se magnitude = 0)

**Produtos:**
- `double vec3_dot(Vec3 a, Vec3 b)` - Produto escalar
- `Vec3 vec3_cross(Vec3 a, Vec3 b)` - Produto vetorial

**Interpolação:**
- `Vec3 vec3_lerp(Vec3 a, Vec3 b, double t)` - Interpolação linear (t ∈ [0,1])

**Utilidades:**
- `void vec3_print(Vec3 v)` - Imprimir no formato "(x, y, z)"
- `int vec3_equals(Vec3 a, Vec3 b, double epsilon)` - Comparação com tolerância

### 📝 Pseudocódigo

```
ESTRUTURA Vec3:
    x, y, z: números reais

FUNÇÃO vec3_add(a, b):
    RETORNAR Vec3(a.x + b.x, a.y + b.y, a.z + b.z)

FUNÇÃO vec3_length(v):
    RETORNAR raiz_quadrada(v.x² + v.y² + v.z²)

FUNÇÃO vec3_normalize(v):
    len = vec3_length(v)
    SE len < EPSILON:
        RETORNAR vec3_zero()
    RETORNAR vec3_scale(v, 1.0 / len)

FUNÇÃO vec3_dot(a, b):
    RETORNAR a.x*b.x + a.y*b.y + a.z*b.z

FUNÇÃO vec3_cross(a, b):
    RETORNAR Vec3(
        a.y*b.z - a.z*b.y,
        a.z*b.x - a.x*b.z,
        a.x*b.y - a.y*b.x
    )

FUNÇÃO vec3_lerp(a, b, t):
    """Interpola entre a (t=0) e b (t=1)"""
    RETORNAR vec3_add(a, vec3_scale(vec3_sub(b, a), t))
```

### 🧪 Testes

Crie `test_vec3.c` que verifica:

```c
// Teste 1: Soma
Vec3 a = vec3_create(1, 2, 3);
Vec3 b = vec3_create(4, 5, 6);
Vec3 c = vec3_add(a, b);
assert(vec3_equals(c, vec3_create(5, 7, 9), 1e-10));

// Teste 2: Magnitude
Vec3 v = vec3_create(3, 4, 0);
assert(fabs(vec3_length(v) - 5.0) < 1e-10);

// Teste 3: Normalização
Vec3 v_norm = vec3_normalize(v);
assert(fabs(vec3_length(v_norm) - 1.0) < 1e-10);

// Teste 4: Produto escalar (perpendiculares)
Vec3 x = vec3_create(1, 0, 0);
Vec3 y = vec3_create(0, 1, 0);
assert(fabs(vec3_dot(x, y)) < 1e-10);

// Teste 5: Produto vetorial
Vec3 z = vec3_cross(x, y);
assert(vec3_equals(z, vec3_create(0, 0, 1), 1e-10));

// Teste 6: Interpolação
Vec3 start = vec3_create(0, 0, 0);
Vec3 end = vec3_create(10, 10, 10);
Vec3 mid = vec3_lerp(start, end, 0.5);
assert(vec3_equals(mid, vec3_create(5, 5, 5), 1e-10));
```

### 💡 Insights

- **length_squared:** Use quando possível evitar √ (caro computacionalmente)
- **EPSILON:** Defina como `1e-10` para comparações de floats
- **cross product:** Ordem importa! `a × b = -(b × a)`
- **lerp:** Útil para animações suaves

### 🎯 Desafios Extras (+30 XP cada)

1. **Reflexão:** `Vec3 vec3_reflect(Vec3 incident, Vec3 normal)` - Vetor refletido em superfície
2. **Projeção:** `Vec3 vec3_project(Vec3 v, Vec3 onto)` - Projetar v sobre onto
3. **Ângulo:** `double vec3_angle(Vec3 a, Vec3 b)` - Ângulo em radianos
4. **Sobrecarga C++:** Versão com operadores `+, -, *, ==` sobrecarregados

---

## Problema 2: Calculadora Matricial CLI

### 🎯 Objetivo

Criar programa de linha de comando para operações com matrizes 3×3, lendo dados de arquivo ou entrada interativa.

### 📐 Contexto Teórico

Matrizes representam transformações lineares:
- **Multiplicação:** `(AB)ij = Σk Aik * Bkj` (O(n³))
- **Transposta:** `(Aᵀ)ij = Aji`
- **Determinante:** Medida de "volume" da transformação
- **Inversa:** Transformação reversa (se det ≠ 0)

### 🛠️ Especificação

**Estrutura:**
```c
typedef struct {
    double m[3][3];
} Mat3;
```

**Funcionalidades:**

**Entrada/Saída:**
- Ler matriz de arquivo texto (9 números separados por espaço/linha)
- Imprimir matriz formatada
- Modo interativo (usuário digita valores)

**Operações:**
- Soma de matrizes
- Multiplicação matriz × matriz
- Multiplicação matriz × vetor
- Transposta
- Determinante (3×3)
- Inversa (3×3, método da adjunta)
- Matriz identidade
- Matriz zero

**Interface CLI:**
```bash
$ ./matcalc
Calculadora Matricial 3×3
Opções:
  1. Carregar matriz A de arquivo
  2. Carregar matriz B de arquivo
  3. A + B
  4. A * B
  5. Transposta de A
  6. Determinante de A
  7. Inversa de A
  8. Imprimir A
  9. Sair
Escolha: _
```

### 📝 Pseudocódigo

```
FUNÇÃO mat3_mul(A, B):
    """Multiplica matrizes 3×3"""
    resultado = matriz_zero()
    
    PARA i DE 0 ATÉ 2:
        PARA j DE 0 ATÉ 2:
            soma = 0
            PARA k DE 0 ATÉ 2:
                soma += A[i][k] * B[k][j]
            resultado[i][j] = soma
    
    RETORNAR resultado

FUNÇÃO mat3_det(M):
    """Determinante 3×3 (Regra de Sarrus)"""
    a = M[0][0], b = M[0][1], c = M[0][2]
    d = M[1][0], e = M[1][1], f = M[1][2]
    g = M[2][0], h = M[2][1], i = M[2][2]
    
    positivo = a*e*i + b*f*g + c*d*h
    negativo = c*e*g + b*d*i + a*f*h
    
    RETORNAR positivo - negativo

FUNÇÃO mat3_inverse(M):
    """Inversa via matriz de cofatores"""
    det = mat3_det(M)
    
    SE abs(det) < EPSILON:
        ERRO "Matriz singular (não invertível)"
    
    // Calcular cofatores (9 determinantes 2×2)
    cofatores = matriz_zero()
    
    PARA i DE 0 ATÉ 2:
        PARA j DE 0 ATÉ 2:
            submatriz = extrair_submatriz_2x2(M, i, j)
            cofator = determinante_2x2(submatriz)
            
            // Sinal alternado
            SE (i + j) % 2 == 1:
                cofator = -cofator
            
            cofatores[i][j] = cofator
    
    // Transpor cofatores (adjunta)
    adjunta = transpor(cofatores)
    
    // Dividir por determinante
    RETORNAR escalar_multiplicar(adjunta, 1.0 / det)

FUNÇÃO carregar_matriz_arquivo(caminho):
    arquivo = abrir(caminho, "r")
    matriz = matriz_zero()
    
    PARA i DE 0 ATÉ 2:
        PARA j DE 0 ATÉ 2:
            ler(arquivo, &matriz[i][j])
    
    fechar(arquivo)
    RETORNAR matriz
```

### 🧪 Testes

**Arquivo `test_identity.txt`:**
```
1 0 0
0 1 0
0 0 1
```

**Arquivo `test_rotation.txt`:** (rotação 90° em Z)
```
0 -1 0
1  0 0
0  0 1
```

**Casos de teste:**
```c
// Teste 1: Identidade
Mat3 I = mat3_identity();
Mat3 A = carregar("test_rotation.txt");
Mat3 result = mat3_mul(I, A);
assert(mat3_equals(result, A));

// Teste 2: Determinante de rotação = 1
assert(fabs(mat3_det(A) - 1.0) < 1e-10);

// Teste 3: Inversa
Mat3 A_inv = mat3_inverse(A);
Mat3 produto = mat3_mul(A, A_inv);
assert(mat3_is_identity(produto, 1e-8));

// Teste 4: Transposta dupla
Mat3 At = mat3_transpose(A);
Mat3 Att = mat3_transpose(At);
assert(mat3_equals(A, Att));
```

### 💡 Insights

- **Arquivo vs interativo:** Arquivo melhor para testes repetidos
- **Formato de impressão:** Alinhar colunas para legibilidade
- **Erro numérico:** Determinante muito pequeno → matriz quase singular
- **Otimização:** Multiplicação pode ser vetorizada (SIMD)

### 🎯 Desafios Extras (+25 XP cada)

1. **Matrizes 4×4:** Expandir para suportar coordenadas homogêneas 3D
2. **Batch mode:** Ler sequência de operações de arquivo script
3. **Trace:** Implementar `trace(A) = Σ Aii`
4. **Potência:** `A^n` via multiplicação repetida ou diagonalização

---

## Problema 3: Visualizador de Vetores 2D

### 🎯 Objetivo

Criar programa gráfico simples que desenha vetores 2D, mostra soma, produto escalar visualmente, e permite interação com mouse.

### 📐 Contexto Teórico

Visualizar vetores ajuda a entender:
- **Soma:** Regra do paralelogramo
- **Produto escalar:** Relacionado ao ângulo (cos θ)
- **Projeção:** Componente paralela/perpendicular
- **Normalização:** Vetor unitário

### 🛠️ Especificação

**Tecnologia:** SDL2 ou SFML (gráficos simples)

**Funcionalidades:**

**Desenho:**
- Grade cartesiana (eixos X, Y)
- Vetor como seta (linha + triângulo na ponta)
- Cores diferentes para cada vetor
- Labels com coordenadas

**Interação:**
- **Click esquerdo:** Criar novo vetor (origem → click)
- **Click direito:** Remover último vetor
- **Tecla S:** Mostrar soma de todos vetores
- **Tecla D:** Mostrar produto escalar entre 2 primeiros vetores
- **Tecla N:** Normalizar vetor selecionado
- **Tecla R:** Reset (limpar tudo)

**Informações exibidas:**
- Coordenadas de cada vetor
- Magnitude
- Ângulo com eixo X
- Soma total (se ativada)
- Produto escalar (se ativado)

### 📝 Pseudocódigo

```
ESTRUTURA Vetor2D:
    origem: Ponto(x, y)
    fim: Ponto(x, y)
    cor: RGB

FUNÇÃO desenhar_vetor(v):
    // Desenhar linha da origem ao fim
    desenhar_linha(v.origem, v.fim, v.cor, espessura=2)
    
    // Desenhar seta na ponta
    direcao = normalizar(v.fim - v.origem)
    perpendicular = rotacionar_90_graus(direcao)
    
    base_seta = v.fim - direcao * 10  // 10 pixels antes da ponta
    ponta_esq = base_seta + perpendicular * 5
    ponta_dir = base_seta - perpendicular * 5
    
    desenhar_triangulo(v.fim, ponta_esq, ponta_dir, v.cor)

FUNÇÃO desenhar_grade():
    // Eixo X
    desenhar_linha((-largura/2, 0), (largura/2, 0), CINZA)
    
    // Eixo Y  
    desenhar_linha((0, -altura/2), (0, altura/2), CINZA)
    
    // Linhas de grade (opcional)
    PARA x DE -largura/2 ATÉ largura/2 A CADA 50:
        desenhar_linha((x, -altura/2), (x, altura/2), CINZA_CLARO)

FUNÇÃO processar_click_esquerdo(posicao):
    novo_vetor = Vetor2D()
    novo_vetor.origem = centro_tela
    novo_vetor.fim = posicao
    novo_vetor.cor = cor_aleatoria()
    
    adicionar_na_lista(vetores, novo_vetor)

FUNÇÃO mostrar_soma_vetores():
    soma = vetor_zero()
    
    PARA CADA v EM vetores:
        delta = v.fim - v.origem
        soma = soma + delta
    
    vetor_soma = Vetor2D()
    vetor_soma.origem = centro_tela
    vetor_soma.fim = centro_tela + soma
    vetor_soma.cor = VERMELHO_FORTE
    
    desenhar_vetor(vetor_soma)
    desenhar_texto("SOMA", vetor_soma.fim, VERMELHO_FORTE)

FUNÇÃO mostrar_produto_escalar():
    SE tamanho(vetores) < 2:
        RETORNAR
    
    v1 = vetores[0].fim - vetores[0].origem
    v2 = vetores[1].fim - vetores[1].origem
    
    dot = v1.x * v2.x + v1.y * v2.y
    
    desenhar_texto(
        "v1 · v2 = " + formatado(dot), 
        posicao_topo_esquerda,
        BRANCO
    )
    
    // Opcional: desenhar projeção
    proj = projetar(v2, sobre=v1)
    desenhar_vetor_tracejado(proj, COR=AMARELO)
```

### 🧪 Testes

**Casos de teste manual:**

1. **Teste perpendicularidade:**
   - Criar v1 = (100, 0)
   - Criar v2 = (0, 100)
   - Pressionar D → produto escalar deve mostrar 0

2. **Teste soma:**
   - Criar v1 = (50, 50)
   - Criar v2 = (30, -20)
   - Pressionar S → vetor vermelho deve ir até (80, 30)

3. **Teste normalização:**
   - Criar v = (30, 40)
   - Pressionar N → magnitude deve ficar ~1 (50 pixels na tela)

4. **Teste múltiplos vetores:**
   - Criar 5 vetores aleatórios
   - Pressionar S → soma deve ser vetorial correta

### 💡 Insights

- **Sistema de coordenadas:** SDL tem Y crescendo para baixo, inverter na conversão
- **Escala visual:** 1 unidade = 10 pixels (ajustar conforme necessário)
- **Performance:** Redesenhar tudo a cada frame (60 FPS)
- **UX:** Feedback visual ao passar mouse (highlight)

### 🎯 Desafios Extras (+35 XP cada)

1. **Vetores móveis:** Arrastar origem/fim com mouse
2. **Ângulo animado:** Mostrar arco do ângulo entre dois vetores
3. **Bases customizadas:** Desenhar base não-canônica e expressar vetores nela
4. **Exportar imagem:** Salvar visualização como PNG

---

## 🎓 Entregáveis

Para cada problema:
1. ✅ Código-fonte completo e compilável
2. ✅ Makefile ou instruções de compilação
3. ✅ Arquivo de testes (automatizado ou manual)
4. ✅ README com:
   - Como compilar/executar
   - Exemplos de uso
   - Decisões de design

---

## 🎯 Próximos Passos

Após completar:
1. ✅ **Intermediários:** `p2-intermediarios.md` (Physics engine, Ray casting)
2. ✅ **Avançados:** `p3-avancados.md` (SVD, PCA, Solvers)
3. ✅ **Projeto final:** `k5-projeto/` (Engine de transformações 2D)

---

## 💡 Dicas Gerais

**Organização:**
- Use `-lm` para funções matemáticas (sqrt, cos, etc.)
- Separe headers (.h) de implementação (.c)
- Use `const` para parâmetros que não mudam

**Debugging:**
- Imprima vetores/matrizes intermediários
- Use `assert()` para verificar invariantes
- Valgrind para verificar leaks de memória

**Estilo:**
- Comentários em funções não-triviais
- Nomes descritivos (vec3_add, não va)
- Indentação consistente (2 ou 4 espaços)

---

<details>
<summary><strong>💻 Exemplo de Estrutura de Projeto (Problema 1)</strong></summary>

```
problema1-vec3lib/
├── include/
│   └── vec3.h          # Declarações
├── src/
│   ├── vec3.c          # Implementações
│   └── test_vec3.c     # Testes
├── Makefile
└── README.md

# Makefile exemplo:
CC = gcc
CFLAGS = -Wall -Wextra -I include -lm

all: test_vec3

test_vec3: src/test_vec3.c src/vec3.c
	$(CC) $(CFLAGS) -o $@ $^

clean:
	rm -f test_vec3

.PHONY: all clean
```

</details>

---

**Total XP disponível:** 300 XP (+ 185 XP extras)  
**Tempo total estimado:** 4h-5h15  
**Dificuldade:** ⭐⭐⭐ Intermediário
