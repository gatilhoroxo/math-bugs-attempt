# Especificação Técnica: Engine de Transformações 2D

## 📐 Visão Geral

Sistema de visualização de transformações lineares 2D que permite aplicar e compor transformações geométricas em tempo real.

---

## 🎯 Requisitos Funcionais

### RF-01: Representação de Vetores
- **Descrição:** Sistema deve representar vetores 2D
- **Entrada:** Coordenadas (x, y)
- **Saída:** Estrutura de dados para vetor
- **Validação:** Valores numéricos válidos
- **Prioridade:** Alta

### RF-02: Operações com Vetores
- **Descrição:** Implementar operações básicas
- **Operações:**
  - Adição de vetores
  - Multiplicação por escalar
  - Produto escalar
  - Norma/magnitude
- **Prioridade:** Alta

### RF-03: Representação de Matrizes
- **Descrição:** Sistema deve representar matrizes 2x2 e 3x3
- **Entrada:** Array 2D ou valores individuais
- **Saída:** Estrutura de dados para matriz
- **Validação:** Dimensões corretas
- **Prioridade:** Alta

### RF-04: Multiplicação Matriz-Vetor
- **Descrição:** Aplicar transformação a um vetor
- **Entrada:** Matriz de transformação + vetor
- **Saída:** Vetor transformado
- **Validação:** Dimensões compatíveis
- **Prioridade:** Alta

### RF-05: Multiplicação Matriz-Matriz
- **Descrição:** Compor transformações
- **Entrada:** Duas matrizes
- **Saída:** Matriz resultante da composição
- **Validação:** Dimensões compatíveis
- **Prioridade:** Alta

### RF-06: Transformações Básicas
- **Descrição:** Criar matrizes de transformação
- **Transformações:**
  - Rotação (ângulo θ)
  - Escala (sx, sy)
  - Cisalhamento/shear (kx, ky)
  - Reflexão (eixos x, y, origem)
  - Translação (tx, ty) - coordenadas homogêneas
- **Prioridade:** Alta

### RF-07: Visualização Gráfica
- **Descrição:** Renderizar vetores e transformações
- **Elementos:**
  - Grade de coordenadas
  - Eixos X e Y
  - Vetores (original e transformado)
  - Opcionalmente: grade deformada
- **Prioridade:** Média

### RF-08: Interatividade
- **Descrição:** Permitir manipulação em tempo real
- **Funcionalidades:**
  - Ajustar parâmetros de transformação
  - Selecionar tipo de transformação
  - Resetar para estado inicial
  - Compor múltiplas transformações
- **Prioridade:** Média

### RF-09: Sistema de Coordenadas
- **Descrição:** Converter entre coordenadas matemáticas e de tela
- **Operações:**
  - Matemática → Tela (rendering)
  - Tela → Matemática (input do usuário)
- **Validação:** Manter proporções corretas
- **Prioridade:** Média

### RF-10: Composição de Transformações
- **Descrição:** Aplicar sequência de transformações
- **Funcionalidades:**
  - Adicionar transformação à pilha
  - Remover última transformação
  - Limpar todas as transformações
  - Visualizar ordem de aplicação
- **Prioridade:** Baixa

---

## 🔧 Requisitos Não-Funcionais

### RNF-01: Performance
- Renderização deve ser fluida (>30 FPS)
- Cálculos devem ser eficientes mesmo com múltiplas transformações

### RNF-02: Usabilidade
- Interface intuitiva
- Feedback visual imediato
- Mensagens de erro claras

### RNF-03: Manutenibilidade
- Código modular e bem documentado
- Separação entre lógica matemática e visualização
- Testes unitários para operações matemáticas

### RNF-04: Portabilidade
- Deve funcionar em ambiente desktop/web
- Sem dependências externas pesadas

---

## 📊 Estrutura de Dados

### Vetor 2D
```
Vetor2D {
    x: número
    y: número
}
```

### Vetor 3D (Coordenadas Homogêneas)
```
Vetor3D {
    x: número
    y: número
    w: número (geralmente 1)
}
```

### Matriz 2x2
```
Matriz2x2 {
    valores: [
        [a, b],
        [c, d]
    ]
}
```

### Matriz 3x3
```
Matriz3x3 {
    valores: [
        [a, b, tx],
        [c, d, ty],
        [0, 0, 1]
    ]
}
```

---

## 🧮 Especificações Matemáticas

### Rotação (θ radianos)
```
| cos(θ)  -sin(θ) |
| sin(θ)   cos(θ) |
```

