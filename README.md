Sistema de Calculadora de Matrizes em C

Sobre o projeto:

Este programa lê as matrizes informadas pelo usuário, valida as condições matemáticas de cada operação, calcula o resultado e o exibe organizado em linhas e colunas. Quando uma operação não pode ser realizada, o sistema explica o motivo.

Adição
Subtração
Multiplicação
Divisão matricial (`A / B = A × B⁻¹`)
- Operações aplicadas **na ordem de cadastro**, da esquerda para a direita
- Exibição organizada da matriz resultante
- Mensagens claras para dimensões incompatíveis, divisor não quadrado e **matriz singular**
- Opção de **realizar novas operações**, **cadastrar novas matrizes** ou **encerrar**
- Leitura de dados **à prova de erros**: entradas inválidas (letras, valores fora do intervalo) são recusadas e pedidas novamente
- Aceita **vírgula ou ponto** como separador decimal (`2,5` ou `2.5`)

Com três ou mais matrizes, as operações são executadas **sequencialmente, da esquerda para a direita**.

| Operação | Condição para ser realizada |
|---|---|
| **Adição** | As matrizes devem ter **as mesmas dimensões** |
| **Subtração** | As matrizes devem ter **as mesmas dimensões** |
| **Multiplicação** | Em cada etapa, o número de **colunas** da primeira matriz deve ser igual ao número de **linhas** da segunda |
| **Divisão** | `A / B = A × B⁻¹`. O divisor deve ser **quadrado**, **inversível** (não singular) e ter **dimensões compatíveis** |

- `A × B × C` é calculada como `(A × B) × C`, validando as dimensões a cada etapa.
- `A / B / C` é calculada como `(A × B⁻¹) × C⁻¹`.
- Se `A` é `m × n`, cada matriz divisora precisa ser `n × n`.
- Se o divisor for **singular** (sem inversa), o sistema avisa e não executa a divisão.

O código é dividido em módulos (`.h` com as declarações e `.c` com a implementação). A `main()` apenas chama o sistema, e todo o fluxo fica organizado em funções específicas.

```
calculadora-de-matrizes/
├── src/
│   ├── main.c          # Ponto de entrada (apenas chama iniciarSistema)
│   ├── sistema.h/.c    # Fluxo principal: cadastro, menu e repetição
│   ├── matriz.h/.c     # Estrutura Matriz, alocação, cópia e liberação
│   ├── entrada.h/.c    # Leitura segura de inteiros, reais e matrizes
│   ├── saida.h/.c      # Menu, exibição de matrizes, resultados e erros
│   ├── validacao.h/.c  # Validação de dimensões e condições matemáticas
│   └── operacoes.h/.c  # Soma, subtração, multiplicação, inversa e divisão
├── Makefile
├── README.md
└── LICENSE
```

| Módulo | Responsabilidade | Principais funções |
|---|---|---|
| `matriz` | Memória | `criarMatriz()`, `copiarMatriz()`, `liberarMatriz()`, `liberarConjunto()` |
| `entrada` | Leitura | `lerInteiro()`, `lerReal()`, `lerMatriz()`, `lerConjuntoMatrizes()` |
| `saida` | Exibição | `exibirMenu()`, `exibirMatriz()`, `exibirResultado()`, `exibirErro()` |
| `validacao` | Validação | `validarMesmaDimensao()`, `validarMultiplicacao()`, `validarDivisao()` |
| `operacoes` | Cálculo | `somarMatrizes()`, `subtrairMatrizes()`, `multiplicarMatrizes()`, `inverterMatriz()`, `dividirMatrizes()`, `calcularSequencial()` |
| `sistema` | Coordenação | `iniciarSistema()` |

Requisitos:

- **GCC** instalado
  - **Linux:** `sudo apt install build-essential`
  - **Windows:** instale o [MinGW-w64](https://www.mingw-w64.org/) ou use o WSL
  - **macOS:** `xcode-select --install`

```bash
git clone https://github.com/SEU-USUARIO/calculadora-de-matrizes.git
cd calculadora-de-matrizes
```

Com make (Linux, macOS ou WSL)

```bash
make
```

```bash
gcc src/main.c src/sistema.c src/entrada.c src/saida.c src/validacao.c src/operacoes.c src/matriz.c -o calculadora -lm
```

> A flag `-lm` vincula a biblioteca matemática (`math.h`).

```bash
./calculadora          # Linux / macOS / WSL
calculadora.exe        # Windows
```

Para apagar os arquivos gerados na compilação: `make clean`.

```
===== CALCULADORA DE MATRIZES =====
Quantas matrizes deseja cadastrar (de 2 a 10)? 2

--- Matriz 1 ---
Linhas: 2
Colunas: 2
Elemento [1][1]: 4
Elemento [1][2]: 6
Elemento [2][1]: 8
Elemento [2][2]: 10

--- Matriz 2 ---
Linhas: 2
Colunas: 2
Elemento [1][1]: 2
Elemento [1][2]: 0
Elemento [2][1]: 0
Elemento [2][2]: 2

===== MENU DE OPERACOES =====
1 - Adicao
2 - Subtracao
3 - Multiplicacao
4 - Divisao (A x B^-1)
5 - Cadastrar novas matrizes
0 - Encerrar
Escolha uma opcao: 4

Resultado da divisao de 2 matrizes (2x2):
|      2.00      3.00 |
|      4.00      5.00 |
```

Com `A = [[1,2],[3,4]]`, `B = [[2,0],[0,2]]` e `C = [[1,0],[0,4]]`:

```
Resultado da divisao de 3 matrizes (2x2):
|      0.50      0.25 |
|      1.50      0.50 |
```

Ou seja, `(A × B⁻¹) × C⁻¹`.


```
[ERRO] Nao foi possivel realizar a divisao (etapa com a Matriz 2).
A Matriz 2 e singular (determinante igual a zero) e nao possui inversa.
```

```
[ERRO] Nao foi possivel realizar a adicao (etapa com a Matriz 2).
A adicao exige matrizes com as mesmas dimensoes.
Operando da esquerda: 2x3 | Matriz 2: 3x2
```

```
[ERRO] Nao foi possivel realizar a divisao (etapa com a Matriz 2).
A Matriz 2 (3x2) nao e quadrada; apenas matrizes quadradas possuem inversa e podem ser divisoras.
```

Como funciona

1. O programa pergunta quantas matrizes serão usadas (de 2 a 10) e cadastra cada uma, com memória **alocada dinamicamente**.
2. As matrizes cadastradas são exibidas e o menu é apresentado.
3. Ao escolher uma operação, `calcularSequencial()` percorre as matrizes **na ordem de cadastro**, acumulando o resultado a cada etapa.
4. Antes de cada etapa, as funções de **validação** verificam as dimensões e as condições matemáticas.
5. Na **divisão**, a inversa do divisor é calculada pelo método de **Gauss-Jordan com pivoteamento parcial**. Se não houver pivô válido (módulo menor que `1e-9`), a matriz é considerada **singular** e a operação é interrompida com uma mensagem.
6. O resultado é exibido e o menu volta a aparecer, permitindo **nova operação**, **novo cadastro** ou **encerrar
Estudante de ência da Computação — UDF

[![GitHub](https://img.shields.io/badge/GitHub-SEU--USUARIO-black?logo=github)](https://github.com/SEU-USUARIO)
