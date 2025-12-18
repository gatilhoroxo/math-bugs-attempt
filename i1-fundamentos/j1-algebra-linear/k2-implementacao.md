# Álgebra Linear - Implementação em C/C++

## Como Codificar Isso?

Vamos implementar os conceitos de Álgebra Linear do zero, entendendo o que acontece "por baixo do capô".

---

## 1. Estrutura Básica: Vetor

### Implementação Simples em C

```c
#include <stdio.h>
#include <math.h>

typedef struct {
    double x, y, z;
} Vec3;

// Criar vetor
Vec3 vec3_create(double x, double y, double z) {
    Vec3 v = {x, y, z};
    return v;
}

// Soma de vetores: a + b
Vec3 vec3_add(Vec3 a, Vec3 b) {
    return vec3_create(a.x + b.x, a.y + b.y, a.z + b.z);
}

// Subtração: a - b
Vec3 vec3_sub(Vec3 a, Vec3 b) {
    return vec3_create(a.x - b.x, a.y - b.y, a.z - b.z);
}

// Multiplicação por escalar: k * v
Vec3 vec3_scale(Vec3 v, double k) {
    return vec3_create(k * v.x, k * v.y, k * v.z);
}

// Magnitude (comprimento) do vetor: |v|
double vec3_length(Vec3 v) {
    return sqrt(v.x * v.x + v.y * v.y + v.z * v.z);
}

// Normalizar (vetor unitário): v / |v|
Vec3 vec3_normalize(Vec3 v) {
    double len = vec3_length(v);
    if (len == 0) return vec3_create(0, 0, 0); // Evitar divisão por zero
    return vec3_scale(v, 1.0 / len);
}

// Produto escalar (dot product): a · b
double vec3_dot(Vec3 a, Vec3 b) {
    return a.x * b.x + a.y * b.y + a.z * b.z;
}

// Produto vetorial (cross product): a × b
Vec3 vec3_cross(Vec3 a, Vec3 b) {
    return vec3_create(
        a.y * b.z - a.z * b.y,
        a.z * b.x - a.x * b.z,
        a.x * b.y - a.y * b.x
    );
}

// Imprimir vetor
void vec3_print(Vec3 v) {
    printf("(%.2f, %.2f, %.2f)\n", v.x, v.y, v.z);
}
```

### Exemplo de Uso

```c
int main() {
    Vec3 a = vec3_create(1, 0, 0);
    Vec3 b = vec3_create(0, 1, 0);
    
    // Soma
    Vec3 c = vec3_add(a, b);
    printf("a + b = "); vec3_print(c);  // (1.00, 1.00, 0.00)
    
    // Produto escalar
    double dot = vec3_dot(a, b);
    printf("a · b = %.2f\n", dot);  // 0.00 (perpendiculares!)
    
    // Produto vetorial (gera vetor perpendicular a ambos)
    Vec3 cross = vec3_cross(a, b);
    printf("a × b = "); vec3_print(cross);  // (0.00, 0.00, 1.00)
    
    return 0;
}
```

**💡 Insight:** O produto vetorial de (1,0,0) e (0,1,0) dá (0,0,1) - o vetor Z! Isso é usado em gráficos 3D para calcular vetores normais a superfícies.

---

## 2. Estrutura Básica: Matriz

### Implementação de Matriz 4×4 (comum em gráficos 3D)

```c
typedef struct {
    double m[4][4];  // Row-major order
} Mat4;

// Criar matriz identidade
Mat4 mat4_identity() {
    Mat4 result = {0};
    for (int i = 0; i < 4; i++) {
        result.m[i][i] = 1.0;
    }
    return result;
}

// Multiplicação matriz × vetor (assume vetor 4D: x, y, z, w)
typedef struct {
    double x, y, z, w;
} Vec4;

Vec4 mat4_mul_vec4(Mat4 mat, Vec4 v) {
    Vec4 result;
    result.x = mat.m[0][0]*v.x + mat.m[0][1]*v.y + mat.m[0][2]*v.z + mat.m[0][3]*v.w;
    result.y = mat.m[1][0]*v.x + mat.m[1][1]*v.y + mat.m[1][2]*v.z + mat.m[1][3]*v.w;
    result.z = mat.m[2][0]*v.x + mat.m[2][1]*v.y + mat.m[2][2]*v.z + mat.m[2][3]*v.w;
    result.w = mat.m[3][0]*v.x + mat.m[3][1]*v.y + mat.m[3][2]*v.z + mat.m[3][3]*v.w;
    return result;
}

// Multiplicação matriz × matriz: C = A × B
Mat4 mat4_mul(Mat4 a, Mat4 b) {
    Mat4 result = {0};
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 4; j++) {
            for (int k = 0; k < 4; k++) {
                result.m[i][j] += a.m[i][k] * b.m[k][j];
            }
        }
    }
    return result;
}

// Imprimir matriz
void mat4_print(Mat4 m) {
    for (int i = 0; i < 4; i++) {
        printf("[ ");
        for (int j = 0; j < 4; j++) {
            printf("%7.2f ", m.m[i][j]);
        }
        printf("]\n");
    }
}
```

