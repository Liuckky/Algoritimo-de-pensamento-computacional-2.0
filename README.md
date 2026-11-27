Sobre o projeto

Este projeto implementa um sistema de calculadora de matrizes com menu interativo no terminal. O usuário informa as dimensões e os elementos das matrizes e escolhe qual operação deseja executar. O objetivo é praticar conceitos fundamentais de C, como alocação dinâmica de memória, ponteiros, funções, modularização e manipulação de vetores bidimensionais, além de aplicar conceitos de álgebra linear.

Funcionalidades:
Soma de matrizes
Subtração de matrizes
Multiplicação de matrizes
Multiplicação de matriz por escalar
Transposta de uma matriz
Determinante (matrizes quadradas)
Matriz inversa (matrizes quadradas não singulares)
Exibição formatada das matrizes e dos resultados
Validação de dimensões e tratamento de erros (ex.: dimensões incompatíveis)

Tecnologias utilizadas
Linguagem C (padrão C99 ou superior)

Como funciona:

As matrizes são representadas como vetores bidimensionais alocados dinamicamente (malloc/free), permitindo trabalhar com qualquer dimensão.
Cada operação é implementada em uma função separada, o que facilita a manutenção e a reutilização do código.
O determinante pode ser calculado por expansão de Laplace ou eliminação de Gauss.
A inversa pode ser obtida pelo método de Gauss-Jordan ou pela matriz adjunta.
Antes de cada operação, o programa valida as dimensões das matrizes e informa o erro caso sejam incompatíveis.

