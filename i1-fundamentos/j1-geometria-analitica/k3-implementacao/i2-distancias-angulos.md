# Implementação: Distâncias e Ângulos

## 🎯 Meta

Implementar cálculos avançados de distâncias (entre todos tipos de objetos) e ângulos (entre vetores, retas, planos), aplicando em problemas de navegação, visão computacional e física.

---

## ⏱️ Tempo Estimado

- **Leitura + Pseudocódigo:** 35-45 min
- **Implementação em linguagem real:** 1h15-1h45
- **Testes e debugging:** 25-35 min
- **Total:** ~2h15-3h

---

## 📋 Pré-requisitos

- `i1-pontos-retas-planos.md` implementado
- Leitura de `k1-teoria/t2-distancias-angulos.md`
- Exercícios `k2-exercicios/e2-distancias-angulos-exercicios.md` (Nível 1-2)

---

## 🎚️ Dificuldade

⭐⭐⭐ Intermediário

---

## 📐 Conceitos-Chave

1. **Distância ponto-reta:** Projeção perpendicular
2. **Distância reta-reta:** Paralelas vs reversas
3. **Ângulo entre vetores:** `cos θ = (u·v) / (||u|| ||v||)`
4. **Ângulo entre planos:** Ângulo entre normais
5. **Projeção:** Componente de vetor em direção dada

---

## 🧩 Estruturas Auxiliares

### Pseudocódigo

```
ESTRUTURA ResultadoDistancia:
    CAMPOS:
        distancia: número real
        ponto1: Ponto3D        // Ponto mais próximo no objeto 1
        ponto2: Ponto3D        // Ponto mais próximo no objeto 2
        parametro1: número real // Parâmetro t/s no objeto 1 (se aplicável)
        parametro2: número real // Parâmetro t/s no objeto 2 (se aplicável)

ESTRUTURA ResultadoAngulo:
    CAMPOS:
        radianos: número real
        graus: número real
        cosseno: número real
```

---

## 🛠️ Implementações

### 1. Projeções e Componentes

#### Pseudocódigo

```
FUNÇÃO produto_escalar(v1, v2):
    RETORNAR v1.x*v2.x + v1.y*v2.y + v1.z*v2.z

FUNÇÃO magnitude(v):
    RETORNAR raiz_quadrada(v.x² + v.y² + v.z²)

FUNÇÃO normalizar(v):
    mag = magnitude(v)
    SE mag < 0.0001:
        RETORNAR criar_ponto(0, 0, 0)
    RETORNAR criar_ponto(v.x/mag, v.y/mag, v.z/mag)

FUNÇÃO escalar_multiplicar(v, k):
    RETORNAR criar_ponto(k*v.x, k*v.y, k*v.z)

FUNÇÃO projecao_escalar(v, sobre_u):
    """Componente escalar de v na direção de u: proj_u(v) = (v·û)"""
    u_unit = normalizar(sobre_u)
    RETORNAR produto_escalar(v, u_unit)

FUNÇÃO projecao_vetorial(v, sobre_u):
    """Vetor projeção de v sobre u: proj_u(v) = ((v·u)/(u·u)) * u"""
    dot_vu = produto_escalar(v, sobre_u)
    dot_uu = produto_escalar(sobre_u, sobre_u)
    
    SE abs(dot_uu) < 0.0001:
        RETORNAR criar_ponto(0, 0, 0)
    
    fator = dot_vu / dot_uu
    RETORNAR escalar_multiplicar(sobre_u, fator)

FUNÇÃO componente_perpendicular(v, direcao):
    """Componente de v perpendicular a direção"""
    proj = projecao_vetorial(v, direcao)
    RETORNAR criar_ponto(
        v.x - proj.x,
        v.y - proj.y,
        v.z - proj.z
    )
```

**💡 Insight:** Projeção decompõe vetor em componente paralela e perpendicular - essencial para física e gráficos.

**🔍 Checkpoint:** `v=(3,4,0)` projetado em `u=(1,0,0)` dá `(3,0,0)` (componente x).

---

### 2. Distâncias Avançadas

#### Pseudocódigo

