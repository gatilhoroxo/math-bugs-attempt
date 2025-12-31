# Implementação: Pontos, Retas e Planos

## 🎯 Meta

Implementar operações fundamentais com pontos, retas e planos do zero, entendendo como representá-los, calcular distâncias, interseções e como essas estruturas aparecem em gráficos 3D, navegação e física.

---

## ⏱️ Tempo Estimado

- **Leitura + Pseudocódigo:** 30-40 min
- **Implementação em linguagem real:** 1h-1h30
- **Testes e debugging:** 20-30 min
- **Total:** ~2h-2h40

---

## 📋 Pré-requisitos

- Leitura de `k1-teoria/t1-pontos-retas-planos.md`
- Exercícios `k2-exercicios/e1-pontos-retas-planos-exercicios.md` (pelo menos Nível 1)

---

## 🎚️ Dificuldade

⭐⭐ Iniciante-Intermediário

---

## 📐 Conceitos-Chave

1. **Ponto:** Posição no espaço `P = (x, y, z)`
2. **Reta:** Conjunto de pontos `P(t) = P₀ + t·d` onde `d` é vetor direção
3. **Plano:** Conjunto de pontos `ax + by + cz + d = 0` ou `n·(P - P₀) = 0`
4. **Distância ponto-ponto:** `||P₁ - P₂||`
5. **Interseção:** Resolver sistemas de equações

---

## 🧩 Estrutura de Dados

### Pseudocódigo (abstrato)

```
ESTRUTURA Ponto3D:
    CAMPOS:
        x: número real
        y: número real
        z: número real

ESTRUTURA Reta3D:
    CAMPOS:
        origem: Ponto3D      // Ponto por onde passa
        direcao: Ponto3D     // Vetor direção (usamos Ponto3D como vetor)

ESTRUTURA Plano3D:
    CAMPOS:
        normal: Ponto3D      // Vetor normal (a, b, c)
        d: número real       // Termo constante em ax + by + cz + d = 0
```

**Design:** Usamos estruturas simples. Em produção, classes com métodos especializados ou bibliotecas como GLM.

---

## 🛠️ Implementações

### 1. Operações com Pontos

#### Pseudocódigo

```
FUNÇÃO criar_ponto(x, y, z):
    RETORNAR Ponto3D com (x, y, z)

FUNÇÃO distancia_pontos(p1, p2):
    """Distância euclidiana entre dois pontos"""
    dx = p2.x - p1.x
    dy = p2.y - p1.y
    dz = p2.z - p1.z
    RETORNAR raiz_quadrada(dx² + dy² + dz²)

FUNÇÃO ponto_medio(p1, p2):
    """Ponto médio entre dois pontos"""
    RETORNAR criar_ponto(
        (p1.x + p2.x) / 2,
        (p1.y + p2.y) / 2,
        (p1.z + p2.z) / 2
    )

FUNÇÃO vetor_entre_pontos(origem, destino):
    """Vetor que vai de origem até destino"""
    RETORNAR criar_ponto(
        destino.x - origem.x,
        destino.y - origem.y,
        destino.z - origem.z
    )
```

**🔍 Checkpoint:** Teste com `P₁=(1,2,3)`, `P₂=(4,6,8)`. Esperado: distância = √29 ≈ 5.385, ponto médio = (2.5, 4, 5.5).

---

### 2. Operações com Retas

#### Pseudocódigo