**⚠️ Complexidade:** A multiplicação matriz × matriz é O(n³) - muito caro! Por isso usamos bibliotecas otimizadas (BLAS, Eigen) em produção.

---

## 3. Transformações 2D

### Rotação

```c
// Matriz de rotação 2D (ângulo em radianos)
typedef struct {
    double m[3][3];  // Matriz 3×3 para coordenadas homogêneas
} Mat3;

Mat3 mat3_rotation(double angle) {
    Mat3 result = {0};
    double c = cos(angle);
    double s = sin(angle);
    
    result.m[0][0] = c;   result.m[0][1] = -s;  result.m[0][2] = 0;
    result.m[1][0] = s;   result.m[1][1] = c;   result.m[1][2] = 0;
    result.m[2][0] = 0;   result.m[2][1] = 0;   result.m[2][2] = 1;
    
    return result;
}

// Por que 3×3 para 2D? Coordenadas homogêneas permitem translação!
```

**Coordenadas Homogêneas:** Representamos pontos 2D como (x, y, 1) para poder fazer translação com multiplicação matricial.

### Translação

```c
Mat3 mat3_translation(double tx, double ty) {
    Mat3 result = mat3_identity();
    result.m[0][2] = tx;
    result.m[1][2] = ty;
    return result;
}
```

### Escala

```c
Mat3 mat3_scale(double sx, double sy) {
    Mat3 result = {0};
    result.m[0][0] = sx;
    result.m[1][1] = sy;
    result.m[2][2] = 1.0;
    return result;
}
```

### Exemplo Prático: Transformar um Triângulo

```c
typedef struct {
    double x, y;
} Vec2;

typedef struct {
    double x, y, w;  // Coordenadas homogêneas
} Vec2H;

Vec2H vec2_to_homogeneous(Vec2 v) {
    Vec2H result = {v.x, v.y, 1.0};
    return result;
}

Vec2 homogeneous_to_vec2(Vec2H v) {
    Vec2 result = {v.x / v.w, v.y / v.w};
    return result;
}

Vec2H mat3_mul_vec2h(Mat3 mat, Vec2H v) {
    Vec2H result;
    result.x = mat.m[0][0]*v.x + mat.m[0][1]*v.y + mat.m[0][2]*v.w;
    result.y = mat.m[1][0]*v.x + mat.m[1][1]*v.y + mat.m[1][2]*v.w;
    result.w = mat.m[2][0]*v.x + mat.m[2][1]*v.y + mat.m[2][2]*v.w;
    return result;
}

void transform_triangle() {
    Vec2 triangle[3] = {
        {0, 0}, {1, 0}, {0.5, 1}
    };
    
    // Rotacionar 45 graus ao redor da origem
    Mat3 rotation = mat3_rotation(M_PI / 4);  // 45° = π/4
    
    // Transladar para (2, 3)
    Mat3 translation = mat3_translation(2, 3);
    
    // Combinar transformações: primeiro rotaciona, depois translada
    Mat3 transform = mat3_mul(translation, rotation);
    
    printf("Triângulo transformado:\n");
    for (int i = 0; i < 3; i++) {
        Vec2H h = vec2_to_homogeneous(triangle[i]);
        Vec2H transformed_h = mat3_mul_vec2h(transform, h);
        Vec2 transformed = homogeneous_to_vec2(transformed_h);
        printf("(%.2f, %.2f)\n", transformed.x, transformed.y);
    }
}
```

**💡 Ordem importa!** `T × R` (translada depois rotaciona) ≠ `R × T` (rotaciona depois translada)

---

## 4. Implementação de Algoritmos Úteis

### Resolver Sistema Linear (Eliminação Gaussiana)

```c
// Resolver Ax = b usando eliminação Gaussiana
// ATENÇÃO: Modifica a matriz A e vetor b!
int gauss_solve(double A[][10], double b[], double x[], int n) {
    // Forward elimination
    for (int i = 0; i < n; i++) {
        // Encontrar pivô
        int max_row = i;
        for (int k = i + 1; k < n; k++) {
            if (fabs(A[k][i]) > fabs(A[max_row][i])) {
                max_row = k;
            }
        }
        
        // Trocar linhas
        if (max_row != i) {
            for (int k = i; k < n; k++) {
                double tmp = A[i][k];
                A[i][k] = A[max_row][k];
                A[max_row][k] = tmp;
            }
            double tmp = b[i];
            b[i] = b[max_row];
            b[max_row] = tmp;
        }
        
        // Verificar se o sistema tem solução
        if (fabs(A[i][i]) < 1e-10) {
            return 0;  // Matriz singular
        }
        
        // Eliminação
        for (int k = i + 1; k < n; k++) {
            double factor = A[k][i] / A[i][i];
            for (int j = i; j < n; j++) {
                A[k][j] -= factor * A[i][j];
            }
            b[k] -= factor * b[i];
        }
    }
    
    // Back substitution
    for (int i = n - 1; i >= 0; i--) {
        x[i] = b[i];
        for (int j = i + 1; j < n; j++) {
            x[i] -= A[i][j] * x[j];
        }
        x[i] /= A[i][i];
    }
    
    return 1;  // Sucesso
}
```