```
FUNÇÃO distancia_ponto_reta_detalhada(ponto, reta):
    """Retorna distância E ponto mais próximo na reta"""
    // Vetor da origem da reta ao ponto
    v = vetor_entre_pontos(reta.origem, ponto)
    
    // Projetar v na direção da reta
    proj = projecao_vetorial(v, reta.direcao)
    
    // Ponto mais próximo = origem + projeção
    ponto_mais_proximo = criar_ponto(
        reta.origem.x + proj.x,
        reta.origem.y + proj.y,
        reta.origem.z + proj.z
    )
    
    // Distância = magnitude do vetor perpendicular
    perp = componente_perpendicular(v, reta.direcao)
    dist = magnitude(perp)
    
    // Calcular parâmetro t
    mag_dir = magnitude(reta.direcao)
    t = magnitude(proj) / mag_dir
    
    // Ajustar sinal de t se proj e direção são opostos
    SE produto_escalar(proj, reta.direcao) < 0:
        t = -t
    
    RETORNAR ResultadoDistancia com:
        distancia = dist
        ponto1 = ponto
        ponto2 = ponto_mais_proximo
        parametro2 = t

FUNÇÃO distancia_ponto_segmento(ponto, p1, p2):
    """Distância de ponto a segmento de reta (limitado entre p1 e p2)"""
    // Criar reta infinita
    reta = criar_reta_entre_pontos(p1, p2)
    
    // Encontrar projeção
    resultado = distancia_ponto_reta_detalhada(ponto, reta)
    t = resultado.parametro2
    
    // Comprimento do segmento
    comprimento = distancia_pontos(p1, p2)
    t_max = comprimento / magnitude(reta.direcao)
    
    // Clampar t ao segmento [0, t_max]
    SE t < 0:
        resultado.ponto2 = p1
        resultado.distancia = distancia_pontos(ponto, p1)
        resultado.parametro2 = 0
    SENÃO SE t > t_max:
        resultado.ponto2 = p2
        resultado.distancia = distancia_pontos(ponto, p2)
        resultado.parametro2 = t_max
    
    RETORNAR resultado

FUNÇÃO distancia_entre_retas(r1, r2):
    """Distância entre duas retas no espaço"""
    // Se retas se cruzam, distância é 0
    (se_cruzam, ponto, t1, t2) = retas_se_intersectam(r1, r2)
    SE se_cruzam:
        RETORNAR ResultadoDistancia com:
            distancia = 0
            ponto1 = ponto
            ponto2 = ponto
            parametro1 = t1
            parametro2 = t2
    
    // Para retas reversas ou paralelas:
    // d = |((P₂ - P₁) · (d₁ × d₂))| / ||d₁ × d₂||
    
    produto_vetorial_dirs = produto_vetorial(r1.direcao, r2.direcao)
    mag_cruz = magnitude(produto_vetorial_dirs)
    
    // Se paralelas (produto vetorial ≈ 0)
    SE mag_cruz < 0.0001:
        // Distância = distância de ponto de r2 até r1
        RETORNAR distancia_ponto_reta_detalhada(r2.origem, r1)
    
    // Retas reversas
    vetor_entre_origens = vetor_entre_pontos(r1.origem, r2.origem)
    dist = abs(produto_escalar(vetor_entre_origens, produto_vetorial_dirs)) / mag_cruz
    
    // Encontrar pontos mais próximos (mais complexo, simplificado aqui)
    // ... (requer resolver sistema 2x2)
    
    RETORNAR ResultadoDistancia com:
        distancia = dist
        // ponto1 e ponto2 requerem cálculo adicional
```

**💡 Insight:** Retas reversas (skew lines) só existem em 3D+ dimensões - têm distância mínima mas não se cruzam.

**🔍 Checkpoint:** Retas `x=t, y=0, z=0` e `x=0, y=s, z=1` são reversas com distância 1.

---

### 3. Cálculo de Ângulos

#### Pseudocódigo