```
FUNÇÃO criar_reta(ponto, direcao):
    """Cria reta que passa por ponto na direção dada"""
    RETORNAR Reta3D com (ponto, direcao)

FUNÇÃO criar_reta_entre_pontos(p1, p2):
    """Cria reta que passa por p1 e p2"""
    direcao = vetor_entre_pontos(p1, p2)
    RETORNAR criar_reta(p1, direcao)

FUNÇÃO ponto_na_reta(reta, t):
    """Calcula ponto na reta para parâmetro t: P(t) = P₀ + t·d"""
    RETORNAR criar_ponto(
        reta.origem.x + t * reta.direcao.x,
        reta.origem.y + t * reta.direcao.y,
        reta.origem.z + t * reta.direcao.z
    )

FUNÇÃO distancia_ponto_reta(ponto, reta):
    """Distância de um ponto a uma reta
    Fórmula: d = ||AP × d|| / ||d||
    onde A é ponto na reta, P é o ponto, d é direção
    """
    // Vetor da origem da reta até o ponto
    ap = vetor_entre_pontos(reta.origem, ponto)
    
    // Produto vetorial ap × d
    cruz = produto_vetorial(ap, reta.direcao)
    
    // ||ap × d|| / ||d||
    mag_cruz = magnitude(cruz)
    mag_direcao = magnitude(reta.direcao)
    
    RETORNAR mag_cruz / mag_direcao

// Funções auxiliares (do módulo de vetores)
FUNÇÃO produto_vetorial(v1, v2):
    RETORNAR criar_ponto(
        v1.y * v2.z - v1.z * v2.y,
        v1.z * v2.x - v1.x * v2.z,
        v1.x * v2.y - v1.y * v2.x
    )

FUNÇÃO magnitude(v):
    RETORNAR raiz_quadrada(v.x² + v.y² + v.z²)
```

**💡 Insight:** Produto vetorial retorna vetor perpendicular. Sua magnitude é a área do paralelogramo formado pelos vetores, útil para distância.

**🔍 Checkpoint:** Reta `x=t, y=0, z=0` e ponto `(0,1,0)` têm distância 1.

---

### 3. Operações com Planos

#### Pseudocódigo

```
FUNÇÃO criar_plano(normal, d):
    """Cria plano com vetor normal e termo constante
    Equação: normal.x*x + normal.y*y + normal.z*z + d = 0
    """
    RETORNAR Plano3D com (normal, d)

FUNÇÃO criar_plano_de_ponto_e_normal(ponto, normal):
    """Cria plano que passa por ponto com normal dada"""
    // ax + by + cz + d = 0, encontrar d
    // Substituindo o ponto: a*px + b*py + c*pz + d = 0
    d = -(normal.x * ponto.x + normal.y * ponto.y + normal.z * ponto.z)
    RETORNAR criar_plano(normal, d)

FUNÇÃO criar_plano_de_tres_pontos(p1, p2, p3):
    """Cria plano que passa por três pontos não colineares"""
    // Vetores no plano
    v1 = vetor_entre_pontos(p1, p2)
    v2 = vetor_entre_pontos(p1, p3)
    
    // Normal é perpendicular a ambos: v1 × v2
    normal = produto_vetorial(v1, v2)
    
    RETORNAR criar_plano_de_ponto_e_normal(p1, normal)

FUNÇÃO distancia_ponto_plano(ponto, plano):
    """Distância de ponto a plano
    Fórmula: d = |ax + by + cz + d| / √(a² + b² + c²)
    """
    numerador = abs(
        plano.normal.x * ponto.x +
        plano.normal.y * ponto.y +
        plano.normal.z * ponto.z +
        plano.d
    )
    
    denominador = magnitude(plano.normal)
    
    RETORNAR numerador / denominador

FUNÇÃO ponto_esta_no_plano(ponto, plano, epsilon=0.0001):
    """Verifica se ponto está no plano (com tolerância)"""
    dist = distancia_ponto_plano(ponto, plano)
    RETORNAR dist < epsilon
```

**💡 Insight:** Normalizar o vetor normal facilita cálculos de distância e projeções.

**🔍 Checkpoint:** Plano `z=0` e ponto `(0,0,5)` têm distância 5.

---

### 4. Interseções

#### Pseudocódigo

