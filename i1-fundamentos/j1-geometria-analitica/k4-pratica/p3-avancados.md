# Problemas Avançados de Programação

## 🎯 Meta

Integrar geometria analítica em projetos avançados: ray tracer 3D completo, transformações para animação, e sistema de colisão para jogos.

---

## ⏱️ Tempo Estimado

- **Problema 1 (Ray Tracer 3D):** 3h-4h
- **Problema 2 (Sistema de Animação 3D):** 2h30-3h30
- **Problema 3 (Motor de Física Completo):** 3h-4h
- **Total:** ~8h30-11h30

---

## 📋 Pré-requisitos

- **Teoria:** Todos os módulos de teoria (t1-t4)
- **Implementação:** Todos os módulos de implementação (i1-i4)
- **Exercícios:** `k2-exercicios/` (Níveis 2-3 completos)
- **Prática:** `p1-basicos.md` e `p2-intermediarios.md` (pelo menos Problema 1 de cada)

---

## 🎚️ Dificuldade

⭐⭐⭐⭐⭐ Muito Avançado

---

## 💪 Sistema de XP

- **Problema 1:** 140 XP
- **Problema 2:** 120 XP
- **Problema 3:** 120 XP

**XP Total Disponível:** 380 XP

---

## 📊 Rastreamento de Progresso

- [ ] Problema 1: Ray Tracer 3D com Sombras (0/1) - 140 XP
- [ ] Problema 2: Sistema de Animação Hierárquica (0/1) - 120 XP
- [ ] Problema 3: Motor de Física com Forças e Torques (0/1) - 120 XP

**XP Conquistado:** ___ / 380 XP

---

## Problema 1: Ray Tracer 3D com Sombras e Materiais

### 🎯 Objetivo

Criar um ray tracer 3D capaz de renderizar cenas com esferas, planos, iluminação, sombras e materiais (difuso, especular).

### 📐 Contexto Teórico

**Ray Tracing Pipeline:**
1. Para cada pixel, lançar raio da câmera
2. Encontrar primeira interseção com geometria
3. Calcular iluminação (Phong shading):
   - Ambiente: `I_a = k_a * I_luz`
   - Difusa: `I_d = k_d * (N·L) * I_luz`
   - Especular: `I_s = k_s * (R·V)^n * I_luz`
4. Lançar raio de sombra para cada luz
5. (Opcional) Reflexões recursivas

**Modelo de Phong:**
- `N` = normal na superfície
- `L` = direção para a luz
- `R` = direção refletida `R = 2(N·L)N - L`
- `V` = direção para a câmera
- `n` = coeficiente de brilho (shininess)

### 🛠️ Especificação

**Cena (arquivo JSON):**
```json
{
  "camera": {
    "position": [0, 0, 5],
    "lookAt": [0, 0, 0],
    "fov": 60,
    "resolution": [800, 600]
  },
  "lights": [
    {"position": [5, 5, 5], "intensity": [1, 1, 1]},
    {"position": [-5, 3, 2], "intensity": [0.5, 0.5, 0.5]}
  ],
  "objects": [
    {
      "type": "sphere",
      "center": [0, 0, 0],
      "radius": 1,
      "material": {
        "ambient": [0.1, 0.1, 0.1],
        "diffuse": [0.7, 0.3, 0.3],
        "specular": [1, 1, 1],
        "shininess": 32
      }
    },
    {
      "type": "plane",
      "normal": [0, 1, 0],
      "d": -1,
      "material": {
        "ambient": [0.1, 0.1, 0.1],
        "diffuse": [0.5, 0.5, 0.5],
        "specular": [0.3, 0.3, 0.3],
        "shininess": 8
      }
    }
  ]
}
```

**Funcionalidades obrigatórias:**

1. **Parser de cena** (JSON)
2. **Geração de raios primários:**
   - Calcular direção do raio para cada pixel
   - Transformação câmera (look-at matrix)
3. **Interseções:**
   - Raio-esfera (do p2)
   - Raio-plano
4. **Iluminação (Phong):**
   - Componente ambiente
   - Componente difusa (Lambertian)
   - Componente especular
