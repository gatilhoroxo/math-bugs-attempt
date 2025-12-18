# Projeto Âncora: Engine de Transformações 2D

## 🎯 Objetivo do Projeto

Criar um programa interativo que visualiza transformações lineares 2D, permitindo que você **veja** álgebra linear em ação. Este projeto solidificará sua compreensão de vetores, matrizes e transformações.

**O que você vai aprender:**
- Como matrizes transformam vetores geometricamente
- Composição de transformações (ordem importa!)
- Coordenadas homogêneas para translação
- Implementação eficiente de operações matriciais
- Visualização e debugging de conceitos abstratos

**Tempo estimado:** 8-12 horas (distribuídas ao longo de 2-3 semanas)

---

## 📋 Requisitos

### Software
- **Linguagem:** C ou C++
- **Biblioteca gráfica:** SDL2 (simples e multiplataforma)
  - Instalação Ubuntu/Debian: `sudo apt-get install libsdl2-dev`
  - Instalação macOS: `brew install sdl2`
  - Windows: Baixar de https://www.libsdl.org/

**Alternativa:** Se preferir, pode usar SFML ou até mesmo fazer uma versão em Python com Pygame/Matplotlib para prototipagem rápida.

### Matemática Necessária
Você deve ter estudado (nos arquivos anteriores):
- [x] Vetores 2D (soma, escala, produto escalar)
- [x] Matrizes 2×2 e 3×3
- [x] Multiplicação matriz × vetor
- [x] Coordenadas homogêneas

---

## 🏗️ Arquitetura do Projeto

```
projeto-transformacoes-2d/
├── src/
│   ├── main.c
│   ├── vec2.h / vec2.c          # Vetores 2D
│   ├── mat3.h / mat3.c          # Matrizes 3×3 (coord. homogêneas)
│   ├── shape.h / shape.c        # Formas geométricas
│   ├── transform.h / transform.c # Transformações
│   └── render.h / render.c      # Renderização
├── Makefile
├── README.md
└── screenshots/
```

---

## 📝 Etapas de Desenvolvimento

### Etapa 0: Setup do Ambiente (30 min)

**Objetivo:** Configurar SDL2 e criar uma janela básica

**Código inicial (main.c):**
```c
#include <SDL2/SDL.h>
#include <stdio.h>
#include <stdbool.h>

#define WINDOW_WIDTH 800
#define WINDOW_HEIGHT 600

int main(int argc, char* argv[]) {
    if (SDL_Init(SDL_INIT_VIDEO) < 0) {
        fprintf(stderr, "SDL init failed: %s\n", SDL_GetErrorString());
        return 1;
    }
    
    SDL_Window* window = SDL_CreateWindow(
        "Transformações 2D",
        SDL_WINDOWPOS_CENTERED,
        SDL_WINDOWPOS_CENTERED,
        WINDOW_WIDTH,
        WINDOW_HEIGHT,
        SDL_WINDOW_SHOWN
    );
    
    if (!window) {
        fprintf(stderr, "Window creation failed: %s\n", SDL_GetErrorString());
        return 1;
    }
    
    SDL_Renderer* renderer = SDL_CreateRenderer(window, -1, 
                                                 SDL_RENDERER_ACCELERATED);
    
    bool running = true;
    SDL_Event event;
    
    while (running) {
        while (SDL_PollEvent(&event)) {
            if (event.type == SDL_QUIT) {
                running = false;
            }
        }
        
        // Limpar tela (preto)
        SDL_SetRenderDrawColor(renderer, 0, 0, 0, 255);
        SDL_RenderClear(renderer);
        
        // TODO: Desenhar aqui
        
        SDL_RenderPresent(renderer);
        SDL_Delay(16); // ~60 FPS
    }
    
    SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);
    SDL_Quit();
    
    return 0;
}
```

**Compilar:**
```bash
gcc main.c -o transform2d -lSDL2 -lm
```

**✅ Checklist:**
- [ ] SDL2 instalado
- [ ] Janela abre sem erros
- [ ] Tela preta aparece

---

### Etapa 1: Implementar Estruturas Básicas (1-2h)

**Objetivo:** Criar tipos para Vec2 e Mat3

**vec2.h:**
```c
#ifndef VEC2_H
#define VEC2_H

typedef struct {
    double x, y;
} Vec2;

// Construtores
Vec2 vec2_create(double x, double y);
Vec2 vec2_zero(void);

// Operações
Vec2 vec2_add(Vec2 a, Vec2 b);
Vec2 vec2_sub(Vec2 a, Vec2 b);
Vec2 vec2_scale(Vec2 v, double k);
double vec2_dot(Vec2 a, Vec2 b);
double vec2_length(Vec2 v);
Vec2 vec2_normalize(Vec2 v);

// Utilidades
void vec2_print(Vec2 v);

#endif
```

