# Implementação: Coordenadas e Transformações

## 🎯 Meta

Implementar sistemas de coordenadas (cartesianas, polares, esféricas, cilíndricas) e transformações geométricas (translação, rotação, escala, cisalhamento) usando matrizes, aplicando em gráficos 3D, robótica e navegação.

---

## ⏱️ Tempo Estimado

- **Leitura + Pseudocódigo:** 45-55 min
- **Implementação em linguagem real:** 2h-2h30
- **Testes e debugging:** 30-40 min
- **Total:** ~3h15-4h

---

## 📋 Pré-requisitos

- Todos os módulos anteriores de implementação
- Leitura de `k1-teoria/t4-coordenadas-transformacoes.md`
- Exercícios `k2-exercicios/e4-coordenadas-transformacoes-exercicios.md` (Nível 1-2)
- Conhecimento básico de matrizes (de álgebra linear)

---

## 🎚️ Dificuldade

⭐⭐⭐⭐ Avançado

---

## 📐 Conceitos-Chave

1. **Sistemas de coordenadas:** Diferentes formas de representar posição
2. **Transformações afins:** Preservam paralelismo (translação, rotação, escala)
3. **Matrizes de transformação:** Representação matricial 4×4 (coordenadas homogêneas)
4. **Composição:** Multiplicar matrizes para combinar transformações
5. **Inversas:** Reverter transformações

---

## 🧩 Estrutura de Dados

### Pseudocódigo

```
ESTRUTURA Matriz4x4:
    CAMPOS:
        m: array[4][4] de números reais
        // m[linha][coluna]

ESTRUTURA CoordenadasPolaresestado2D:
    CAMPOS:
        r: número real      // Raio (distância da origem)
        theta: número real  // Ângulo (radianos)

ESTRUTURA CoordenadasEsfericas:
    CAMPOS:
        rho: número real    // Distância radial
        theta: número real  // Azimute (ângulo no plano XY)
        phi: número real    // Ângulo polar (do eixo Z)

ESTRUTURA CoordenadasCilindricas:
    CAMPOS:
        r: número real      // Distância radial do eixo Z
        theta: número real  // Azimute
        z: número real      // Altura

ESTRUTURA Transformacao:
    CAMPOS:
        matriz: Matriz4x4
        inversa: Matriz4x4  // Cache da inversa
```

---

## 🛠️ Implementações

### 1. Conversões de Coordenadas

#### Pseudocódigo

```
CONSTANTE PI = 3.14159265358979323846

// ========== 2D: CARTESIANAS ↔ POLARES ==========

FUNÇÃO cartesianas_para_polares(x, y):
    """Converte (x, y) para (r, θ)"""
    r = raiz_quadrada(x² + y²)
    theta = arco_tangente2(y, x)  // atan2 lida com todos os quadrantes
    
    RETORNAR CoordenadasPolares2D com (r, theta)

FUNÇÃO polares_para_cartesianas(r, theta):
    """Converte (r, θ) para (x, y)"""
    x = r * cosseno(theta)
    y = r * seno(theta)
    
    RETORNAR criar_ponto(x, y, 0)

// ========== 3D: CARTESIANAS ↔ ESFÉRICAS ==========

FUNÇÃO cartesianas_para_esfericas(x, y, z):
    """Converte (x, y, z) para (ρ, θ, φ)
    ρ = distância da origem
    θ = azimute (ângulo no plano XY de X)
    φ = ângulo polar (do eixo Z positivo)
    """
    rho = raiz_quadrada(x² + y² + z²)
    
    SE rho < 0.0001:
        // Origem
        RETORNAR CoordenadasEsfericas com (0, 0, 0)
    
    theta = arco_tangente2(y, x)
    phi = arco_cosseno(z / rho)  // φ ∈ [0, π]
    
    RETORNAR CoordenadasEsfericas com (rho, theta, phi)

FUNÇÃO esfericas_para_cartesianas(rho, theta, phi):
    """Converte (ρ, θ, φ) para (x, y, z)"""
    x = rho * seno(phi) * cosseno(theta)
    y = rho * seno(phi) * seno(theta)
    z = rho * cosseno(phi)
    
    RETORNAR criar_ponto(x, y, z)

// ========== 3D: CARTESIANAS ↔ CILÍNDRICAS ==========

FUNÇÃO cartesianas_para_cilindricas(x, y, z):
    """Converte (x, y, z) para (r, θ, z)"""
    r = raiz_quadrada(x² + y²)
    theta = arco_tangente2(y, x)
    
    RETORNAR CoordenadasCilindricas com (r, theta, z)

FUNÇÃO cilindricas_para_cartesianas(r, theta, z):
    """Converte (r, θ, z) para (x, y, z)"""
    x = r * cosseno(theta)
    y = r * seno(theta)
    
    RETORNAR criar_ponto(x, y, z)
```

