# Implementação: Cônicas e Superfícies Quádricas

## 🎯 Meta

Implementar representações e operações com cônicas (elipses, hipérboles, parábolas) e superfícies quádricas (esferas, cilindros, cones), aplicando em gráficos 3D, física e modelagem.

---

## ⏱️ Tempo Estimado

- **Leitura + Pseudocódigo:** 40-50 min
- **Implementação em linguagem real:** 1h30-2h
- **Testes e debugging:** 30-40 min
- **Total:** ~2h40-3h30

---

## 📋 Pré-requisitos

- `i1-pontos-retas-planos.md` e `i2-distancias-angulos.md` implementados
- Leitura de `k1-teoria/t3-conicas-superficies.md`
- Exercícios `k2-exercicios/e3-conicas-superficies-exercicios.md` (Nível 1-2)

---

## 🎚️ Dificuldade

⭐⭐⭐⭐ Intermediário-Avançado

---

## 📐 Conceitos-Chave

1. **Cônicas:** Curvas de 2º grau (círculo, elipse, parábola, hipérbole)
2. **Superfícies Quádricas:** Superfícies de 2º grau (esfera, elipsoide, paraboloide, hiperboloide, cone, cilindro)
3. **Equação implícita:** `Ax² + By² + Cz² + ... = 0`
4. **Equação paramétrica:** `(x(t), y(t), z(t))`
5. **Interseção raio-superfície:** Resolver equação quadrática

---

## 🧩 Estrutura de Dados

### Pseudocódigo

```
ESTRUTURA Esfera:
    CAMPOS:
        centro: Ponto3D
        raio: número real

ESTRUTURA Cilindro:
    CAMPOS:
        base: Ponto3D         // Ponto na base
        eixo: Ponto3D         // Direção do eixo (vetor unitário)
        raio: número real
        altura: número real   // -1 para infinito

ESTRUTURA Cone:
    CAMPOS:
        apice: Ponto3D        // Ponto do vértice
        eixo: Ponto3D         // Direção (vetor unitário)
        angulo: número real   // Ângulo de abertura (radianos)
        altura: número real   // -1 para infinito

ESTRUTURA Plano:
    CAMPOS:
        normal: Ponto3D
        d: número real

ESTRUTURA ResultadoIntersecao:
    CAMPOS:
        tem_intersecao: booleano
        num_pontos: inteiro   // 0, 1, 2 ou infinito
        ponto1: Ponto3D
        ponto2: Ponto3D
        t1: número real       // Parâmetro na reta
        t2: número real
```

---

## 🛠️ Implementações

### 1. Esfera

#### Pseudocódigo