```
CONSTANTE PI = 3.14159265358979323846

FUNÇÃO graus_para_radianos(graus):
    RETORNAR graus * PI / 180.0

FUNÇÃO radianos_para_graus(rad):
    RETORNAR rad * 180.0 / PI

FUNÇÃO angulo_entre_vetores(v1, v2):
    """Calcula ângulo entre dois vetores
    Retorna ResultadoAngulo
    """
    // cos θ = (v1 · v2) / (||v1|| ||v2||)
    dot = produto_escalar(v1, v2)
    mag1 = magnitude(v1)
    mag2 = magnitude(v2)
    
    SE mag1 < 0.0001 OU mag2 < 0.0001:
        RETORNAR ResultadoAngulo com (0, 0, 1)  // Indefinido
    
    cos_theta = dot / (mag1 * mag2)
    
    // Clampar por erro numérico
    SE cos_theta > 1.0:
        cos_theta = 1.0
    SE cos_theta < -1.0:
        cos_theta = -1.0
    
    rad = arco_cosseno(cos_theta)
    graus = radianos_para_graus(rad)
    
    RETORNAR ResultadoAngulo com:
        radianos = rad
        graus = graus
        cosseno = cos_theta

FUNÇÃO angulo_entre_retas(r1, r2):
    """Ângulo entre direções de duas retas (menor ângulo)"""
    resultado = angulo_entre_vetores(r1.direcao, r2.direcao)
    
    // Garantir ângulo agudo (0° a 90°)
    SE resultado.radianos > PI/2:
        resultado.radianos = PI - resultado.radianos
        resultado.graus = 180 - resultado.graus
        resultado.cosseno = -resultado.cosseno
    
    RETORNAR resultado

FUNÇÃO angulo_entre_planos(plano1, plano2):
    """Ângulo entre dois planos (ângulo entre normais)"""
    RETORNAR angulo_entre_vetores(plano1.normal, plano2.normal)

FUNÇÃO angulo_entre_reta_e_plano(reta, plano):
    """Ângulo entre reta e plano
    θ = 90° - ângulo entre direção e normal
    """
    angulo_com_normal = angulo_entre_vetores(reta.direcao, plano.normal)
    
    // Complementar (90° - θ)
    rad = PI/2 - angulo_com_normal.radianos
    graus = 90 - angulo_com_normal.graus
    
    // sen θ do ângulo com plano = cos θ do ângulo com normal
    seno = angulo_com_normal.cosseno
    
    RETORNAR ResultadoAngulo com:
        radianos = rad
        graus = graus
        cosseno = raiz_quadrada(1 - seno²)
```

**💡 Insight:** Ângulo entre reta e plano é complementar ao ângulo entre reta e normal do plano.

**🔍 Checkpoint:** Vetores `(1,0,0)` e `(0,1,0)` têm ângulo de 90° (π/2 rad).

---

### 4. Ângulos Orientados (2D ou plano específico)

#### Pseudocódigo

```
FUNÇÃO angulo_orientado_2d(v1, v2):
    """Ângulo de v1 para v2 no plano XY (anti-horário positivo)
    Usa atan2 para ter sinal correto
    """
    // Calcular ângulos individuais
    angulo1 = arco_tangente2(v1.y, v1.x)
    angulo2 = arco_tangente2(v2.y, v2.x)
    
    // Diferença
    diff = angulo2 - angulo1
    
    // Normalizar para [-π, π]
    ENQUANTO diff > PI:
        diff = diff - 2*PI
    ENQUANTO diff < -PI:
        diff = diff + 2*PI
    
    RETORNAR ResultadoAngulo com:
        radianos = diff
        graus = radianos_para_graus(diff)
        cosseno = cosseno(diff)

FUNÇÃO angulo_orientado_3d(v1, v2, normal):
    """Ângulo de v1 para v2 em plano definido por normal
    Positivo segue regra da mão direita com normal
    """
    // Ângulo não orientado
    angulo_base = angulo_entre_vetores(v1, v2)
    
    // Determinar sinal com produto vetorial
    cruz = produto_vetorial(v1, v2)
    sinal = produto_escalar(cruz, normal)
    
    SE sinal < 0:
        angulo_base.radianos = -angulo_base.radianos
        angulo_base.graus = -angulo_base.graus
    
    RETORNAR angulo_base
```