### Escala (sx, sy)
```
| sx   0  |
| 0   sy  |
```

### Cisalhamento/Shear
Horizontal (kx):
```
| 1   kx |
| 0    1 |
```

Vertical (ky):
```
| 1    0 |
| ky   1 |
```

### Reflexão
Eixo X:
```
| 1   0 |
| 0  -1 |
```

Eixo Y:
```
|-1   0 |
| 0   1 |
```

Origem:
```
|-1   0 |
| 0  -1 |
```

### Translação (coordenadas homogêneas)
```
| 1   0   tx |
| 0   1   ty |
| 0   0   1  |
```

### Composição
Para aplicar T1 seguida de T2:
```
T_final = T2 × T1
```
**IMPORTANTE:** Ordem inversa da aplicação!

---

## 🎨 Interface do Usuário

### Componentes Principais

1. **Área de Visualização**
   - Canvas/janela de desenho
   - Grade de coordenadas
   - Eixos X e Y claramente marcados
   - Vetores renderizados com cores distintas

2. **Painel de Controle**
   - Seletor de tipo de transformação
   - Campos de entrada para parâmetros
   - Botão "Aplicar"
   - Botão "Reset"
   - Lista de transformações aplicadas

3. **Informações**
   - Matriz de transformação atual
   - Coordenadas do vetor original
   - Coordenadas do vetor transformado
   - Determinante da matriz

---

## 🧪 Casos de Teste

### CT-01: Vetor Identidade
- **Entrada:** Vetor (1, 0), Transformação Identidade
- **Saída Esperada:** Vetor (1, 0)

### CT-02: Rotação 90°
- **Entrada:** Vetor (1, 0), Rotação π/2
- **Saída Esperada:** Vetor (0, 1) [aproximadamente]

### CT-03: Escala Uniforme
- **Entrada:** Vetor (1, 1), Escala (2, 2)
- **Saída Esperada:** Vetor (2, 2)

### CT-04: Escala Não-Uniforme
- **Entrada:** Vetor (1, 1), Escala (2, 3)
- **Saída Esperada:** Vetor (2, 3)

### CT-05: Reflexão Eixo X
- **Entrada:** Vetor (2, 3), Reflexão X
- **Saída Esperada:** Vetor (2, -3)

### CT-06: Composição Rotação + Escala
- **Entrada:** Vetor (1, 0), Rotação 90° depois Escala (2, 2)
- **Saída Esperada:** Vetor (0, 2)

### CT-07: Ordem de Composição
- **Entrada:** Vetor (1, 0)
  - A: Rotação 90° → Escala (2, 1)
  - B: Escala (2, 1) → Rotação 90°
- **Saída Esperada:** Resultados diferentes (demonstra ordem importa)

### CT-08: Translação
- **Entrada:** Vetor (1, 1), Translação (3, 4)
- **Saída Esperada:** Vetor (4, 5)

### CT-09: Matriz Inversa
- **Entrada:** Transformação T seguida de T^(-1)
- **Saída Esperada:** Vetor original

### CT-10: Determinante Zero
- **Entrada:** Matriz [[1, 1], [1, 1]]
- **Comportamento Esperado:** Aviso de transformação não-invertível

---

## 📁 Estrutura de Arquivos

```
projeto-transformacoes-2d/
├── src/
│   ├── math/
│   │   ├── vetor.extensao
│   │   ├── matriz.extensao
│   │   └── transformacoes.extensao
│   ├── graphics/
│   │   ├── canvas.extensao
│   │   └── coordenadas.extensao
│   └── main.extensao
├── tests/
│   ├── test_vetor.extensao
│   ├── test_matriz.extensao
│   └── test_transformacoes.extensao
├── exemplos/
│   ├── rotacao.extensao
│   ├── composicao.extensao
│   └── animacao.extensao
├── especificacao.md (este arquivo)
└── readme.md
```

---

## 🚀 Fases de Desenvolvimento

### Fase 1: Fundação Matemática
- [ ] Implementar estrutura de Vetor2D
- [ ] Implementar operações de vetores
- [ ] Implementar estrutura de Matriz2x2
- [ ] Implementar multiplicação matriz-vetor
- [ ] Testes unitários para todas as operações

### Fase 2: Transformações Básicas
- [ ] Implementar função de rotação
- [ ] Implementar função de escala
- [ ] Implementar função de cisalhamento
- [ ] Implementar função de reflexão
- [ ] Testes para cada transformação