```
FUNÇÃO criar_esfera(centro, raio):
    RETORNAR Esfera com (centro, raio)

FUNÇÃO ponto_esta_na_esfera(ponto, esfera, epsilon=0.0001):
    """Verifica se ponto está na superfície da esfera"""
    dist = distancia_pontos(ponto, esfera.centro)
    RETORNAR abs(dist - esfera.raio) < epsilon

FUNÇÃO ponto_esta_dentro_esfera(ponto, esfera):
    """Verifica se ponto está dentro (ou na superfície)"""
    dist = distancia_pontos(ponto, esfera.centro)
    RETORNAR dist <= esfera.raio

FUNÇÃO intersecao_raio_esfera(origem_raio, direcao_raio, esfera):
    """Interseção raio-esfera
    Raio: P(t) = O + t*D
    Esfera: ||P - C||² = r²
    Substituindo: ||O + t*D - C||² = r²
    """
    // Vetor da origem do raio ao centro da esfera
    oc = vetor_entre_pontos(esfera.centro, origem_raio)
    
    // Coeficientes da equação quadrática: at² + bt + c = 0
    // a = D·D (sempre 1 se D normalizado, mas vamos calcular)
    a = produto_escalar(direcao_raio, direcao_raio)
    b = 2.0 * produto_escalar(oc, direcao_raio)
    c = produto_escalar(oc, oc) - esfera.raio * esfera.raio
    
    // Discriminante
    discriminante = b*b - 4*a*c
    
    // Sem interseção
    SE discriminante < 0:
        RETORNAR ResultadoIntersecao com:
            tem_intersecao = FALSO
            num_pontos = 0
    
    // Uma interseção (tangente)
    SE abs(discriminante) < 0.0001:
        t = -b / (2*a)
        ponto = criar_ponto(
            origem_raio.x + t * direcao_raio.x,
            origem_raio.y + t * direcao_raio.y,
            origem_raio.z + t * direcao_raio.z
        )
        RETORNAR ResultadoIntersecao com:
            tem_intersecao = VERDADEIRO
            num_pontos = 1
            ponto1 = ponto
            t1 = t
    
    // Duas interseções
    sqrt_disc = raiz_quadrada(discriminante)
    t1 = (-b - sqrt_disc) / (2*a)
    t2 = (-b + sqrt_disc) / (2*a)
    
    ponto1 = criar_ponto(
        origem_raio.x + t1 * direcao_raio.x,
        origem_raio.y + t1 * direcao_raio.y,
        origem_raio.z + t1 * direcao_raio.z
    )
    
    ponto2 = criar_ponto(
        origem_raio.x + t2 * direcao_raio.x,
        origem_raio.y + t2 * direcao_raio.y,
        origem_raio.z + t2 * direcao_raio.z
    )
    
    RETORNAR ResultadoIntersecao com:
        tem_intersecao = VERDADEIRO
        num_pontos = 2
        ponto1 = ponto1
        ponto2 = ponto2
        t1 = t1
        t2 = t2

FUNÇÃO normal_esfera_no_ponto(ponto, esfera):
    """Normal unitária à esfera no ponto (aponta para fora)"""
    normal = vetor_entre_pontos(esfera.centro, ponto)
    RETORNAR normalizar(normal)
```

**💡 Insight:** Interseção raio-esfera é equação quadrática clássica - discriminante determina 0, 1 ou 2 soluções.

**🔍 Checkpoint:** Raio de `(0,0,0)` na direção `(1,0,0)` intersecta esfera em `(2,0,0)` de raio 1 em dois pontos: `(1,0,0)` e `(3,0,0)`.

---

### 2. Cilindro Infinito

#### Pseudocódigo