**💡 Insight:** Ângulos orientados preservam informação de rotação (horário vs anti-horário), essencial para navegação e controle.

---

## 🎮 Aplicações Práticas

### 1. Sistema de Navegação: "Desvio da rota"

```
FUNÇÃO calcular_desvio_rota(posicao_atual, rota_planejada):
    """Calcula distância perpendicular à rota e ângulo de correção"""
    // rota_planejada é lista de segmentos
    
    // Encontrar segmento mais próximo
    menor_dist = INFINITO
    segmento_mais_proximo = NULL
    resultado_melhor = NULL
    
    PARA CADA seg EM rota_planejada:
        res = distancia_ponto_segmento(posicao_atual, seg.inicio, seg.fim)
        SE res.distancia < menor_dist:
            menor_dist = res.distancia
            segmento_mais_proximo = seg
            resultado_melhor = res
    
    // Calcular ângulo de correção
    // Direção da rota
    dir_rota = vetor_entre_pontos(segmento_mais_proximo.inicio, segmento_mais_proximo.fim)
    
    // Direção do usuário até rota
    dir_correcao = vetor_entre_pontos(posicao_atual, resultado_melhor.ponto2)
    
    angulo_correcao = angulo_orientado_2d(dir_rota, dir_correcao)
    
    RETORNAR:
        distancia_desvio = menor_dist
        angulo = angulo_correcao.graus
        ponto_volta_rota = resultado_melhor.ponto2
```

**🎯 Uso:** GPS, piloto automático.

---

### 2. Visão Computacional: Campo de visão

```
FUNÇÃO objeto_esta_no_campo_visao(pos_camera, dir_olhar, pos_objeto, angulo_fov):
    """Verifica se objeto está no cone de visão
    angulo_fov em graus (ex: 90° para FOV de 90°)
    """
    // Vetor da câmera ao objeto
    vetor_ao_objeto = vetor_entre_pontos(pos_camera, pos_objeto)
    
    // Ângulo entre direção de olhar e vetor ao objeto
    angulo = angulo_entre_vetores(dir_olhar, vetor_ao_objeto)
    
    // Objeto visível se ângulo < metade do FOV
    RETORNAR angulo.graus < (angulo_fov / 2.0)

FUNÇÃO calcular_direcao_para_objeto(pos_origem, pos_destino, frente_atual):
    """Calcula quanto deve rotacionar para olhar para objeto"""
    dir_para_destino = vetor_entre_pontos(pos_origem, pos_destino)
    dir_para_destino = normalizar(dir_para_destino)
    
    // Ângulo de rotação necessário (em plano XY para yaw)
    normal_up = criar_ponto(0, 0, 1)
    angulo_yaw = angulo_orientado_3d(frente_atual, dir_para_destino, normal_up)
    
    RETORNAR angulo_yaw.graus
```

**🎯 Uso:** IA de jogos, frustum culling, detecção de alvos.

---

### 3. Física: Reflexão

```
FUNÇÃO refletir_vetor(vetor_incidente, normal_superficie):
    """Reflexão de vetor em superfície (lei da reflexão)
    R = V - 2(V·N)N onde N é normal unitária
    """
    normal_unit = normalizar(normal_superficie)
    
    dot = produto_escalar(vetor_incidente, normal_unit)
    fator = 2.0 * dot
    
    termo_reflexao = escalar_multiplicar(normal_unit, fator)
    
    RETORNAR criar_ponto(
        vetor_incidente.x - termo_reflexao.x,
        vetor_incidente.y - termo_reflexao.y,
        vetor_incidente.z - termo_reflexao.z
    )

FUNÇÃO calcular_angulo_incidencia(raio_incidente, normal_superficie):
    """Ângulo entre raio e normal (importante para lei de Snell)"""
    angulo = angulo_entre_vetores(raio_incidente, normal_superficie)
    
    // Ângulo de incidência é medido da normal
    // Se > 90°, vetor está "entrando" na superfície
    SE angulo.graus > 90:
        angulo.radianos = PI - angulo.radianos
        angulo.graus = 180 - angulo.graus
    
    RETORNAR angulo
```