5. **Sombras:**
   - Lançar raio de sombra para cada luz
   - Se atingir objeto, luz é bloqueada
6. **Saída:**
   - Imagem PPM (formato ASCII simples)
   - Tempo de renderização

**Extras (opcional):**
- Reflexões recursivas (espelhos)
- Anti-aliasing (supersampling)
- Múltiplos tipos de objetos (cilindros, cones)

### 📝 Pseudocódigo

```
ESTRUTURA Camera:
    posicao: Vec3
    direcao: Vec3
    up: Vec3
    fov: número
    largura, altura: inteiros

ESTRUTURA Luz:
    posicao: Vec3
    intensidade: Vec3 (RGB)

ESTRUTURA Material:
    ambiente: Vec3
    difuso: Vec3
    especular: Vec3
    shininess: número

ESTRUTURA HitRecord:
    tem_hit: booleano
    ponto: Vec3
    normal: Vec3
    distancia: número
    material: Material

FUNÇÃO gerar_raio_primario(camera, pixel_x, pixel_y):
    // Converter coordenadas de pixel para NDC [-1, 1]
    ndc_x = (2 * pixel_x / camera.largura) - 1
    ndc_y = 1 - (2 * pixel_y / camera.altura)
    
    // Ajustar por aspect ratio
    aspect = camera.largura / camera.altura
    ndc_x *= aspect
    
    // Ajustar por FOV
    fov_rad = graus_para_rad(camera.fov)
    escala = tan(fov_rad / 2)
    ndc_x *= escala
    ndc_y *= escala
    
    // Calcular vetores da câmera
    forward = normalizar(camera.direcao)
    right = normalizar(produto_vetorial(forward, camera.up))
    up_corrigido = produto_vetorial(right, forward)
    
    // Direção do raio
    direcao = normalizar(
        forward + ndc_x * right + ndc_y * up_corrigido
    )
    
    RETORNAR Raio(camera.posicao, direcao)

FUNÇÃO tracar_raio(raio, cena, profundidade_max):
    // Encontrar interseção mais próxima
    hit = encontrar_intersecao_mais_proxima(raio, cena.objetos)
    
    SE NÃO hit.tem_hit:
        RETORNAR cor_fundo  // Céu
    
    // Calcular iluminação
    cor = hit.material.ambiente * cena.luz_ambiente
    
    PARA CADA luz EM cena.luzes:
        // Raio de sombra
        direcao_luz = normalizar(luz.posicao - hit.ponto)
        dist_luz = magnitude(luz.posicao - hit.ponto)
        
        raio_sombra = Raio(hit.ponto + hit.normal * 0.001, direcao_luz)
        hit_sombra = encontrar_intersecao_mais_proxima(raio_sombra, cena.objetos)
        
        // Se raio atinge algo antes da luz, está em sombra
        SE hit_sombra.tem_hit E hit_sombra.distancia < dist_luz:
            CONTINUAR  // Pular esta luz
        
        // Componente difusa (Lambertian)
        n_dot_l = max(0, produto_escalar(hit.normal, direcao_luz))
        difusa = hit.material.difuso * luz.intensidade * n_dot_l
        
        // Componente especular (Blinn-Phong)
        dir_view = normalizar(raio.origem - hit.ponto)
        halfvec = normalizar(direcao_luz + dir_view)
        n_dot_h = max(0, produto_escalar(hit.normal, halfvec))
        especular = hit.material.especular * luz.intensidade * 
                    potencia(n_dot_h, hit.material.shininess)
        
        cor += difusa + especular
    
    // Reflexão (se profundidade > 0 e material refletivo)
    SE profundidade_max > 0 E hit.material.refletividade > 0:
        dir_reflexao = refletir(raio.direcao, hit.normal)
        raio_reflexao = Raio(hit.ponto + hit.normal * 0.001, dir_reflexao)
        cor_reflexao = tracar_raio(raio_reflexao, cena, profundidade_max - 1)
        cor += cor_reflexao * hit.material.refletividade
    
    RETORNAR clampar(cor, 0, 1)

FUNÇÃO renderizar(cena):
    imagem = criar_imagem(cena.camera.largura, cena.camera.altura)
    
    PARA y DE 0 ATÉ altura-1:
        PARA x DE 0 ATÉ largura-1:
            raio = gerar_raio_primario(cena.camera, x, y)
            cor = tracar_raio(raio, cena, profundidade_reflexao=3)
            imagem[y][x] = cor
        
        // Mostrar progresso
        SE y % 10 == 0:
            IMPRIMIR "Progresso: {y/altura*100}%"
    
    RETORNAR imagem

FUNÇÃO refletir(vetor_incidente, normal):
    // R = V - 2(V·N)N
    RETORNAR vetor_incidente - 2 * produto_escalar(vetor_incidente, normal) * normal
```