```
FUNÇÃO produto_escalar(v1, v2):
    RETORNAR v1.x*v2.x + v1.y*v2.y + v1.z*v2.z

FUNÇÃO intersecao_reta_plano(reta, plano):
    """Encontra interseção entre reta e plano
    Retorna: (tem_intersecao, ponto, parametro_t)
    """
    // P(t) = P₀ + t·d
    // Substituindo em ax + by + cz + d = 0:
    // a(p₀.x + t·d.x) + b(p₀.y + t·d.y) + c(p₀.z + t·d.z) + d = 0
    
    // Denominador: n·d (produto escalar normal com direção)
    denom = produto_escalar(plano.normal, reta.direcao)
    
    // Se denom ≈ 0, reta é paralela ao plano
    SE abs(denom) < 0.0001:
        // Verificar se reta está contida no plano
        SE ponto_esta_no_plano(reta.origem, plano):
            RETORNAR (VERDADEIRO, reta.origem, 0)  // Infinitos pontos
        SENÃO:
            RETORNAR (FALSO, NULL, 0)  // Sem interseção
    
    // Numerador: -(n·p₀ + d)
    numer = -(produto_escalar(plano.normal, reta.origem) + plano.d)
    
    // Parâmetro t
    t = numer / denom
    
    // Ponto de interseção
    ponto_inter = ponto_na_reta(reta, t)
    
    RETORNAR (VERDADEIRO, ponto_inter, t)

FUNÇÃO retas_sao_paralelas(r1, r2, epsilon=0.0001):
    """Verifica se duas retas são paralelas"""
    // Direções paralelas → produto vetorial é zero
    cruz = produto_vetorial(r1.direcao, r2.direcao)
    mag = magnitude(cruz)
    RETORNAR mag < epsilon

FUNÇÃO retas_se_intersectam(r1, r2):
    """Verifica se duas retas se cruzam (coplanares)
    Retorna: (se_cruzam, ponto, t1, t2)
    """
    // Se paralelas, não cruzam (ou são coincidentes)
    SE retas_sao_paralelas(r1, r2):
        RETORNAR (FALSO, NULL, 0, 0)
    
    // Criar plano contendo r1
    // Normal = d₁ × d₂
    normal_plano = produto_vetorial(r1.direcao, r2.direcao)
    plano_r1 = criar_plano_de_ponto_e_normal(r1.origem, normal_plano)
    
    // Interseção de r2 com esse plano
    (tem_inter, ponto_inter, t2) = intersecao_reta_plano(r2, plano_r1)
    
    SE NÃO tem_inter:
        RETORNAR (FALSO, NULL, 0, 0)  // Retas reversas
    
    // Verificar se ponto está em r1 também
    // Resolver P₁(t₁) = ponto_inter para t₁
    // Usar componente não-zero de r1.direcao
    SE abs(r1.direcao.x) > 0.0001:
        t1 = (ponto_inter.x - r1.origem.x) / r1.direcao.x
    SENÃO SE abs(r1.direcao.y) > 0.0001:
        t1 = (ponto_inter.y - r1.origem.y) / r1.direcao.y
    SENÃO:
        t1 = (ponto_inter.z - r1.origem.z) / r1.direcao.z
    
    // Verificar se P₁(t₁) ≈ ponto_inter
    p_calculado = ponto_na_reta(r1, t1)
    dist = distancia_pontos(p_calculado, ponto_inter)
    
    SE dist < 0.0001:
        RETORNAR (VERDADEIRO, ponto_inter, t1, t2)
    SENÃO:
        RETORNAR (FALSO, NULL, 0, 0)  // Retas reversas
```

**💡 Insight:** Retas podem ser: paralelas, concorrentes (se cruzam) ou reversas (não paralelas e não se cruzam - só em 3D!).

**🔍 Checkpoint:** Reta `(0,0,0) + t(1,0,0)` e plano `x=5` se cruzam em `(5,0,0)` com `t=5`.

---

## 🎮 Aplicações Práticas

### 1. Ray Casting em Gráficos 3D

```
FUNÇÃO ray_cast(origem_camera, direcao_olhar, triangulos_cena):
    """Encontra primeiro objeto que o raio atinge"""
    raio = criar_reta(origem_camera, direcao_olhar)
    
    menor_t = INFINITO
    objeto_mais_proximo = NULL
    
    PARA CADA triangulo EM triangulos_cena:
        // Criar plano do triângulo
        plano_tri = criar_plano_de_tres_pontos(
            triangulo.v1,
            triangulo.v2,
            triangulo.v3
        )
        
        (tem_inter, ponto, t) = intersecao_reta_plano(raio, plano_tri)
        
        SE tem_inter E t > 0 E t < menor_t:
            // Verificar se ponto está dentro do triângulo
            SE ponto_dentro_triangulo(ponto, triangulo):
                menor_t = t
                objeto_mais_proximo = triangulo
    
    RETORNAR objeto_mais_proximo
```