**💡 Insight:** `atan2(y, x)` é preferível a `atan(y/x)` porque determina quadrante correto e evita divisão por zero.

**🔍 Checkpoint:** Ponto `(1, 1, 0)` em polares é `(√2, π/4)`. Ponto `(1, 0, 1)` em esféricas é `(√2, 0, π/4)`.

---

### 2. Matrizes 4×4 e Operações Básicas

#### Pseudocódigo

```
FUNÇÃO criar_matriz_identidade():
    """Matriz identidade 4×4"""
    mat = Matriz4x4
    
    PARA i DE 0 ATÉ 3:
        PARA j DE 0 ATÉ 3:
            SE i == j:
                mat.m[i][j] = 1.0
            SENÃO:
                mat.m[i][j] = 0.0
    
    RETORNAR mat

FUNÇÃO multiplicar_matrizes(A, B):
    """Multiplica duas matrizes 4×4: C = A × B"""
    C = Matriz4x4
    
    PARA i DE 0 ATÉ 3:
        PARA j DE 0 ATÉ 3:
            soma = 0.0
            PARA k DE 0 ATÉ 3:
                soma = soma + A.m[i][k] * B.m[k][j]
            C.m[i][j] = soma
    
    RETORNAR C

FUNÇÃO transformar_ponto(matriz, ponto):
    """Aplica transformação a ponto (coordenadas homogêneas)
    [x', y', z', w'] = Matriz × [x, y, z, 1]
    """
    x = matriz.m[0][0]*ponto.x + matriz.m[0][1]*ponto.y + matriz.m[0][2]*ponto.z + matriz.m[0][3]
    y = matriz.m[1][0]*ponto.x + matriz.m[1][1]*ponto.y + matriz.m[1][2]*ponto.z + matriz.m[1][3]
    z = matriz.m[2][0]*ponto.x + matriz.m[2][1]*ponto.y + matriz.m[2][2]*ponto.z + matriz.m[2][3]
    w = matriz.m[3][0]*ponto.x + matriz.m[3][1]*ponto.y + matriz.m[3][2]*ponto.z + matriz.m[3][3]
    
    // Normalizar por w (para projeções perspectivas)
    SE abs(w - 1.0) > 0.0001:
        x = x / w
        y = y / w
        z = z / w
    
    RETORNAR criar_ponto(x, y, z)

FUNÇÃO transformar_vetor(matriz, vetor):
    """Aplica transformação a vetor (direção, w=0)
    Ignora translação!
    """
    x = matriz.m[0][0]*vetor.x + matriz.m[0][1]*vetor.y + matriz.m[0][2]*vetor.z
    y = matriz.m[1][0]*vetor.x + matriz.m[1][1]*vetor.y + matriz.m[1][2]*vetor.z
    z = matriz.m[2][0]*vetor.x + matriz.m[2][1]*vetor.y + matriz.m[2][2]*vetor.z
    
    RETORNAR criar_ponto(x, y, z)
```

**💡 Insight:** Coordenadas homogêneas (x, y, z, w) permitem translação via multiplicação matricial. Pontos têm w=1, vetores w=0.

---

### 3. Transformações Básicas

#### Pseudocódigo