```
FUNÇÃO criar_cilindro(base, eixo, raio, altura=-1):
    eixo_normalizado = normalizar(eixo)
    RETORNAR Cilindro com (base, eixo_normalizado, raio, altura)

FUNÇÃO intersecao_raio_cilindro_infinito(origem_raio, direcao_raio, cilindro):
    """Interseção com cilindro infinito alinhado ao eixo
    Cilindro: ||P - (P·a)a - C||² = r²
    onde a é o eixo unitário, C é ponto no eixo
    """
    // Simplificação: componente perpendicular ao eixo deve ter distância r do eixo
    
    // Vetor da base do cilindro à origem do raio
    oc = vetor_entre_pontos(cilindro.base, origem_raio)
    
    // Componente da direção perpendicular ao eixo
    d_perp = componente_perpendicular(direcao_raio, cilindro.eixo)
    
    // Componente de oc perpendicular ao eixo
    oc_perp = componente_perpendicular(oc, cilindro.eixo)
    
    // Equação quadrática: ||oc_perp + t * d_perp||² = r²
    // Expandindo: (oc_perp + t*d_perp)·(oc_perp + t*d_perp) = r²
    // t²(d_perp·d_perp) + 2t(oc_perp·d_perp) + (oc_perp·oc_perp - r²) = 0
    
    a = produto_escalar(d_perp, d_perp)
    b = 2.0 * produto_escalar(oc_perp, d_perp)
    c = produto_escalar(oc_perp, oc_perp) - cilindro.raio * cilindro.raio
    
    discriminante = b*b - 4*a*c
    
    SE discriminante < 0:
        RETORNAR ResultadoIntersecao com:
            tem_intersecao = FALSO
            num_pontos = 0
    
    SE abs(a) < 0.0001:
        // Raio paralelo ao eixo
        SE abs(c) < 0.0001:
            // Raio está na superfície
            RETORNAR ResultadoIntersecao com:
                tem_intersecao = VERDADEIRO
                num_pontos = -1  // Infinitos
        SENÃO:
            RETORNAR ResultadoIntersecao com:
                tem_intersecao = FALSO
                num_pontos = 0
    
    sqrt_disc = raiz_quadrada(discriminante)
    t1 = (-b - sqrt_disc) / (2*a)
    t2 = (-b + sqrt_disc) / (2*a)
    
    ponto1 = criar_ponto(
        origem_raio.x + t1 * direcao_raio.x,
        origem_raio.y + t1 * direcao_raio.y,
        origem_raio.z + t1 * direcao_raio.z
    )
    
    ponto2 = criar_ponto(
        origem_raio.x + t2 * direcao_raio.x,
        origem_raio.y + t2 * direcao_raio.y,
        origem_raio.z + t2 * direcao_raio.z
    )
    
    RETORNAR ResultadoIntersecao com:
        tem_intersecao = VERDADEIRO
        num_pontos = 2
        ponto1 = ponto1
        ponto2 = ponto2
        t1 = t1
        t2 = t2

FUNÇÃO normal_cilindro_no_ponto(ponto, cilindro):
    """Normal ao cilindro no ponto (perpendicular ao eixo)"""
    // Projetar ponto no eixo
    vetor_base_ponto = vetor_entre_pontos(cilindro.base, ponto)
    proj = projecao_vetorial(vetor_base_ponto, cilindro.eixo)
    
    ponto_no_eixo = criar_ponto(
        cilindro.base.x + proj.x,
        cilindro.base.y + proj.y,
        cilindro.base.z + proj.z
    )
    
    // Normal = vetor do eixo ao ponto
    normal = vetor_entre_pontos(ponto_no_eixo, ponto)
    RETORNAR normalizar(normal)
```

**💡 Insight:** Cilindro é lugar geométrico de pontos a distância fixa de um eixo - decompomos problema em componente perpendicular.

---

### 3. Cone Infinito

#### Pseudocódigo

```
FUNÇÃO criar_cone(apice, eixo, angulo, altura=-1):
    eixo_normalizado = normalizar(eixo)
    RETORNAR Cone com (apice, eixo_normalizado, angulo, altura)

FUNÇÃO intersecao_raio_cone_infinito(origem_raio, direcao_raio, cone):
    """Interseção raio-cone duplo infinito
    Cone: (V·a)² = cos²(θ) * ||V||²
    onde V = P - A (vetor do ápice ao ponto), a = eixo unitário
    """
    // Vetor do ápice à origem do raio
    va = vetor_entre_pontos(cone.apice, origem_raio)
    
    // Pré-calcular
    cos_theta = cosseno(cone.angulo)
    cos2 = cos_theta * cos_theta
    
    // Componentes
    va_dot_eixo = produto_escalar(va, cone.eixo)
    d_dot_eixo = produto_escalar(direcao_raio, cone.eixo)
    va_dot_d = produto_escalar(va, direcao_raio)
    va_dot_va = produto_escalar(va, va)
    d_dot_d = produto_escalar(direcao_raio, direcao_raio)
    
    // Coeficientes da quadrática
    a = d_dot_eixo*d_dot_eixo - cos2*d_dot_d
    b = 2*(va_dot_eixo*d_dot_eixo - cos2*va_dot_d)
    c = va_dot_eixo*va_dot_eixo - cos2*va_dot_va
    
    discriminante = b*b - 4*a*c
    
    SE discriminante < 0:
        RETORNAR ResultadoIntersecao com:
            tem_intersecao = FALSO
            num_pontos = 0
    
    SE abs(a) < 0.0001:
        // Caso degenerado
        SE abs(b) > 0.0001:
            t = -c / b
            ponto = criar_ponto(
                origem_raio.x + t * direcao_raio.x,
                origem_raio.y + t * direcao_raio.y,
                origem_raio.z + t * direcao_raio.z
            )
            RETORNAR ResultadoIntersecao com:
                tem_intersecao = VERDADEIRO
                num_pontos = 1
                ponto1 = ponto
                t1 = t
        SENÃO:
            RETORNAR ResultadoIntersecao com:
                tem_intersecao = FALSO
                num_pontos = 0
    
    sqrt_disc = raiz_quadrada(discriminante)
    t1 = (-b - sqrt_disc) / (2*a)
    t2 = (-b + sqrt_disc) / (2*a)
    
    ponto1 = criar_ponto(
        origem_raio.x + t1 * direcao_raio.x,
        origem_raio.y + t1 * direcao_raio.y,
        origem_raio.z + t1 * direcao_raio.z
    )
    
    ponto2 = criar_ponto(
        origem_raio.x + t2 * direcao_raio.x,
        origem_raio.y + t2 * direcao_raio.y,
        origem_raio.z + t2 * direcao_raio.z
    )
    
    RETORNAR ResultadoIntersecao com:
        tem_intersecao = VERDADEIRO
        num_pontos = 2
        ponto1 = ponto1
        ponto2 = ponto2
        t1 = t1
        t2 = t2
```

