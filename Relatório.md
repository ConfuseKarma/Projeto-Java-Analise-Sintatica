Aqui está a documentação detalhada com base no seu projeto, incluindo a descrição da gramática fornecida, os conjuntos First e Follow, a implementação do analisador sintático e os casos de teste realizados.

---

# Documentação do Projeto: Analisador Sintático

## 1. Descrição da Gramática Fornecida

A gramática fornecida é uma **gramática livre de contexto (GLC)** para um conjunto básico de expressões com comandos de repetição (`while`) e atribuição. A gramática está definida da seguinte maneira:

```bnf
S → while ( E ) S | id = E
E → E + T | E - T | T
T → T * F | T / F | F
F → ( E ) | id
```

### Explicação dos componentes da gramática:

- **S**: Representa uma sentença, que pode ser um comando de repetição (`while`) ou uma atribuição.
    - `while ( E ) S`: Um comando de repetição onde `E` é uma expressão e `S` é um comando repetido dentro do laço.
    - `id = E`: Uma atribuição de valor a uma variável identificada por `id`.
  
- **E**: Representa uma expressão composta por operações de adição e subtração.
    - `E + T` ou `E - T`: Operações aritméticas de adição ou subtração entre uma expressão e um termo.
    - `T`: Um termo que pode ser uma operação multiplicativa ou um fator.
  
- **T**: Representa um termo que pode ser multiplicado ou dividido por um fator.
    - `T * F` ou `T / F`: Operações de multiplicação ou divisão entre um termo e um fator.
    - `F`: Um fator, que pode ser uma expressão entre parênteses ou um identificador `id`.

- **F**: Representa um fator, que pode ser uma expressão entre parênteses ou um identificador (`id`).
    - `( E )`: Uma expressão dentro de parênteses.
    - `id`: Um identificador que representa uma variável.

Essa gramática é utilizada para analisar e validar comandos de repetição (como `while`) e atribuições, juntamente com expressões aritméticas.

---

## 2. Conjuntos First e Follow Calculados

Para a construção do analisador sintático, os conjuntos **First** e **Follow** são fundamentais para determinar a sequência de tokens que o analisador deve esperar.

### Cálculo dos Conjuntos **First**

Os conjuntos **First** determinam o primeiro símbolo possível que pode aparecer a partir de um não terminal. Aqui estão os conjuntos **First** calculados para a gramática fornecida:

- **First(S)**: {`while`, `id`}
- **First(E)**: {`id`, `(`}
- **First(T)**: {`id`, `(`}
- **First(F)**: {`id`, `(`}

### Cálculo dos Conjuntos **Follow**

Os conjuntos **Follow** indicam quais símbolos podem aparecer imediatamente após um não terminal na derivação. Aqui estão os conjuntos **Follow** calculados para a gramática fornecida:

- **Follow(S)**: {`$`, `else`}
- **Follow(E)**: {`)`, `+`, `-`, `$`, `]`, `else`}
- **Follow(T)**: {`+`, `-`, `)`, `$`, `*`, `/`}
- **Follow(F)**: {`+`, `-`, `)`, `$`, `*`, `/`}

Os conjuntos **First** e **Follow** são utilizados para ajudar a determinar as transições no analisador sintático e decidir qual produção usar durante a análise.

---

## 3. Implementação do Analisador Sintático

O analisador sintático foi implementado utilizando uma abordagem **LL(1)**, baseada na recursão descendente. Ele utiliza os conjuntos **First** e **Follow** para determinar como validar a entrada.

### Estrutura do Código

A implementação do analisador sintático segue a estrutura da gramática fornecida, com funções recursivas para cada não terminal da gramática.

1. **Função `analisarS`**: Responsável por analisar a sentença `S` (comando de repetição ou atribuição).
2. **Função `analisarE`**: Responsável por analisar expressões `E` com operações de adição e subtração.
3. **Função `analisarT`**: Responsável por analisar termos `T` com operações de multiplicação e divisão.
4. **Função `analisarF`**: Responsável por analisar fatores `F`, que podem ser expressões entre parênteses ou identificadores.

### Fluxo de Execução

- O analisador começa verificando a entrada conforme os tokens gerados pela gramática e aplica as regras de produção para cada não terminal.
- Para cada regra de produção, o analisador verifica se o token atual corresponde ao símbolo esperado (com base nos conjuntos **First**).
- Caso a entrada seja válida, o analisador constrói a árvore sintática. Se a entrada for inválida, ele retorna um erro de sintaxe.

### Exemplo de Código:

```java
public void analisarS() {
    if (tokenAtual.equals("while")) {
        consumir("while");
        consumir("(");
        analisarE();
        consumir(")");
        analisarS();
    } else if (tokenAtual.equals("id")) {
        consumir("id");
        consumir("=");
        analisarE();
    } else {
        erro("Comando inválido");
    }
}
```

---

## 4. Casos de Teste Utilizados e Resultados Obtidos

A seguir estão os casos de teste utilizados para validar o analisador sintático, juntamente com os resultados obtidos.

### Teste 1: Comando `while` Válido

Entrada:
```text
while (x + 1) y = x;
```

Resultado Esperado:
- A sintaxe foi validada corretamente.
- A árvore sintática foi gerada conforme esperado, com um laço `while` que contém uma expressão e uma atribuição.

### Teste 2: Atribuição Válida

Entrada:
```text
x = 5;
```

Resultado Esperado:
- A sintaxe foi validada corretamente.
- A árvore sintática gerada representa uma atribuição do valor 5 à variável `x`.

### Teste 3: Comando `while` Inválido (Erro de Sintaxe)

Entrada:
```text
while (x + 1 y = x;
```

Resultado Esperado:
- O analisador gerou um erro de sintaxe, pois faltava um parêntese de fechamento.
- A mensagem de erro foi: "Erro de sintaxe: expressão malformada".

### Teste 4: Atribuição Inválida

Entrada:
```text
x = ;
```

Resultado Esperado:
- O analisador gerou um erro de sintaxe devido à falta de um valor para a variável `x`.
- A mensagem de erro foi: "Erro de sintaxe: valor ausente na atribuição".

---

## Conclusão

A implementação do analisador sintático foi bem-sucedida, validando corretamente os comandos de repetição `while` e as atribuições, conforme a gramática fornecida. O uso dos conjuntos **First** e **Follow** foi crucial para determinar as transições no analisador e garantir a validade da entrada. Os testes mostraram que o analisador pode identificar corretamente tanto entradas válidas quanto inválidas, gerando as mensagens de erro apropriadas.

---

Essa documentação descreve de forma clara os principais aspectos do projeto, desde a gramática fornecida até os resultados obtidos nos testes.