### 🧪 Cena de Teste

**Cena simples (3 esferas + chão):**
```json
{
  "camera": {
    "position": [0, 2, 6],
    "lookAt": [0, 0, 0],
    "fov": 45,
    "resolution": [400, 300]
  },
  "lights": [
    {"position": [10, 10, 10], "intensity": [1, 1, 1]}
  ],
  "objects": [
    {
      "type": "sphere",
      "center": [0, 0, 0],
      "radius": 1,
      "material": {
        "diffuse": [0.8, 0.2, 0.2],
        "specular": [0.8, 0.8, 0.8],
        "shininess": 32
      }
    },
    {
      "type": "sphere",
      "center": [-2.5, 0.5, -1],
      "radius": 0.5,
      "material": {
        "diffuse": [0.2, 0.8, 0.2],
        "specular": [0.5, 0.5, 0.5],
        "shininess": 16
      }
    },
    {
      "type": "plane",
      "normal": [0, 1, 0],
      "d": -1,
      "material": {
        "diffuse": [0.6, 0.6, 0.6],
        "specular": [0.2, 0.2, 0.2],
        "shininess": 4
      }
    }
  ]
}
```

Resultado esperado:
- Esfera vermelha no centro com destaque especular
- Esfera verde à esquerda
- Chão cinza com sombras das esferas
- Gradiente de iluminação suave

### 🏆 Critérios de Conclusão

- [ ] Parser JSON funcional
- [ ] Geração de raios primários correta
- [ ] Interseções raio-esfera e raio-plano
- [ ] Modelo de Phong implementado
- [ ] Sombras funcionais
- [ ] Imagem PPM gerada corretamente
- [ ] Pelo menos 2 cenas de teste renderizadas

**XP Concedido:** 140 XP

---

## Problema 2: Sistema de Animação Hierárquica 3D

### 🎯 Objetivo

Criar sistema de animação baseado em transformações hierárquicas (como esqueleto de personagem ou braço robótico) com interpolação suave entre keyframes.

### 📐 Contexto Teórico

**Hierarquia de transformações:**
- Cada "osso" tem transformação local (relativa ao pai)
- Transformação global = Pai_global × Local
- Exemplo: Mão se move com braço, que se move com ombro

**Interpolação:**
- Linear: `P(t) = (1-t)P₀ + t·P₁`
- SLERP (rotações): Interpolação esférica de quaternions
- Cubic spline: Suavidade C²

### 🛠️ Especificação

**Estrutura hierárquica (esqueleto simples):**
```
Raiz (Tronco)
├─ Braço_Esquerdo
│  └─ Antebraço_Esquerdo
│     └─ Mão_Esquerda
└─ Braço_Direito
   └─ Antebraço_Direito
      └─ Mão_Direita
```

**Keyframes (arquivo de animação):**
```
# Tempo 0.0s
Tronco: pos=(0,0,0) rot=(0,0,0) escala=(1,1,1)
Braço_Esq: rot_local=(0,0,0)
Antebraço_Esq: rot_local=(0,0,0)

# Tempo 1.0s (onda com braço)
Braço_Esq: rot_local=(0,0,90)
Antebraço_Esq: rot_local=(0,0,-45)

# Tempo 2.0s (abaixar braço)
Braço_Esq: rot_local=(0,0,0)
Antebraço_Esq: rot_local=(0,0,0)
```

**Funcionalidades:**

1. **Estrutura de ossos:**
   - Cada osso tem: transformação local, pai, filhos
   - Calcular transformação global (acumular hierarquia)