```
// ========== TRANSLAÇÃO ==========

FUNÇÃO criar_translacao(tx, ty, tz):
    """Matriz de translação por (tx, ty, tz)
    ⎡1  0  0  tx⎤
    ⎢0  1  0  ty⎥
    ⎢0  0  1  tz⎥
    ⎣0  0  0  1 ⎦
    """
    mat = criar_matriz_identidade()
    mat.m[0][3] = tx
    mat.m[1][3] = ty
    mat.m[2][3] = tz
    RETORNAR mat

// ========== ESCALA ==========

FUNÇÃO criar_escala(sx, sy, sz):
    """Matriz de escala por (sx, sy, sz)
    ⎡sx 0  0  0⎤
    ⎢0  sy 0  0⎥
    ⎢0  0  sz 0⎥
    ⎣0  0  0  1⎦
    """
    mat = criar_matriz_identidade()
    mat.m[0][0] = sx
    mat.m[1][1] = sy
    mat.m[2][2] = sz
    RETORNAR mat

FUNÇÃO criar_escala_uniforme(s):
    """Escala uniforme (mesmo fator em todas direções)"""
    RETORNAR criar_escala(s, s, s)

// ========== ROTAÇÃO (eixos principais) ==========

FUNÇÃO criar_rotacao_x(angulo_rad):
    """Rotação ao redor do eixo X
    ⎡1   0        0       0⎤
    ⎢0   cos(θ)  -sen(θ)  0⎥
    ⎢0   sen(θ)   cos(θ)  0⎥
    ⎣0   0        0       1⎦
    """
    mat = criar_matriz_identidade()
    c = cosseno(angulo_rad)
    s = seno(angulo_rad)
    
    mat.m[1][1] = c
    mat.m[1][2] = -s
    mat.m[2][1] = s
    mat.m[2][2] = c
    
    RETORNAR mat

FUNÇÃO criar_rotacao_y(angulo_rad):
    """Rotação ao redor do eixo Y
    ⎡ cos(θ)  0  sen(θ)  0⎤
    ⎢ 0       1  0       0⎥
    ⎢-sen(θ)  0  cos(θ)  0⎥
    ⎣ 0       0  0       1⎦
    """
    mat = criar_matriz_identidade()
    c = cosseno(angulo_rad)
    s = seno(angulo_rad)
    
    mat.m[0][0] = c
    mat.m[0][2] = s
    mat.m[2][0] = -s
    mat.m[2][2] = c
    
    RETORNAR mat

FUNÇÃO criar_rotacao_z(angulo_rad):
    """Rotação ao redor do eixo Z
    ⎡cos(θ)  -sen(θ)  0  0⎤
    ⎢sen(θ)   cos(θ)  0  0⎥
    ⎢0        0       1  0⎥
    ⎣0        0       0  1⎦
    """
    mat = criar_matriz_identidade()
    c = cosseno(angulo_rad)
    s = seno(angulo_rad)
    
    mat.m[0][0] = c
    mat.m[0][1] = -s
    mat.m[1][0] = s
    mat.m[1][1] = c
    
    RETORNAR mat
```

**🔍 Checkpoint:** Rotação de 90° em Z transforma `(1,0,0)` em `(0,1,0)`.

---

### 4. Rotação Arbitrária (Eixo Qualquer)

#### Pseudocódigo

```
FUNÇÃO criar_rotacao_eixo(eixo, angulo_rad):
    """Rotação ao redor de eixo arbitrário (fórmula de Rodrigues)
    Eixo deve ser unitário!
    """
    eixo_norm = normalizar(eixo)
    x = eixo_norm.x
    y = eixo_norm.y
    z = eixo_norm.z
    
    c = cosseno(angulo_rad)
    s = seno(angulo_rad)
    t = 1.0 - c  // "one minus cosine"
    
    mat = criar_matriz_identidade()
    
    // Primeira linha
    mat.m[0][0] = t*x*x + c
    mat.m[0][1] = t*x*y - s*z
    mat.m[0][2] = t*x*z + s*y
    
    // Segunda linha
    mat.m[1][0] = t*x*y + s*z
    mat.m[1][1] = t*y*y + c
    mat.m[1][2] = t*y*z - s*x
    
    // Terceira linha
    mat.m[2][0] = t*x*z - s*y
    mat.m[2][1] = t*y*z + s*x
    mat.m[2][2] = t*z*z + c
    
    RETORNAR mat
```