**💡 Insight:** Cone é definido por ângulo entre eixo e geratriz - equação vem de relação trigonométrica.

---

### 4. Cônicas 2D (Elipse)

#### Pseudocódigo

```
ESTRUTURA Elipse:
    CAMPOS:
        centro: Ponto3D  // (x, y, 0) para 2D
        semi_eixo_a: número real  // Semi-eixo maior
        semi_eixo_b: número real  // Semi-eixo menor
        rotacao: número real      // Ângulo de rotação (radianos)

FUNÇÃO criar_elipse(centro, a, b, rotacao=0):
    RETORNAR Elipse com (centro, a, b, rotacao)

FUNÇÃO ponto_esta_na_elipse(ponto, elipse, epsilon=0.0001):
    """Verifica se ponto está na elipse
    Equação: (x/a)² + (y/b)² = 1 (sem rotação)
    Com rotação: aplicar transformação inversa
    """
    // Transladar para origem
    px = ponto.x - elipse.centro.x
    py = ponto.y - elipse.centro.y
    
    // Rotacionar para eixos canônicos (rotação inversa)
    cos_r = cosseno(-elipse.rotacao)
    sen_r = seno(-elipse.rotacao)
    
    px_rot = px*cos_r - py*sen_r
    py_rot = px*sen_r + py*cos_r
    
    // Verificar equação
    valor = (px_rot/elipse.semi_eixo_a)² + (py_rot/elipse.semi_eixo_b)²
    
    RETORNAR abs(valor - 1.0) < epsilon

FUNÇÃO ponto_parametrico_elipse(elipse, t):
    """Ponto na elipse usando parâmetro t ∈ [0, 2π]
    Forma paramétrica: (a*cos(t), b*sen(t)) + rotação + translação
    """
    // Ponto no sistema canônico
    x_can = elipse.semi_eixo_a * cosseno(t)
    y_can = elipse.semi_eixo_b * seno(t)
    
    // Rotacionar
    cos_r = cosseno(elipse.rotacao)
    sen_r = seno(elipse.rotacao)
    
    x_rot = x_can*cos_r - y_can*sen_r
    y_rot = x_can*sen_r + y_can*cos_r
    
    // Transladar
    RETORNAR criar_ponto(
        elipse.centro.x + x_rot,
        elipse.centro.y + y_rot,
        0
    )

FUNÇÃO gerar_pontos_elipse(elipse, num_pontos=100):
    """Gera lista de pontos para renderização"""
    pontos = LISTA_VAZIA
    
    PARA i DE 0 ATÉ num_pontos-1:
        t = 2 * PI * i / num_pontos
        ponto = ponto_parametrico_elipse(elipse, t)
        ADICIONAR ponto A pontos
    
    RETORNAR pontos
```

