# Analisador Sintático - Projeto de Compiladores

**Faculdade Engenheiro Salvador Arena**  
**Engenharia da Computação**  
**Disciplina:** Compiladores – Aula 8 – 1º Semestre/2025

## Integrantes

- **Kauê de Souza Silva** - RA: 081220003
- **Marcos Felipe Correa Soares** - RA: 081220020

## Descrição

Este projeto tem como objetivo a implementação de um analisador sintático (parser) em Java, utilizando os conceitos de *First* e *Follow* para validar a sintaxe de expressões e gerar a árvore sintática correspondente. O analisador foi desenvolvido a partir de uma gramática fornecida em **BNF (Backus-Naur Form)**, com o intuito de aplicar a teoria de análise sintática em compiladores.

### Objetivos:
- Implementar um analisador sintático em Java.
- Calcular e documentar os conjuntos *First* e *Follow* para a gramática fornecida.
- Validar a sintaxe de entradas válidas e inválidas.
- Gerar a árvore sintática correspondente à entrada fornecida.
  
## Gramática

Nosso grupo, número 10 trabalhou com a **gramática de comandos de repetição**:

```bnf
S → while ( E ) S | id = E
E → E + T | E - T | T
T → T * F | T / F | F
F → ( E ) | id