**💡 Insight:** Fórmula de Rodrigues generaliza rotações - qualquer rotação 3D pode ser descrita por eixo + ângulo.

---

### 5. Transformações Compostas

#### Pseudocódigo

```
FUNÇÃO criar_transformacao_composta(operacoes):
    """Compõe lista de transformações
    operacoes = [(tipo, parametros), ...]
    Exemplo: [("translacao", (1,2,3)), ("rotacao_z", 1.57), ("escala", (2,2,2))]
    
    IMPORTANTE: Aplicadas da DIREITA para ESQUERDA (ordem de multiplicação)
    """
    mat_resultado = criar_matriz_identidade()
    
    // Multiplicar na ordem reversa
    PARA i DE tamanho(operacoes)-1 ATÉ 0 DECREMENTANDO:
        (tipo, params) = operacoes[i]
        
        SE tipo == "translacao":
            mat = criar_translacao(params[0], params[1], params[2])
        SENÃO SE tipo == "rotacao_x":
            mat = criar_rotacao_x(params)
        SENÃO SE tipo == "rotacao_y":
            mat = criar_rotacao_y(params)
        SENÃO SE tipo == "rotacao_z":
            mat = criar_rotacao_z(params)
        SENÃO SE tipo == "escala":
            mat = criar_escala(params[0], params[1], params[2])
        SENÃO:
            CONTINUAR
        
        mat_resultado = multiplicar_matrizes(mat, mat_resultado)
    
    RETORNAR mat_resultado

FUNÇÃO criar_look_at(posicao_camera, alvo, up):
    """Matriz de visualização (câmera olhando para alvo)
    Posiciona câmera em 'posicao_camera' olhando para 'alvo'
    'up' define orientação vertical
    """
    // Direção de visão (para trás na convenção OpenGL)
    forward = normalizar(vetor_entre_pontos(alvo, posicao_camera))
    
    // Direita
    right = normalizar(produto_vetorial(up, forward))
    
    // Up corrigido
    up_corrigido = produto_vetorial(forward, right)
    
    // Montar matriz de rotação + translação
    mat = criar_matriz_identidade()
    
    // Colunas da matriz são os eixos
    mat.m[0][0] = right.x
    mat.m[0][1] = right.y
    mat.m[0][2] = right.z
    
    mat.m[1][0] = up_corrigido.x
    mat.m[1][1] = up_corrigido.y
    mat.m[1][2] = up_corrigido.z
    
    mat.m[2][0] = forward.x
    mat.m[2][1] = forward.y
    mat.m[2][2] = forward.z
    
    // Translação
    mat.m[0][3] = -produto_escalar(right, posicao_camera)
    mat.m[1][3] = -produto_escalar(up_corrigido, posicao_camera)
    mat.m[2][3] = -produto_escalar(forward, posicao_camera)
    
    RETORNAR mat
```

**💡 Insight:** Ordem importa! Rotar depois transladar ≠ transladar depois rotar. Multiplicação matricial é não-comutativa.

---

## 🎮 Aplicações Práticas

### 1. Gráficos 3D: Pipeline de transformação

```
FUNÇÃO renderizar_objeto(vertices, matriz_modelo, matriz_view, matriz_projecao):
    """Pipeline completo: Modelo → Mundo → Câmera → Tela"""
    
    // Matriz MVP (Model-View-Projection)
    mat_mv = multiplicar_matrizes(matriz_view, matriz_modelo)
    mat_mvp = multiplicar_matrizes(matriz_projecao, mat_mv)
    
    vertices_transformados = LISTA_VAZIA
    
    PARA CADA vertice EM vertices:
        // Aplicar transformação completa
        v_clip = transformar_ponto(mat_mvp, vertice)
        
        // Normalização de dispositivo (NDC) já feita por w
        ADICIONAR v_clip A vertices_transformados
    
    RETORNAR vertices_transformados
```

