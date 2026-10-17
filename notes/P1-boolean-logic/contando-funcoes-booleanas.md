# Por que existem 16 funções booleanas de 2 variáveis

`2026-10-17` · P1 Boolean Logic · #contagem #funcao-booleana #tabela-verdade

## O que é

A lecture (slide 22) afirma: *N variáveis binárias geram 2^(2ᴺ) funções booleanas*. Com 2 variáveis, isso dá **16**. Esta nota explica **de onde vem** essa fórmula, que a lecture só enuncia.

## Por que importa

- Mostra que And, Or, Xor e Nand **não são as únicas** funções. São 4 de 16, as que ganharam nome por serem úteis.
- Justifica o teorema do Nand: como dá pra **listar e contar** todas as funções, faz sentido provar que *todas* saem de Nand.
- Treina a contagem binária que aparece o curso inteiro: endereços de RAM, tamanho de registrador, opcodes.

## Como funciona

### Peça 1: o princípio multiplicativo

Escolhas independentes **se multiplicam**. Com 3 camisas e 2 calças, dá pra montar 3 × 2 = 6 roupas. Com 4 decisões de sim/não, são 2 × 2 × 2 × 2 = 16 possibilidades.

### Peça 2: quantas linhas a tabela tem (1º uso do princípio)

Cada **variável de entrada** é uma escolha com 2 opções. Com N variáveis, são 2 × 2 × … × 2 = **2ᴺ linhas**.

```
N=1 → 2 linhas    N=2 → 4 linhas    N=3 → 8 linhas
```

⚠️ **Erro já cometido:** 3 variáveis **não** dão 3 × 2 = 6 linhas. São 3 escolhas, cada uma com 2 opções, então 2 × 2 × 2 = 8. A cada variável nova, a lista de linhas **dobra**, não soma 2.

### Peça 3: o que é uma função (a peça que faltava)

Uma função **não** é algo pré-definido por alguém. É **qualquer regra que dá exatamente uma saída para cada entrada**. Na lógica booleana, a função **é a coluna de saída**, e nada mais.

- Duas fórmulas com a mesma coluna são **a mesma função**: `Not(Not(x))` e `x` dão a coluna `01`, então são iguais.
- Toda condição de `if` que você escreve é uma função booleana que **você inventou**.

**Exemplo real (inventado na sessão):**

```java
// "solicita pós-conciliação se NÃO existe movimentado aguardando E a conciliação já liberou"
boolean solicitarPosConciliacao(boolean existeMovimentadoJaAguardando,
                                boolean conciliacaoJaLiberouExecucao) {
    return !existeMovimentadoJaAguardando && conciliacaoJaLiberouExecucao;
}
```

| x (aguardando) | y (liberou) | saída |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | **1** |
| 1 | 0 | 0 |
| 1 | 1 | 0 |

A coluna de saída é **`0100`**. Não é And, nem Or, nem Xor: é uma das 12 funções sem nome famoso.

### Peça 4: quantas funções existem (2º uso do princípio)

Criar uma função é **decidir a saída de cada linha**. Com 4 linhas, são 4 decisões, cada uma 0 ou 1:

```
decidi a linha 00 → 2
decidi a linha 01 → 2 × 2 = 4
decidi a linha 10 → 4 × 2 = 8
decidi a linha 11 → 8 × 2 = 16
```

Ou seja, cada função é **um número de 4 bits**, e cabem **16** números em 4 bits (de `0000` a `1111`).

**Analogia do gabarito:** uma prova de V/F com 4 questões tem 16 gabaritos possíveis. As questões são as linhas, e cada gabarito é uma função.

### A fórmula, montada

```
linhas  = 2ᴺ           ← princípio multiplicativo nas ENTRADAS
funções = 2^(linhas)   ← princípio multiplicativo nas SAÍDAS
        = 2^(2ᴺ)
```

O "2" aparece duas vezes porque o mesmo princípio é usado duas vezes.

### Generalização: os dois "2" têm papéis diferentes

> Complemento de `2026-10-17` (recall da sessão).

Na lógica booleana, entrada e saída são binárias, então as duas bases valem 2. Isso **esconde** que os dois 2 têm significados diferentes. A fórmula geral separa os dois:

```
linhas  = (valores por entrada)^(nº de entradas)
funções = (valores da saída)^(linhas)
```

Cada lado da tabela gera um dos números:

```
 a  b | saída
 0  0 |  ?      ← lado ESQUERDO (o que cabe em a, b) gera as LINHAS
 0  1 |  ?      ← lado DIREITO (o que cabe em cada ?) gera as FUNÇÕES
 1  0 |  ?
 1  1 |  ?
```

| entradas | valores por entrada | valores da saída | linhas | funções |
|---|---|---|---|---|
| 2 | 2 | 2 | 2² = 4 | 2⁴ = 16 |
| 3 | 2 | 2 | 2³ = 8 | 2⁸ = 256 |
| 2 | 2 | **3** | 2² = 4 | 3⁴ = **81** |
| 3 | 2 | **3** | 2³ = 8 | 3⁸ = **6561** |
| 2 | **3** | **3** | 3² = 9 | 3⁹ = 19683 |

⚠️ **Erro já cometido:** com saída de 0 a 2 e entradas binárias, respondi 3^(3²) = 19683. A conta estava certa, mas para outra pergunta: troquei a base **também nas entradas**. Mudar os valores da saída só mexe no expoente de fora (a base das funções). As linhas continuam 2ᴺ enquanto as entradas forem binárias.

### Mapa das 16 (colunas nas linhas 00, 01, 10, 11)

| coluna | função |
|---|---|
| `0000` | sempre 0 |
| `0001` | And |
| `0100` | `!x && y` (o exemplo acima) |
| `0110` | Xor (luz da escada com 2 interruptores) |
| `0111` | Or |
| `1110` | Nand |
| `1111` | sempre 1 |
| … | outras 9 sem nome famoso |

E com **1 variável**, onde dá pra listar todas: `00` sempre 0, `01` copia x, `10` Not, `11` sempre 1. São 4 = 2^(2¹).

## Conexões

- **Java:** uma função `boolean f(boolean x, boolean y)` equivale a um `boolean[4]` de consulta (*lookup table*). Contar funções é contar quantos `boolean[4]` diferentes existem.
  ```java
  boolean[] tabela = {false, true, false, false};   // 0100
  boolean f(boolean x, boolean y) { return tabela[(x ? 2 : 0) + (y ? 1 : 0)]; }
  ```
- **Hardware real:** FPGAs implementam lógica exatamente assim, com **LUTs** (*Look-Up Tables*): uma pequena memória que guarda a coluna de saída. Reprogramar o chip é trocar o conteúdo da LUT.
- **Byte:** 8 bits dão 2⁸ = 256 valores. É a mesma conta, só que aplicada à coluna de saída.
- Relacionadas: [Nand2Tetris: visão geral](nand2tetris-visao-geral.md) · [Leis de De Morgan](leis-de-de-morgan.md)

## Armadilha: maior valor ≠ quantidade de valores (*off-by-one*)

Esse erro já apareceu duas vezes:
- Byte: o maior valor é **255**, mas a quantidade é **256**.
- 4 bits: o maior valor é **15** (`1111`), mas a quantidade é **16**.

A contagem começa em `0`. Quantidade = maior valor + 1 = 2ⁿ.

## Ligações

- [lecture 1 - PT-BR.md](../../materials/Part%20I%20-%20Harware/Project%201%20-%20Boolean%20Logic/lecture%201%20-%20PT-BR.md), §1.1 (2ᴺ estados) e §1.2 (as 16 funções, slide 22)

## Recall

1. Com **3 variáveis**: quantas linhas a tabela tem? A coluna de saída tem quantos bits? Quantas funções existem? (Pista: a resposta tem tudo a ver com o byte.)
2. Por que `Not(Not(x))` e `x` são a mesma função?
3. Invente uma regra de negócio com 2 condições, escreva a coluna de saída e diga se ela é uma das funções com nome.
4. Entradas binárias, 4 variáveis, saída podendo ser 0, 1 ou 2: quantas linhas e quantas funções? Qual base mudou e qual não mudou?
5. Na fórmula 2^(2ᴺ), o que cada um dos dois "2" representa?