**🎯 Uso:** Renderização, picking de objetos, física.

---

### 2. Navegação GPS: Ponto mais próximo em rota

```
FUNÇÃO ponto_mais_proximo_na_rota(posicao_usuario, segmentos_rota):
    """Encontra ponto mais próximo na rota (lista de segmentos de reta)"""
    menor_distancia = INFINITO
    ponto_mais_proximo = NULL
    
    PARA CADA segmento EM segmentos_rota:
        reta_seg = criar_reta_entre_pontos(segmento.inicio, segmento.fim)
        
        // Projetar usuário na reta do segmento
        // (simplificado: assumir reta infinita primeiro)
        d = distancia_ponto_reta(posicao_usuario, reta_seg)
        
        SE d < menor_distancia:
            // Calcular ponto de projeção
            // ... (requer cálculo de projeção)
            menor_distancia = d
    
    RETORNAR ponto_mais_proximo
```

**🎯 Uso:** Navegação, snap-to-road.

---

## 🧪 Testes Essenciais

```
FUNÇÃO testar_tudo():
    // Teste 1: Distância entre pontos
    p1 = criar_ponto(0, 0, 0)
    p2 = criar_ponto(3, 4, 0)
    ASSERT distancia_pontos(p1, p2) == 5.0
    
    // Teste 2: Reta paramétrica
    reta = criar_reta(criar_ponto(1, 1, 1), criar_ponto(1, 0, 0))
    p = ponto_na_reta(reta, 5)
    ASSERT p.x == 6 E p.y == 1 E p.z == 1
    
    // Teste 3: Distância ponto-reta (eixo X, ponto em Y)
    reta_x = criar_reta(criar_ponto(0,0,0), criar_ponto(1,0,0))
    p_y = criar_ponto(0, 3, 0)
    ASSERT distancia_ponto_reta(p_y, reta_x) == 3.0
    
    // Teste 4: Plano XY (z=0)
    plano_xy = criar_plano(criar_ponto(0,0,1), 0)  // n=(0,0,1), d=0
    p_acima = criar_ponto(0, 0, 7)
    ASSERT distancia_ponto_plano(p_acima, plano_xy) == 7.0
    
    // Teste 5: Interseção reta-plano
    reta_z = criar_reta(criar_ponto(0,0,0), criar_ponto(0,0,1))
    plano_z5 = criar_plano(criar_ponto(0,0,1), -5)  // z=5
    (tem, ponto, t) = intersecao_reta_plano(reta_z, plano_z5)
    ASSERT tem E ponto.z == 5 E t == 5
    
    IMPRIMIR "✅ Todos os testes passaram!"
```

---

## 📊 Complexidade

| Operação | Tempo | Espaço |
|----------|-------|--------|
| Distância ponto-ponto | O(1) | O(1) |
| Ponto na reta | O(1) | O(1) |
| Distância ponto-reta | O(1) | O(1) |
| Distância ponto-plano | O(1) | O(1) |
| Interseção reta-plano | O(1) | O(1) |
| Interseção reta-reta | O(1) | O(1) |

**Nota:** Todas operações geométricas básicas são O(1)!

---

## 🚀 Otimizações

1. **SIMD:** Processar 4 pontos simultaneamente (SSE/AVX)
2. **Cache de planos:** Pré-calcular planos de triângulos
3. **Bounding boxes:** Evitar interseções desnecessárias
4. **Tolerância adaptativa:** Ajustar epsilon baseado em escala

---

## 🔗 Próximos Passos

- `i2-distancias-angulos.md` → Cálculos avançados de distância e ângulos
- `k4-pratica/p1-basicos.md` → Problemas de programação
- Aplicação: Implementar ray tracer simples

---

## 📚 Referências

- Real-Time Rendering (Möller & Haines) - Cap. 16
- Geometric Tools for Computer Graphics (Schneider & Eberly)
- GLM (OpenGL Mathematics) - biblioteca C++ de referência