**mat3.h:**
```c
#ifndef MAT3_H
#define MAT3_H

#include "vec2.h"

typedef struct {
    double x, y, w; // Coordenadas homogêneas (x, y, 1)
} Vec2H;

typedef struct {
    double m[3][3];
} Mat3;

// Construtores
Mat3 mat3_identity(void);
Mat3 mat3_zero(void);

// Transformações básicas
Mat3 mat3_translation(double tx, double ty);
Mat3 mat3_rotation(double angle_rad);
Mat3 mat3_scale(double sx, double sy);

// Operações
Mat3 mat3_mul(Mat3 a, Mat3 b); // A × B
Vec2H mat3_mul_vec2h(Mat3 m, Vec2H v); // M × v

// Conversões
Vec2H vec2_to_homogeneous(Vec2 v);
Vec2 homogeneous_to_vec2(Vec2H v);

// Utilidades
void mat3_print(Mat3 m);

#endif
```

**Tarefa:** Implemente essas funções em vec2.c e mat3.c

**Dicas:**
- Comece com as funções mais simples (create, zero, identity)
- Teste cada função isoladamente
- Use `assert()` para verificar resultados esperados

**✅ Checklist:**
- [ ] Vec2 implementado e testado
- [ ] Mat3 implementado e testado
- [ ] Multiplicação matricial funcionando
- [ ] Coordenadas homogêneas funcionando

---

### Etapa 2: Desenhar Formas Simples (1-2h)

**Objetivo:** Desenhar um quadrado e um triângulo na tela

**shape.h:**
```c
#ifndef SHAPE_H
#define SHAPE_H

#include "vec2.h"
#include <SDL2/SDL.h>

#define MAX_VERTICES 100

typedef struct {
    Vec2 vertices[MAX_VERTICES];
    int vertex_count;
    SDL_Color color;
} Shape;

// Construtores
Shape shape_create_square(double size);
Shape shape_create_triangle(double size);
Shape shape_create_regular_polygon(int sides, double radius);

// Renderização
void shape_draw(SDL_Renderer* renderer, Shape* shape, 
                int screen_width, int screen_height);

// Transformações (aplicar matriz a todos os vértices)
void shape_transform(Shape* shape, Mat3 transform);

#endif
```

**Função de desenho:**
```c
void shape_draw(SDL_Renderer* renderer, Shape* shape, 
                int screen_width, int screen_height) {
    SDL_SetRenderDrawColor(renderer, 
                          shape->color.r, 
                          shape->color.g, 
                          shape->color.b, 
                          shape->color.a);
    
    // Converter coordenadas matemáticas para coordenadas de tela
    // Origem no centro, Y para cima
    for (int i = 0; i < shape->vertex_count; i++) {
        int next = (i + 1) % shape->vertex_count;
        
        // Transformar de coordenadas matemáticas para tela
        int x1 = (int)(shape->vertices[i].x + screen_width / 2);
        int y1 = (int)(screen_height / 2 - shape->vertices[i].y);
        int x2 = (int)(shape->vertices[next].x + screen_width / 2);
        int y2 = (int)(screen_height / 2 - shape->vertices[next].y);
        
        SDL_RenderDrawLine(renderer, x1, y1, x2, y2);
    }
}
```

**Tarefa:** 
1. Implemente shape_create_square que retorna um quadrado centrado na origem
2. Desenhe o quadrado na tela
3. Adicione um sistema de eixos (linhas X e Y)

**✅ Checklist:**
- [ ] Quadrado desenhado no centro
- [ ] Eixos X e Y visíveis
- [ ] Cores diferentes para cada forma
- [ ] Coordenadas matemáticas → tela funcionando

---

### Etapa 3: Aplicar Transformações Básicas (2-3h)

**Objetivo:** Rotacionar, escalar e transladar formas

