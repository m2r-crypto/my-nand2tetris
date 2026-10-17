# Nand2Tetris: o quê, por quê, e o Not.hdl

`2026-10-16` · P1 Boolean Logic · #visao-geral #abstracao #nand

## O que é

Um curso/livro (*The Elements of Computing Systems*, Nisan & Schocken) em que você constrói um computador completo partindo de uma única porta lógica, a **NAND**, e sobe camada por camada até um sistema que roda Tetris. São 12 projetos: 6 de hardware (Parte I) e 6 de software (Parte II).

## Por que importa

O computador só executa **linguagem de máquina** (bits). Entre o código-fonte e a física existe uma pilha de camadas, e cada uma é construída usando só a camada de baixo:

```
Programa (Jack, parecido com Java)        ← P9
   ↓ compilador                           ← P10, P11
Código de VM (bytecode)                   ← P7, P8
   ↓ VM translator
Assembly / linguagem de máquina           ← P4
   ↓ assembler                            ← P6
Computador: CPU + RAM                     ← P5
   ↑ feito de
ALU, registradores, memória               ← P2, P3
   ↑ feitos de
Portas lógicas: And, Or, Not, Mux...      ← P1
   ↑ feitas de
NAND                                      ← dado pronto
   ↑ feito de
Transistores (aqui acaba computação e começa física)
```

Depois de construir cada camada, nada nessa pilha fica "mágico".

## Como funciona

**Por que NAND:** toda porta lógica pode ser construída só com NAND, e a indústria usa isso porque NAND é barato de fabricar com transistores. O curso te dá só o NAND, e o resto você monta.

**Princípio central: abstração × implementação.**
- **Abstração** é *o quê* o módulo faz. No `.hdl`, é a interface: `IN`, `OUT` e o comentário.
- **Implementação** é *como* ele faz. No `.hdl`, é o bloco `PARTS:`.
- Quando você usa um chip, olha só a interface e ignora como ele foi feito.

**Fluxo de trabalho:** HDL + simulador, como fazem os engenheiros de hardware de verdade (Verilog/VHDL). O `.tst` funciona como um teste unitário (o "JUnit do chip") e o `.cmp` traz a saída esperada. Se o teste passa, o chip está certo.

**O Not.hdl:** é o primeiro chip do Project 1. A tarefa é implementar a interface "Not" usando só NAND:

```
not(a) = nand(a, a)
// porque and(a,a) = a  →  nand(a,a) = not(and(a,a)) = not(a)
```

## Conexões

| Java | Nand2Tetris |
|---|---|
| `.java` | Programa (Jack) |
| `javac` | compilador (P10–11) |
| `.class` (bytecode) | código de VM |
| JVM | VM translator (P7–8), mas traduz **em runtime**, com JIT |

`interface` + `class` em Java é a mesma separação abstração × implementação que existe em cada chip.

## Ligações

- Introdução da Parte I: `materials/Part I - Harware/part I intro.pdf`
- [lecture 1 - PT-BR.md](../../materials/Part%20I%20-%20Harware/Project%201%20-%20Boolean%20Logic/lecture%201%20-%20PT-BR.md), §1.4 (poder do Nand) e §2.2 (interface/implementação)
- Relacionadas: [George Boole](george-boole.md) · [Claude Shannon](claude-shannon.md)

## Recall

1. Por que o curso começa do NAND e não de outra porta?
2. Por que `nand(a,a)` é um NOT? Prove pela tabela-verdade.
3. No `.hdl`, o que é interface e o que é implementação? Qual o equivalente em Java?