**🎯 Uso:** OpenGL, DirectX, motores de jogos.

---

### 2. Robótica: Cinemática de braço robótico

```
FUNÇÃO calcular_posicao_efetuador(angulos_juntas, comprimentos_links):
    """Cinemática direta: ângulos → posição da garra
    Braço com 3 juntas rotacionais
    """
    mat_acumulada = criar_matriz_identidade()
    
    // Junta 1 (base, rotação em Z)
    mat1 = criar_rotacao_z(angulos_juntas[0])
    mat_acumulada = multiplicar_matrizes(mat_acumulada, mat1)
    
    // Link 1
    mat_trans1 = criar_translacao(comprimentos_links[0], 0, 0)
    mat_acumulada = multiplicar_matrizes(mat_acumulada, mat_trans1)
    
    // Junta 2 (rotação em Z)
    mat2 = criar_rotacao_z(angulos_juntas[1])
    mat_acumulada = multiplicar_matrizes(mat_acumulada, mat2)
    
    // Link 2
    mat_trans2 = criar_translacao(comprimentos_links[1], 0, 0)
    mat_acumulada = multiplicar_matrizes(mat_acumulada, mat_trans2)
    
    // Junta 3
    mat3 = criar_rotacao_z(angulos_juntas[2])
    mat_acumulada = multiplicar_matrizes(mat_acumulada, mat3)
    
    // Link 3
    mat_trans3 = criar_translacao(comprimentos_links[2], 0, 0)
    mat_acumulada = multiplicar_matrizes(mat_acumulada, mat_trans3)
    
    // Posição do efetuador = origem transformada
    origem = criar_ponto(0, 0, 0)
    posicao_final = transformar_ponto(mat_acumulada, origem)
    
    RETORNAR posicao_final
```

**🎯 Uso:** Robótica, animação, IK (inverse kinematics).

---

### 3. Navegação: Conversão GPS para coordenadas locais

```
FUNÇÃO gps_para_local(lat, lon, alt, lat_ref, lon_ref, alt_ref):
    """Converte GPS (lat/lon/alt) para coordenadas cartesianas locais
    Referência: ponto de origem do mapa
    """
    // Raio da Terra (aproximado)
    R_TERRA = 6371000.0  // metros
    
    // Diferenças angulares (em radianos)
    dlat = graus_para_radianos(lat - lat_ref)
    dlon = graus_para_radianos(lon - lon_ref)
    
    // Aproximação plana (válida para distâncias pequenas)
    lat_media = graus_para_radianos((lat + lat_ref) / 2)
    
    // X = leste, Y = norte, Z = altitude
    x = R_TERRA * dlon * cosseno(lat_media)
    y = R_TERRA * dlat
    z = alt - alt_ref
    
    RETORNAR criar_ponto(x, y, z)

FUNÇÃO local_para_gps(x, y, z, lat_ref, lon_ref, alt_ref):
    """Converte coordenadas locais de volta para GPS"""
    R_TERRA = 6371000.0
    
    lat_media = graus_para_radianos(lat_ref)
    
    dlat = y / R_TERRA
    dlon = x / (R_TERRA * cosseno(lat_media))
    
    lat = lat_ref + radianos_para_graus(dlat)
    lon = lon_ref + radianos_para_graus(dlon)
    alt = alt_ref + z
    
    RETORNAR (lat, lon, alt)
```

**🎯 Uso:** GPS, mapas, drones.

---

## 🧪 Testes Essenciais