**Adicione controles do teclado:**
```c
// No loop principal
while (SDL_PollEvent(&event)) {
    if (event.type == SDL_QUIT) {
        running = false;
    }
    
    if (event.type == SDL_KEYDOWN) {
        Mat3 transform;
        
        switch (event.key.keysym.sym) {
            case SDLK_r: // Rotação +10°
                transform = mat3_rotation(M_PI / 18); // 10° em radianos
                shape_transform(&my_shape, transform);
                break;
            
            case SDLK_e: // Rotação -10°
                transform = mat3_rotation(-M_PI / 18);
                shape_transform(&my_shape, transform);
                break;
            
            case SDLK_UP: // Transladar para cima
                transform = mat3_translation(0, 10);
                shape_transform(&my_shape, transform);
                break;
            
            case SDLK_DOWN: // Transladar para baixo
                transform = mat3_translation(0, -10);
                shape_transform(&my_shape, transform);
                break;
            
            case SDLK_LEFT:
                transform = mat3_translation(-10, 0);
                shape_transform(&my_shape, transform);
                break;
            
            case SDLK_RIGHT:
                transform = mat3_translation(10, 0);
                shape_transform(&my_shape, transform);
                break;
            
            case SDLK_EQUALS: // Escala aumentar
                transform = mat3_scale(1.1, 1.1);
                shape_transform(&my_shape, transform);
                break;
            
            case SDLK_MINUS: // Escala diminuir
                transform = mat3_scale(0.9, 0.9);
                shape_transform(&my_shape, transform);
                break;
            
            case SDLK_SPACE: // Reset
                my_shape = shape_create_square(50);
                break;
        }
    }
}
```

**✅ Checklist:**
- [ ] Rotação funcionando (R/E)
- [ ] Translação funcionando (setas)
- [ ] Escala funcionando (+/-)
- [ ] Reset funcionando (espaço)
- [ ] A forma não "explode" ou some da tela

---

### Etapa 4: Visualizar Transformação em Tempo Real (2-3h)

**Objetivo:** Mostrar DUAS formas - uma original (fantasma) e uma transformada

**Estrutura:**
```c
typedef struct {
    Shape original;      // Forma original (cinza, transparente)
    Shape transformed;   // Forma após transformação (cor vibrante)
    Mat3 current_transform; // Transformação acumulada
} TransformDemo;

TransformDemo demo_create(Shape shape) {
    TransformDemo demo;
    demo.original = shape;
    demo.transformed = shape;
    demo.current_transform = mat3_identity();
    return demo;
}

void demo_apply_transform(TransformDemo* demo, Mat3 new_transform) {
    // Acumular transformação
    demo->current_transform = mat3_mul(new_transform, 
                                       demo->current_transform);
    
    // Aplicar transformação acumulada ao original
    demo->transformed = demo->original;
    shape_transform(&demo->transformed, demo->current_transform);
}

void demo_reset(TransformDemo* demo) {
    demo->current_transform = mat3_identity();
    demo->transformed = demo->original;
}
```

**Adicione UI de texto:**
```c
// Mostre a matriz atual na tela (use SDL_ttf ou apenas printf no terminal)
void print_current_matrix(Mat3 m) {
    printf("\nMatriz de Transformação Atual:\n");
    printf("[ %6.2f  %6.2f  %6.2f ]\n", m.m[0][0], m.m[0][1], m.m[0][2]);
    printf("[ %6.2f  %6.2f  %6.2f ]\n", m.m[1][0], m.m[1][1], m.m[1][2]);
    printf("[ %6.2f  %6.2f  %6.2f ]\n", m.m[2][0], m.m[2][1], m.m[2][2]);
}
```

**✅ Checklist:**
- [ ] Forma original visível (cinza claro)
- [ ] Forma transformada visível (cor diferente)
- [ ] Matriz exibida no terminal
- [ ] Reset volta ao estado inicial

---

### Etapa 5: Composição de Transformações (2h)

**Objetivo:** Entender que A × B ≠ B × A

**Adicione modo de composição:**
```c
typedef enum {
    MODE_IMMEDIATE,    // Aplicar direto na forma
    MODE_COMPOSE       // Compor com transformação atual
} TransformMode;

// Experimente:
// 1. Rotacionar 45° + Transladar (10, 0)
// 2. Transladar (10, 0) + Rotacionar 45°
// Veja a diferença!
```

**Desafio:** Crie um modo "gravador" que armazena as transformações aplicadas e permite replay

**✅ Checklist:**
- [ ] Modo imediato funcionando
- [ ] Modo composição funcionando
- [ ] Diferença visível entre ordem de operações
- [ ] Pode desfazer (Ctrl+Z) a última transformação

---

### Etapa 6: Features Avançadas (Opcional, 3-5h)

**6.1 Múltiplas Formas**
- Desenhe várias formas diferentes
- Cada uma com sua própria transformação
- Seleção com mouse