**🎯 Uso:** Ray tracing, simulação de luz, bilhar.

---

## 🧪 Testes Essenciais

```
FUNÇÃO testar_distancias_angulos():
    epsilon = 0.0001
    
    // Teste 1: Projeção
    v = criar_ponto(3, 4, 0)
    u = criar_ponto(1, 0, 0)
    proj = projecao_vetorial(v, u)
    ASSERT abs(proj.x - 3.0) < epsilon E abs(proj.y) < epsilon
    
    // Teste 2: Distância ponto-segmento (projeção fora do segmento)
    p1 = criar_ponto(0, 0, 0)
    p2 = criar_ponto(1, 0, 0)
    ponto_teste = criar_ponto(2, 1, 0)
    resultado = distancia_ponto_segmento(ponto_teste, p1, p2)
    ASSERT abs(resultado.distancia - raiz_quadrada(2)) < epsilon
    ASSERT distancia_pontos(resultado.ponto2, p2) < epsilon  // Mais próximo é p2
    
    // Teste 3: Ângulo entre vetores perpendiculares
    v1 = criar_ponto(1, 0, 0)
    v2 = criar_ponto(0, 1, 0)
    angulo = angulo_entre_vetores(v1, v2)
    ASSERT abs(angulo.graus - 90.0) < epsilon
    ASSERT abs(angulo.cosseno) < epsilon
    
    // Teste 4: Ângulo entre vetores paralelos
    v3 = criar_ponto(2, 0, 0)
    v4 = criar_ponto(5, 0, 0)
    angulo2 = angulo_entre_vetores(v3, v4)
    ASSERT abs(angulo2.graus) < epsilon
    
    // Teste 5: Reflexão (45° em parede vertical)
    incidente = normalizar(criar_ponto(1, 1, 0))
    normal_parede = criar_ponto(0, 1, 0)
    refletido = refletir_vetor(incidente, normal_parede)
    esperado = normalizar(criar_ponto(1, -1, 0))
    ASSERT distancia_pontos(refletido, esperado) < epsilon
    
    IMPRIMIR "✅ Todos os testes de distâncias e ângulos passaram!"
```

---

## 📊 Complexidade

| Operação | Tempo | Notas |
|----------|-------|-------|
| Projeção vetorial | O(1) | Produto escalar + divisão |
| Distância ponto-reta | O(1) | Projeção + magnitude |
| Distância reta-reta | O(1) | Caso especial: O(1), geral pode requerer solver |
| Ângulo entre vetores | O(1) | Produto escalar + acos |
| Reflexão | O(1) | Produto escalar + subtração |

---

## 🚀 Otimizações

1. **Evitar sqrt quando possível:** Comparar `dist²` em vez de `dist`
2. **Tabelas de acos:** Para ângulos aproximados (jogos)
3. **Fast inverse sqrt:** Truque clássico para normalização rápida
4. **Batch processing:** SIMD para múltiplos cálculos

```
// Exemplo: Comparar distâncias sem sqrt
FUNÇÃO ponto_mais_proximo_evitando_sqrt(ponto, lista_candidatos):
    menor_dist_quadrada = INFINITO
    mais_proximo = NULL
    
    PARA CADA candidato EM lista_candidatos:
        dx = candidato.x - ponto.x
        dy = candidato.y - ponto.y
        dz = candidato.z - ponto.z
        dist_quadrada = dx² + dy² + dz²
        
        SE dist_quadrada < menor_dist_quadrada:
            menor_dist_quadrada = dist_quadrada
            mais_proximo = candidato
    
    RETORNAR mais_proximo
```

---

## 🔗 Próximos Passos

- `i3-conicas-superficies.md` → Geometria de curvas e superfícies
- `k4-pratica/p2-intermediarios.md` → Problemas de navegação e ray tracing
- Aplicação: Sistema de colisão 3D

---

## 📚 Referências

- Mathematics for 3D Game Programming (Lengyel) - Cap. 3-4
- Real-Time Collision Detection (Ericson) - Cap. 5
- GLM documentation - `glm::angle`, `glm::reflect`
