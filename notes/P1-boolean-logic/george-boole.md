# George Boole: o pai da álgebra booleana

`2026-10-16` · P1 Boolean Logic · #historia #algebra-booleana

## O que é

George Boole (1815–1864) foi um matemático inglês autodidata, filho de um sapateiro, que nunca fez universidade. Mesmo assim chegou a professor no Queen's College Cork, na Irlanda. Em *The Mathematical Analysis of Logic* (1847) e principalmente em *An Investigation of the Laws of Thought* (1854), ele transformou a **lógica**, que vinha de Aristóteles e era estudada com palavras, em **álgebra**: proposições viram variáveis que só valem 0 (falso) ou 1 (verdadeiro), e o raciocínio vira cálculo com operações (E, OU, NÃO).

## Por que importa

Tudo o que o nand2tetris faz se apoia nessa álgebra. As portas lógicas são funções booleanas implementadas fisicamente, e o tipo `boolean` do Java tem esse nome por causa dele.

Na época foi matemática pura, sem aplicação. Passaram-se ~80 anos até que Claude Shannon (1937) mostrasse que essa álgebra descreve circuitos elétricos. Daí nasceu o hardware digital.

## Como funciona

A álgebra de Boole se parece com a álgebra comum, mas as variáveis só valem 0 ou 1:

| Operação | Símbolo de Boole | Nome hoje | Java |
|---|---|---|---|
| E | x · y | And | `x && y` |
| OU | x + y | Or | `x \|\| y` |
| NÃO | 1 − x | Not | `!x` |

A **lei característica** de Boole, que não vale na álgebra comum:

```
x · x = x     (só é verdade se x ∈ {0, 1}: 0·0 = 0, 1·1 = 1)
```

Em portas lógicas, essa lei é `And(a, a) = a`, e é exatamente por isso que `Nand(a, a) = Not(And(a, a)) = Not(a)`. Foi o primeiro chip que você raciocinou.

## Conexões

- **Shannon** pegou essa álgebra e aplicou a relés e chaves: série = And, paralelo = Or. Ver [Claude Shannon](claude-shannon.md).
- **De Morgan**, contemporâneo e amigo de Boole, formalizou as leis de negação de expressões compostas. Ver [Leis de De Morgan](leis-de-de-morgan.md).
- Em software: todo `if`, todo filtro `WHERE` no SQL e todo predicado em `stream().filter()` é álgebra booleana.

## Ligações

- [lecture 1 - PT-BR.md](../../materials/Part%20I%20-%20Harware/Project%201%20-%20Boolean%20Logic/lecture%201%20-%20PT-BR.md), §1.1 Valores booleanos (slide 5) e 🔧 "Por que computadores são binários"

## Recall

1. O que Boole criou, e por que essa criação ficou quase 80 anos sem uso prático?
2. Qual lei de Boole não vale na álgebra comum, e como ela explica que `Nand(a, a)` é um Not?
3. Como ficam E, OU e NÃO na notação de Boole?