**6.2 Animação**
- Interpolar entre duas transformações
- Usar lerp para transição suave
```c
Mat3 mat3_lerp(Mat3 a, Mat3 b, double t) {
    Mat3 result;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            result.m[i][j] = a.m[i][j] + t * (b.m[i][j] - a.m[i][j]);
        }
    }
    return result;
}
```

**6.3 Transformações Customizadas**
- Cisalhamento (shear)
- Reflexão
- Projeção

**6.4 Grid de Transformação**
- Desenhar uma grade
- Aplicar transformação na grade inteira
- Ver como o espaço se "distorce"

**6.5 Visualizar Autovalores/Autovetores**
- Para uma matriz dada, mostrar suas direções especiais
- Desenhar os autovetores como setas

---

## 🎓 Conceitos para Fixar

Ao final do projeto, você deve ser capaz de:

1. **Explicar geometricamente:**
   - O que uma matriz de rotação faz?
   - Por que translação precisa de coordenadas homogêneas?
   - O que o determinante de uma transformação significa?

2. **Implementar eficientemente:**
   - Multiplicação matriz × vetor
   - Composição de transformações
   - Conversão entre sistemas de coordenadas

3. **Debugar visualmente:**
   - Ver se uma transformação está correta
   - Identificar bugs pela geometria
   - Entender ordem de operações

---

## 🐛 Problemas Comuns e Soluções

### "Minha rotação está errada!"
- Verifique se está usando radianos, não graus
- SDL usa Y crescendo para baixo, você precisa inverter

### "A translação não funciona!"
- Certifique-se de usar coordenadas homogêneas (x, y, 1)
- Verifique se w = 1 após transformação

### "A ordem das transformações importa?"
- **SIM!** Rotação depois translação ≠ Translação depois rotação
- Em geral: `Transform_final = T × R × S` (translação por último)

### "Minha forma está espelhada!"
- SDL tem Y crescendo para baixo
- Inverta Y ao converter de/para coordenadas de tela

---

## 📊 Critérios de Sucesso

Seu projeto está completo quando:

- [ ] Implementa Vec2, Mat3 com todos os métodos básicos
- [ ] Desenha pelo menos 2 formas diferentes
- [ ] Aplica rotação, translação e escala via teclado
- [ ] Mostra forma original + transformada simultaneamente
- [ ] Demonstra que ordem de transformações importa
- [ ] Exibe a matriz de transformação atual
- [ ] Código está organizado em múltiplos arquivos
- [ ] Tem um README.md explicando como usar
- [ ] Funciona sem bugs ou crashes

**Bônus:** 
- [ ] Suporta múltiplas formas
- [ ] Tem animações suaves
- [ ] Visualiza grid transformado
- [ ] Salva/carrega sequências de transformações

---

## 🎯 Extensões Futuras

Depois de completar este projeto, você pode:

1. **Adicionar 3D** - Expandir para Mat4 e transformações 3D
2. **Ray Tracer** - Usar para calcular interseções raio-objeto
3. **Physics Engine** - Adicionar colisões e física básica
4. **Game Engine** - Transformar em base de um engine de jogos 2D

---

## 📚 Recursos Complementares

**Tutoriais SDL2:**
- https://lazyfoo.net/tutorials/SDL/ (excelente para começar)
- https://wiki.libsdl.org/ (documentação oficial)

**Visualizações:**
- Assista novamente "Linear transformations" do 3Blue1Brown
- https://www.geogebra.org/ (brincar com transformações online)

**Debugging:**
- Imprima as matrizes a cada passo
- Use `assert()` para verificar propriedades
- Desenhe vetores base (i, j) para ver direção

---

## ✅ Checklist Final

Após completar o projeto:

- [ ] Consegue explicar cada transformação geometricamente
- [ ] Entende por que ordem de multiplicação importa
- [ ] Pode implementar qualquer transformação linear 2D
- [ ] Sabe converter entre coordenadas matemáticas e de tela
- [ ] Código está limpo, organizado e documentado
- [ ] Projeto está no repositório com commit messages claros

**Parabéns!** 🎉 Você construiu uma ferramenta que materializa conceitos abstratos de álgebra linear. Isso é exatamente o tipo de projeto que solidifica o aprendizado!

---

## 💬 Feedback e Iteração

Após terminar, reflita:
1. O que foi mais difícil?
2. Que conceito finalmente "clicou"?
3. O que você faria diferente?
4. Que extensão você quer adicionar?

Esse tipo de reflexão é essencial para aprendizado profundo!