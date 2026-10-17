# Claude Shannon: da álgebra de Boole ao circuito

`2026-10-16` · P1 Boolean Logic · #historia #circuitos #teoria-da-informacao

## O que é

Claude Shannon (1916–2001) foi um matemático e engenheiro elétrico americano. Duas contribuições dele fundaram áreas inteiras:

1. **1937, tese de mestrado no MIT** (*A Symbolic Analysis of Relay and Switching Circuits*): mostrou que a álgebra de Boole descreve exatamente circuitos de chaves e relés. É chamada com frequência de a tese de mestrado mais importante do século XX.
2. **1948, na Bell Labs** (*A Mathematical Theory of Communication*): fundou a **teoria da informação**. Definiu como medir informação e popularizou o **bit** (*binary digit*, termo cunhado por John Tukey) como a unidade fundamental.

## Por que importa

Antes de Shannon, quem projetava circuitos de relés (centrais telefônicas, por exemplo) trabalhava na base da intuição e da tentativa e erro. Depois dele, circuitos passaram a ser **projetados e simplificados com álgebra**: escreve a expressão booleana, simplifica e desenha o circuito. É exatamente o que você faz no nand2tetris (tabela-verdade → expressão → HDL).

## Como funciona

A equivalência central (slide 33 da lecture 1):

```
Chaves em SÉRIE → And
  fonte ──/ a ──/ b ── lâmpada
  a lâmpada só acende se a E b estiverem fechadas

Chaves em PARALELO → Or
  fonte ─┬─/ a ─┬─ lâmpada
         └─/ b ─┘
  a lâmpada acende se a OU b estiver fechada
```

| Estado elétrico | Valor booleano |
|---|---|
| chave fechada / corrente passa | 1 |
| chave aberta / sem corrente | 0 |

Os relés de 1937 viraram válvulas, depois transistores. O princípio continua o mesmo: um transistor é uma chave controlada por tensão, e portas lógicas são combinações de chaves.

## Conexões

- **Boole → Shannon → nand2tetris:** Boole deu a álgebra, Shannon a ligou ao circuito, e o nand2tetris faz você percorrer esse caminho. Ver [George Boole](george-boole.md).
- **Teoria da informação no seu dia a dia:** compressão (gzip, os payloads no Kong), códigos de correção de erro (TCP checksum, ECC na RAM de servidor) e o limite de capacidade de um canal de rede vêm todos do trabalho de 1948.
- **Bit:** é a unidade que você já usa (8 bits = 1 byte = 2⁸ = 256 valores).

## Ligações

- [lecture 1 - PT-BR.md](../../materials/Part%20I%20-%20Harware/Project%201%20-%20Boolean%20Logic/lecture%201%20-%20PT-BR.md), §2.1 Portas lógicas (circuitos And/Or, slide 33) e 🔧 "Por que computadores são binários"

## Recall

1. Qual ponte Shannon construiu em 1937? Como ficam And e Or num circuito de chaves?
2. Qual foi a segunda contribuição dele (1948), e que unidade ela popularizou?
3. O que um transistor tem a ver com as chaves de Shannon?