### Fase 3: Visualização Básica
- [ ] Criar canvas/janela de desenho
- [ ] Desenhar grade de coordenadas
- [ ] Desenhar eixos X e Y
- [ ] Desenhar vetores
- [ ] Sistema de conversão de coordenadas

### Fase 4: Interatividade
- [ ] Interface para entrada de parâmetros
- [ ] Botões de controle (aplicar, reset)
- [ ] Atualização em tempo real

### Fase 5: Coordenadas Homogêneas
- [ ] Implementar Vetor3D
- [ ] Implementar Matriz3x3
- [ ] Adicionar suporte para translação
- [ ] Atualizar visualização

### Fase 6: Composição
- [ ] Sistema de pilha de transformações
- [ ] Visualização da ordem de transformações
- [ ] Multiplicação de múltiplas matrizes

### Fase 7: Polimento
- [ ] Melhorar feedback visual
- [ ] Adicionar animações
- [ ] Documentação completa
- [ ] Exemplos de uso

---

## 🎓 Conceitos de Álgebra Linear Aplicados

### Vetores
- Representação geométrica
- Operações: adição, multiplicação por escalar
- Produto escalar e ângulos
- Base e coordenadas

### Matrizes
- Transformações lineares
- Multiplicação matriz-vetor
- Composição de transformações
- Determinante e área
- Matriz inversa

### Coordenadas Homogêneas
- Por que 2D → 3D
- Translação como transformação linear
- Projeção de volta para 2D

### Propriedades
- Linearidade
- Preservação de operações
- Isometrias vs. transformações gerais

---

## ⚠️ Desafios e Armadilhas

### Armadilha 1: Ordem de Multiplicação
```
T2 × T1 aplica T1 primeiro, depois T2
```
Não confundir com ordem de escrita!

### Armadilha 2: Radianos vs. Graus
Funções trigonométricas usam radianos. Converter:
```
radianos = graus × (π / 180)
```

### Armadilha 3: Sistema de Coordenadas de Tela
- Geralmente Y cresce para baixo
- Precisa inverter ou ajustar cálculos

### Armadilha 4: Precisão Numérica
- Comparações de ponto flutuante
- Usar epsilon para igualdade
- Arredondamento em visualização

### Armadilha 5: Determinante Zero
- Transformação colapsa dimensão
- Não tem inversa
- Tratar casos especiais

---

## 📚 Extensões Possíveis

### Nível 1: Melhorias Básicas
- [ ] Múltiplos vetores simultâneos
- [ ] Histórico de transformações (undo/redo)
- [ ] Salvar/carregar configurações
- [ ] Exportar imagem do estado atual

### Nível 2: Funcionalidades Intermediárias
- [ ] Animação de transformação (interpolação)
- [ ] Visualização de grade deformada
- [ ] Autovalores e autovetores
- [ ] Decomposição SVD visualizada

### Nível 3: Desafios Avançados
- [ ] Transformações 3D (webGL/OpenGL)
- [ ] Bezier curves com transformações
- [ ] Editor de formas customizadas
- [ ] Sistema de partículas com transformações

---

## ✅ Critérios de Conclusão

### Funcionalidade
- [ ] Todas as transformações básicas implementadas
- [ ] Composição funciona corretamente
- [ ] Coordenadas homogêneas funcionam
- [ ] Visualização clara e precisa

### Qualidade do Código
- [ ] Código modular e organizado
- [ ] Funções bem nomeadas
- [ ] Comentários explicativos
- [ ] Sem duplicação de código

### Testes
- [ ] Todos os casos de teste passam
- [ ] Testes cobrem casos extremos
- [ ] Testes verificam precisão numérica

### Aprendizado
- [ ] Entende como matrizes transformam geometria
- [ ] Sabe explicar ordem de composição
- [ ] Compreende coordenadas homogêneas
- [ ] Pode implementar transformação customizada

---

## 💡 Dicas de Implementação

### Estrutura do Código
1. Comece com estruturas de dados puras (vetor, matriz)
2. Implemente operações matemáticas básicas
3. Adicione funções de transformação
4. Por último, adicione visualização

### Debugging
- Use `console.log` para imprimir matrizes
- Teste cada transformação isoladamente
- Compare com cálculos manuais
- Desenhe no papel para intuição

### Performance
- Evite criar objetos desnecessariamente
- Reutilize buffers quando possível
- Cache cálculos repetidos (sin/cos)
- Só redesenhe quando necessário

---

**Boa sorte com o projeto!** 🚀

Este projeto é a ponte entre teoria abstrata e aplicação concreta. Cada linha de código que você escreve está solidificando conceitos fundamentais de álgebra linear.