2. **Keyframe system:**
   - Armazenar estado (posição, rotação, escala) em tempos específicos
   - Interpolação linear entre keyframes
3. **Animação:**
   - Atualizar em loop (delta time)
   - Calcular transformações interpoladas
   - Atualizar toda hierarquia
4. **Visualização:**
   - Renderizar ossos como linhas/cápsulas
   - Mostrar eixos locais
   - Exportar frames como imagens

### 📝 Pseudocódigo

```
ESTRUTURA Osso:
    nome: string
    pai: Osso (ou NULL se raiz)
    filhos: lista de Osso
    
    // Transformação local (relativa ao pai)
    pos_local: Vec3
    rot_local: Vec3  // Euler angles ou quaternion
    escala_local: Vec3
    
    // Transformação global (cache)
    matriz_global: Matriz4x4

ESTRUTURA Keyframe:
    tempo: número
    osso: string (nome)
    posicao: Vec3
    rotacao: Vec3
    escala: Vec3

ESTRUTURA Animacao:
    keyframes: lista de Keyframe (ordenado por tempo)
    duracao: número

FUNÇÃO calcular_matriz_local(osso):
    mat_trans = criar_translacao(osso.pos_local)
    mat_rot = criar_rotacao_euler(osso.rot_local)
    mat_escala = criar_escala(osso.escala_local)
    
    // TRS: primeiro escala, depois rota, depois translada
    RETORNAR mat_trans * mat_rot * mat_escala

FUNÇÃO atualizar_hierarquia(osso, matriz_pai):
    """Calcula transformações globais recursivamente"""
    mat_local = calcular_matriz_local(osso)
    osso.matriz_global = matriz_pai * mat_local
    
    PARA CADA filho EM osso.filhos:
        atualizar_hierarquia(filho, osso.matriz_global)

FUNÇÃO interpolar_keyframes(anim, tempo_atual, nome_osso):
    """Encontra keyframes adjacentes e interpola"""
    // Normalizar tempo (loop)
    t = tempo_atual % anim.duracao
    
    // Filtrar keyframes deste osso
    kfs_osso = filtrar(anim.keyframes, lambda k: k.osso == nome_osso)
    
    // Encontrar keyframes antes e depois de t
    kf_anterior = NULL
    kf_proximo = NULL
    
    PARA i DE 0 ATÉ tamanho(kfs_osso)-1:
        SE kfs_osso[i].tempo <= t:
            kf_anterior = kfs_osso[i]
        SE kfs_osso[i].tempo > t E kf_proximo == NULL:
            kf_proximo = kfs_osso[i]
            QUEBRAR
    
    SE kf_anterior == NULL OU kf_proximo == NULL:
        RETORNAR transformacao_padrao
    
    // Fator de interpolação [0, 1]
    delta_t = kf_proximo.tempo - kf_anterior.tempo
    fator = (t - kf_anterior.tempo) / delta_t
    
    // Interpolar linearmente
    pos = lerp(kf_anterior.posicao, kf_proximo.posicao, fator)
    rot = lerp(kf_anterior.rotacao, kf_proximo.rotacao, fator)
    escala = lerp(kf_anterior.escala, kf_proximo.escala, fator)
    
    RETORNAR (pos, rot, escala)

FUNÇÃO animar(esqueleto, animacao, tempo_atual):
    """Atualiza esqueleto baseado na animação"""
    PARA CADA osso EM esqueleto.todos_ossos():
        (pos, rot, escala) = interpolar_keyframes(animacao, tempo_atual, osso.nome)
        osso.pos_local = pos
        osso.rot_local = rot
        osso.escala_local = escala
    
    // Recalcular hierarquia
    atualizar_hierarquia(esqueleto.raiz, criar_matriz_identidade())

FUNÇÃO renderizar_esqueleto(esqueleto):
    PARA CADA osso EM esqueleto.todos_ossos():
        // Posição global do osso
        pos_osso = extrair_posicao(osso.matriz_global)
        
        SE osso.pai != NULL:
            pos_pai = extrair_posicao(osso.pai.matriz_global)
            desenhar_linha(pos_pai, pos_osso, cor=branco)
        
        // Desenhar eixos locais (RGB = XYZ)
        desenhar_eixos(osso.matriz_global, tamanho=0.5)
```