**🔍 Checkpoint:** Elipse com `a=5, b=3` centrada em origem tem ponto `(5,0,0)` em `t=0` e `(0,3,0)` em `t=π/2`.

---

## 🎮 Aplicações Práticas

### 1. Ray Tracing: Cena com múltiplos objetos

```
FUNÇÃO renderizar_cena(origem_camera, direcao_pixel, cena):
    """Encontra primeiro objeto atingido pelo raio"""
    raio_origem = origem_camera
    raio_dir = normalizar(direcao_pixel)
    
    objeto_mais_proximo = NULL
    t_mais_proximo = INFINITO
    ponto_hit = NULL
    
    // Testar esferas
    PARA CADA esfera EM cena.esferas:
        resultado = intersecao_raio_esfera(raio_origem, raio_dir, esfera)
        
        SE resultado.tem_intersecao:
            // Pegar t positivo mais próximo
            SE resultado.t1 > 0 E resultado.t1 < t_mais_proximo:
                t_mais_proximo = resultado.t1
                objeto_mais_proximo = esfera
                ponto_hit = resultado.ponto1
            
            SE resultado.num_pontos == 2 E resultado.t2 > 0 E resultado.t2 < t_mais_proximo:
                t_mais_proximo = resultado.t2
                objeto_mais_proximo = esfera
                ponto_hit = resultado.ponto2
    
    // Testar cilindros
    PARA CADA cilindro EM cena.cilindros:
        resultado = intersecao_raio_cilindro_infinito(raio_origem, raio_dir, cilindro)
        // ... similar
    
    // Calcular cor baseado em normal
    SE objeto_mais_proximo != NULL:
        normal = calcular_normal(objeto_mais_proximo, ponto_hit)
        cor = calcular_iluminacao(ponto_hit, normal, cena.luzes)
        RETORNAR cor
    SENÃO:
        RETORNAR cor_fundo
```

**🎯 Uso:** Ray tracing, sombras, reflexões.

---

### 2. Física: Detecção de colisão esfera-esfera

```
FUNÇÃO esferas_colidem(esfera1, esfera2):
    """Verifica colisão entre duas esferas"""
    dist = distancia_pontos(esfera1.centro, esfera2.centro)
    soma_raios = esfera1.raio + esfera2.raio
    
    RETORNAR dist <= soma_raios

FUNÇÃO calcular_resposta_colisao(esfera1, vel1, esfera2, vel2):
    """Calcula velocidades após colisão elástica"""
    // Normal de colisão
    normal = vetor_entre_pontos(esfera1.centro, esfera2.centro)
    normal = normalizar(normal)
    
    // Velocidades relativas ao longo da normal
    vel_rel = criar_ponto(
        vel1.x - vel2.x,
        vel1.y - vel2.y,
        vel1.z - vel2.z
    )
    
    vel_ao_longo_normal = produto_escalar(vel_rel, normal)
    
    // Se afastando, ignorar
    SE vel_ao_longo_normal > 0:
        RETORNAR (vel1, vel2)
    
    // Impulso (simplificado: massas iguais)
    impulso = escalar_multiplicar(normal, vel_ao_longo_normal)
    
    nova_vel1 = criar_ponto(
        vel1.x - impulso.x,
        vel1.y - impulso.y,
        vel1.z - impulso.z
    )
    
    nova_vel2 = criar_ponto(
        vel2.x + impulso.x,
        vel2.y + impulso.y,
        vel2.z + impulso.z
    )
    
    RETORNAR (nova_vel1, nova_vel2)
```

**🎯 Uso:** Jogos, simulações físicas.

---

## 🧪 Testes Essenciais