```
FUNÇÃO testar_coordenadas_transformacoes():
    epsilon = 0.001
    
    // Teste 1: Conversão polares
    p1 = criar_ponto(1, 1, 0)
    polar = cartesianas_para_polares(p1.x, p1.y)
    ASSERT abs(polar.r - raiz_quadrada(2)) < epsilon
    ASSERT abs(polar.theta - PI/4) < epsilon
    
    p1_volta = polares_para_cartesianas(polar.r, polar.theta)
    ASSERT distancia_pontos(p1, p1_volta) < epsilon
    
    // Teste 2: Conversão esféricas
    p2 = criar_ponto(0, 0, 1)
    esf = cartesianas_para_esfericas(p2.x, p2.y, p2.z)
    ASSERT abs(esf.rho - 1.0) < epsilon
    ASSERT abs(esf.phi) < epsilon  // No eixo Z, φ=0
    
    // Teste 3: Translação
    mat_trans = criar_translacao(5, 0, 0)
    p3 = criar_ponto(1, 2, 3)
    p3_trans = transformar_ponto(mat_trans, p3)
    ASSERT abs(p3_trans.x - 6) < epsilon
    ASSERT abs(p3_trans.y - 2) < epsilon
    
    // Teste 4: Rotação 90° em Z
    mat_rot = criar_rotacao_z(PI/2)
    p4 = criar_ponto(1, 0, 0)
    p4_rot = transformar_ponto(mat_rot, p4)
    ASSERT abs(p4_rot.x) < epsilon
    ASSERT abs(p4_rot.y - 1) < epsilon
    
    // Teste 5: Composição (transladar + escalar)
    mat_composta = criar_transformacao_composta([
        ("translacao", (1, 0, 0)),
        ("escala", (2, 2, 2))
    ])
    p5 = criar_ponto(1, 1, 1)
    p5_comp = transformar_ponto(mat_composta, p5)
    // Ordem: primeiro escala (2,2,2), depois translada (1,0,0)
    // Esperado: (3, 2, 2)
    ASSERT abs(p5_comp.x - 3) < epsilon
    ASSERT abs(p5_comp.y - 2) < epsilon
    
    IMPRIMIR "✅ Todos os testes de coordenadas e transformações passaram!"
```

---

## 📊 Complexidade

| Operação | Tempo | Espaço |
|----------|-------|--------|
| Conversão coordenadas | O(1) | O(1) |
| Multiplicação matriz 4×4 | O(1) | O(1) |
| Transformar ponto | O(1) | O(1) |
| Compor n transformações | O(n) | O(1) |

**Nota:** Matriz 4×4 é tamanho fixo, logo todas operações são O(1).

---

## 🚀 Otimizações

1. **Cache de matrizes:** Pré-calcular transformações estáticas
2. **SIMD:** SSE/AVX para multiplicar 4 pontos simultaneamente
3. **Quaternions:** Mais eficientes para rotações (menos gimbal lock)
4. **Decomposição:** Armazenar T, R, S separados em vez de matriz completa

```
// Exemplo: Estrutura otimizada
ESTRUTURA TransformacaoOtimizada:
    CAMPOS:
        translacao: Ponto3D
        rotacao: Quaternion   // Mais eficiente que matriz 3×3
        escala: Ponto3D
        
        matriz_cache: Matriz4x4
        cache_valido: booleano

FUNÇÃO obter_matriz(transform):
    SE NÃO transform.cache_valido:
        // Reconstruir apenas quando modificado
        transform.matriz_cache = construir_matriz_TRS(
            transform.translacao,
            transform.rotacao,
            transform.escala
        )
        transform.cache_valido = VERDADEIRO
    
    RETORNAR transform.matriz_cache
```

---

## 🔗 Próximos Passos

- `k4-pratica/p3-avancados.md` → Problemas de transformações complexas
- `k5-projeto/` → Sistema de navegação completo com múltiplos sistemas de coordenadas
- Explorar quaternions para rotações avançadas

---

## 📚 Referências

- 3D Math Primer for Graphics and Game Development (Dunn & Parberry) - Cap. 4-6
- Mathematics for 3D Game Programming (Lengyel) - Cap. 2-3
- GLM documentation - transformações e projeções
- Real-Time Rendering (Möller & Haines) - Cap. 4 (Transforms)