### 🧪 Teste

Animação de "onda" com braço:
```
Keyframe 0.0s:
  Braço: rot=(0,0,0)
  Antebraço: rot=(0,0,0)

Keyframe 0.5s:
  Braço: rot=(0,0,45)
  Antebraço: rot=(0,0,-30)

Keyframe 1.0s:
  Braço: rot=(0,0,90)
  Antebraço: rot=(0,0,-90)

Keyframe 1.5s:
  Braço: rot=(0,0,45)
  Antebraço: rot=(0,0,-30)

Keyframe 2.0s:
  Braço: rot=(0,0,0)
  Antebraço: rot=(0,0,0)
```

Resultado esperado:
- Braço levanta suavemente até posição vertical
- Antebraço dobra enquanto braço levanta (efeito de onda)
- Retorna à posição inicial suavemente
- Loop contínuo

### 🏆 Critérios de Conclusão

- [ ] Hierarquia de ossos funcional
- [ ] Transformações locais → globais corretas
- [ ] Interpolação de keyframes suave
- [ ] Loop de animação
- [ ] Visualização (mesmo que simples)
- [ ] Pelo menos 1 animação completa

**XP Concedido:** 120 XP

---

## Problema 3: Motor de Física com Forças e Torques

### 🎯 Objetivo

Expandir motor de colisão (do p2) para incluir física rotacional: torque, momento de inércia, e rotação de objetos rígidos.

### 📐 Contexto Teórico

**Dinâmica rotacional:**
- **Torque:** `τ = r × F` (produto vetorial)
- **Momento angular:** `L = I·ω` (I = tensor de inércia, ω = velocidade angular)
- **Equação:** `dL/dt = τ` → `I·α = τ` (α = aceleração angular)

**Tensor de inércia (esfera):**
```
I = (2/5)mr² * [1 0 0]
              [0 1 0]
              [0 0 1]
```

### 🛠️ Especificação

**Entidades com rotação:**
```c
typedef struct {
    // Translação
    Vec3 posicao;
    Vec3 velocidade;
    double massa;
    
    // Rotação
    Vec3 orientacao;  // Quaternion ou Euler
    Vec3 vel_angular;  // ω (rad/s)
    Matriz3x3 tensor_inercia;
    
    // Geometria
    double raio;
} CorpoRigido;
```

**Funcionalidades:**
- Aplicar força em ponto (gera torque se fora do centro de massa)
- Integrar física rotacional
- Colisão com transferência de momento angular
- Visualizar rotação (setas/textura)

### 📝 Pseudocódigo (resumido)

```
FUNÇÃO aplicar_forca_em_ponto(corpo, forca, ponto_aplicacao):
    // Força translacional
    corpo.velocidade += (forca / corpo.massa) * dt
    
    // Torque rotacional
    r = ponto_aplicacao - corpo.posicao
    torque = produto_vetorial(r, forca)
    
    // α = I⁻¹ * τ
    I_inv = inverter(corpo.tensor_inercia)
    aceleracao_angular = multiplicar_matriz_vetor(I_inv, torque)
    corpo.vel_angular += aceleracao_angular * dt

FUNÇÃO integrar_rotacao(corpo, dt):
    // Atualizar orientação usando velocidade angular
    // (Simplificado: Euler. Ideal: quaternion integration)
    delta_rot = corpo.vel_angular * dt
    corpo.orientacao += delta_rot
```

### 🧪 Teste

Cubo flutuante com força aplicada em canto:
- Deve transladar E rotacionar
- Conservar momento angular

### 🏆 Critérios

- [ ] Torque calculado corretamente
- [ ] Rotação integrada
- [ ] Colisão com torque
- [ ] Teste funcional

**XP:** 120 XP

---

## 🔗 Próximos Passos

- `k5-projeto/` → Sistema completo de navegação
- Explore bibliotecas: Bullet Physics, GLM

---

## 📚 Recursos

- Game Physics (Millington)
- Real-Time Rendering (Möller)
- Scratchapixel