### Calcular Autovalores (Método da Potência - simples)

```c
// Encontra o maior autovalor e seu autovetor (método da potência)
double power_method(double A[][10], int n, double eigenvector[], int max_iter) {
    // Inicializar com vetor aleatório
    for (int i = 0; i < n; i++) {
        eigenvector[i] = 1.0;
    }
    
    double eigenvalue = 0;
    double temp[10];
    
    for (int iter = 0; iter < max_iter; iter++) {
        // Multiplicar: temp = A × eigenvector
        for (int i = 0; i < n; i++) {
            temp[i] = 0;
            for (int j = 0; j < n; j++) {
                temp[i] += A[i][j] * eigenvector[j];
            }
        }
        
        // Calcular magnitude
        double norm = 0;
        for (int i = 0; i < n; i++) {
            norm += temp[i] * temp[i];
        }
        norm = sqrt(norm);
        
        // Normalizar
        for (int i = 0; i < n; i++) {
            eigenvector[i] = temp[i] / norm;
        }
        
        eigenvalue = norm;
    }
    
    return eigenvalue;
}
```

---

## 5. Versão C++ com Classes (Mais Organizado)

```cpp
#include <iostream>
#include <cmath>
#include <array>

class Vec3 {
public:
    double x, y, z;
    
    Vec3(double x = 0, double y = 0, double z = 0) : x(x), y(y), z(z) {}
    
    // Sobrecarga de operadores
    Vec3 operator+(const Vec3& other) const {
        return Vec3(x + other.x, y + other.y, z + other.z);
    }
    
    Vec3 operator-(const Vec3& other) const {
        return Vec3(x - other.x, y - other.y, z - other.z);
    }
    
    Vec3 operator*(double scalar) const {
        return Vec3(x * scalar, y * scalar, z * scalar);
    }
    
    double dot(const Vec3& other) const {
        return x * other.x + y * other.y + z * other.z;
    }
    
    Vec3 cross(const Vec3& other) const {
        return Vec3(
            y * other.z - z * other.y,
            z * other.x - x * other.z,
            x * other.y - y * other.x
        );
    }
    
    double length() const {
        return std::sqrt(x*x + y*y + z*z);
    }
    
    Vec3 normalize() const {
        double len = length();
        return (len > 0) ? (*this) * (1.0 / len) : Vec3();
    }
    
    void print() const {
        std::cout << "(" << x << ", " << y << ", " << z << ")" << std::endl;
    }
};
```

---

## 6. Otimizações e Bibliotecas Recomendadas

### Para Projetos Sérios, Use Bibliotecas Otimizadas:

**C/C++:**
- **Eigen**: Biblioteca header-only, muito rápida, ideal para aprendizado
- **GLM (OpenGL Mathematics)**: Focada em gráficos 3D
- **BLAS/LAPACK**: Bibliotecas de baixo nível, super otimizadas

**Python (para prototipagem rápida):**
- **NumPy**: Álgebra linear eficiente
- **SciPy**: Algoritmos avançados

### Por que usar bibliotecas?

```c
// Sua implementação ingênua: O(n³)
for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++)
        for (int k = 0; k < n; k++)
            C[i][j] += A[i][k] * B[k][j];

// Bibliotecas como BLAS usam:
// - Algoritmo de Strassen: O(n^2.807)
// - Paralelização (múltiplos cores)
// - Otimizações de cache (memory locality)
// - Instruções SIMD (vetorização)
```

Para matrizes grandes, a diferença pode ser **100x mais rápido**!

---

## 7. Exemplo Completo: Mini Engine de Transformações

Veja o diretório `projeto-transformacoes-2d/` para um projeto completo que:
- Desenha formas 2D
- Aplica transformações interativas
- Visualiza o efeito de cada matriz
- Implementa todos os conceitos vistos

---

## Próximos Passos

1. Implemente as estruturas básicas (Vec3, Mat4)
2. Teste cada operação isoladamente
3. Faça os exercícios do **03-exercicios.md**
4. Construa o projeto âncora (transformações 2D)
5. Compare sua implementação com uma biblioteca (Eigen ou GLM)

**💡 Dica:** Comece simples! Implemente Vec2 antes de Vec3, matrizes 2×2 antes de 4×4.