```
FUNÇÃO testar_conicas_superficies():
    epsilon = 0.001
    
    // Teste 1: Raio atinge esfera
    esfera = criar_esfera(criar_ponto(5, 0, 0), 2)
    origem = criar_ponto(0, 0, 0)
    direcao = criar_ponto(1, 0, 0)
    resultado = intersecao_raio_esfera(origem, direcao, esfera)
    
    ASSERT resultado.tem_intersecao
    ASSERT resultado.num_pontos == 2
    ASSERT abs(resultado.t1 - 3.0) < epsilon  // Entra em t=3
    ASSERT abs(resultado.t2 - 7.0) < epsilon  // Sai em t=7
    
    // Teste 2: Raio tangente à esfera
    origem2 = criar_ponto(0, 2, 0)
    direcao2 = criar_ponto(1, 0, 0)
    resultado2 = intersecao_raio_esfera(origem2, direcao2, esfera)
    
    ASSERT resultado2.tem_intersecao
    ASSERT resultado2.num_pontos == 1
    ASSERT abs(resultado2.t1 - 5.0) < epsilon
    
    // Teste 3: Raio não atinge esfera
    origem3 = criar_ponto(0, 5, 0)
    resultado3 = intersecao_raio_esfera(origem3, direcao, esfera)
    ASSERT NÃO resultado3.tem_intersecao
    
    // Teste 4: Ponto na elipse
    elipse = criar_elipse(criar_ponto(0, 0, 0), 5, 3, 0)
    ponto_elipse = criar_ponto(5, 0, 0)
    ASSERT ponto_esta_na_elipse(ponto_elipse, elipse)
    
    // Teste 5: Colisão esferas
    esfera1 = criar_esfera(criar_ponto(0, 0, 0), 1)
    esfera2 = criar_esfera(criar_ponto(1.5, 0, 0), 1)
    ASSERT esferas_colidem(esfera1, esfera2)
    
    esfera3 = criar_esfera(criar_ponto(5, 0, 0), 1)
    ASSERT NÃO esferas_colidem(esfera1, esfera3)
    
    IMPRIMIR "✅ Todos os testes de cônicas e superfícies passaram!"
```

---

## 📊 Complexidade

| Operação | Tempo | Notas |
|----------|-------|-------|
| Interseção raio-esfera | O(1) | Resolver quadrática |
| Interseção raio-cilindro | O(1) | Resolver quadrática |
| Interseção raio-cone | O(1) | Resolver quadrática |
| Colisão esfera-esfera | O(1) | Comparar distâncias |
| Gerar pontos elipse | O(n) | n = número de pontos |

---

## 🚀 Otimizações

1. **Bounding spheres:** Testar esfera envolvente antes de geometria complexa
2. **Early out:** Ordenar testes por probabilidade de hit
3. **SIMD quadrática:** Resolver múltiplas equações simultaneamente
4. **Tabelas pré-calculadas:** sen/cos para elipses

```
// Exemplo: Otimização com bounding sphere
FUNÇÃO intersecao_otimizada(raio_origem, raio_dir, objeto_complexo):
    // Teste rápido com esfera envolvente
    SE NÃO intersecao_raio_esfera(raio_origem, raio_dir, objeto_complexo.bounding_sphere).tem_intersecao:
        RETORNAR sem_intersecao
    
    // Só agora fazer teste complexo
    RETORNAR intersecao_raio_malha(raio_origem, raio_dir, objeto_complexo.malha)
```

---

## 🔗 Próximos Passos

- `i4-coordenadas-transformacoes.md` → Sistemas de coordenadas e transformações
- `k4-pratica/p3-avancados.md` → Ray tracer completo
- `k5-projeto/` → Sistema de navegação com colisões

---

## 📚 Referências

- Real-Time Rendering (Möller & Haines) - Cap. 22 (Intersection)
- Ray Tracing in One Weekend (Shirley)
- Scratchapixel - "Geometry" section
- pbrt (Physically Based Rendering) - Shapes chapter
