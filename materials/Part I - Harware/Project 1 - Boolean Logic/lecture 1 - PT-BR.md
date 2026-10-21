# Lecture 1 — Lógica Booleana (PT-BR)

> Tradução de `lecture 1.pdf` — slides do Capítulo 1 de *The Elements of Computing Systems* (2ª ed.), Nisan & Schocken, MIT Press.
> Slides de navegação repetidos foram omitidos; a referência `[sN]` aponta o slide original.
> Termos técnicos ficam em inglês quando é assim que aparecem em código/ferramentas.
> Seções marcadas **🔧 Complemento** não estão nos slides — são adições nossas de estudo.

**Roteiro do capítulo** `[s2]`

| Teoria | Prática | Project 1 |
|---|---|---|
| Conceitos básicos | Portas lógicas | Introdução |
| Nand | HDL | Chips |
| | Simulação de hardware | Diretrizes |
| | Barramentos multi-bit | |

---

## Parte 1 — Teoria

### 1.1 Valores booleanos `[s3–s11]`

Rótulos diferentes, todos se referindo a **dois estados possíveis**:

| Estado | | | |
|---|---|---|---|
| apagado (*off*) | não (*no*) | `false` | `0` |
| aceso (*on*) | sim (*yes*) | `true` | `1` |

*George Boole (1815–1864).*

Quantos estados cabem em várias variáveis binárias?

- 1 variável binária: 2 estados possíveis
- 2 variáveis binárias: 4 estados possíveis
- 3 variáveis binárias: 8 estados possíveis
- …

**Pergunta:** quantos estados diferentes podem ser representados por *N* variáveis binárias?
**Resposta:** 2ᴺ

### 1.2 Funções booleanas `[s12–s22]`

Uma tabela-verdade define uma função:

| x | y | f |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

```
f(x,y) = 1  quando x == 1 e y == 1
         0  caso contrário
```

Essa função é o **And**.

**Função booleana:** uma função que opera sobre variáveis booleanas e retorna um valor booleano.

Funções booleanas simples (como And):
- às vezes são chamadas de **operadores**;
- suas variáveis são chamadas de **operandos**;
- `f(x, y)` também pode ser escrito como `x f y`.

As funções mais comuns:

| x | y | x And y | x Or y | x Nand y | x Xor y |
|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 1 | 0 |
| 0 | 1 | 0 | 1 | 1 | 1 |
| 1 | 0 | 0 | 1 | 1 | 1 |
| 1 | 1 | 1 | 1 | 0 | 0 |

| x | Not(x) |
|---|---|
| 0 | 1 |
| 1 | 0 |

… e uma função genérica `x f y` tem saídas v₁, v₂, v₃, v₄, uma para cada linha.

**Pergunta:** quantas funções booleanas `x f y` existem sobre duas variáveis binárias?
**Resposta:** 16. *N* variáveis binárias geram 2^(2ᴺ) funções booleanas.

> 🔧 De onde vem essa fórmula, passo a passo: [nota: Por que existem 16 funções](../../../notes/P1-boolean-logic/contando-funcoes-booleanas.md).

> **🔧 Complemento (§1.2.1–1.2.6):** as subseções abaixo não estão nos slides. Detalham, uma a uma, as funções da tabela acima (mais o Nor). As implementações em HDL ficam de fora de propósito, porque são o exercício do Project 1.
>
> Todas seguem o mesmo roteiro: **regra → tabela → intuição → leis → mais de 2 entradas → pegadinha → teste rápido**.

**Truque para reconhecer qualquer porta de 2 entradas:** conte quantos 1 tem a coluna de saída e veja **em qual linha** eles estão.

| Porta | Quantos 1 na saída | Linha que se destaca |
|---|---|---|
| And | 1 | o único 1 está em `a=1, b=1` |
| Or | 3 | o único 0 está em `a=0, b=0` |
| Nand | 3 | o único 0 está em `a=1, b=1` |
| Nor | 1 | o único 1 está em `a=0, b=0` |
| Xor | 2 | os 1 estão onde `a ≠ b` |

Repare: o **Nand é o And com a coluna invertida**, e o **Nor é o Or com a coluna invertida**.

#### 1.2.1 Porta lógica And 🔧

**Regra:** a saída é 1 **só quando todas** as entradas são 1. Basta um 0 na entrada para a saída ser 0.

| a | b | And(a, b) |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | **1** |

**Intuição:** é uma lista de requisitos. "Só entro se tiver crachá **e** a porta estiver destravada." Um requisito falhando já barra.

**Notação:** `a And b`, `a · b`, `ab`, `a ∧ b`. A notação de multiplicação (`·`) não é por acaso: com 0 e 1, `a · b` dá exatamente a tabela do And.

**Em Java:**
```java
boolean ok = temCracha && portaDestravada; // lógico, com short-circuit
int baixo  = x & 0xFF;                    // bit a bit: fica só com os 8 bits de baixo
```

**Físico:** duas chaves em **série**. A corrente só passa se as duas estiverem fechadas (§2.1).

**Leis (teste cada uma na tabela):**
- `a And 1 = a` → o 1 é **neutro**, deixa passar.
- `a And 0 = 0` → o 0 **domina**, zera tudo.
- `a And a = a` → repetir não muda nada.
- `a And Not(a) = 0` → uma coisa e o seu contrário nunca valem ao mesmo tempo.

As duas primeiras são a base da **máscara de bits**: And com 1 preserva o bit, And com 0 apaga o bit.

**Com mais de 2 entradas:** `And(a, b, c, …)` é 1 só se **todas** forem 1. Com N entradas, a tabela tem 2ᴺ linhas e **só uma** delas dá 1. É o And3 da §2.1, montado como `And(And(a, b), c)`.

**Pegadinha:** `Not(a And b)` **não é** `Not(a) And Not(b)`. Teste com `a=1, b=0`: o primeiro dá 1 e o segundo dá 0. O certo é `Not(a) Or Not(b)` (De Morgan, §1.4).

<details><summary>Teste rápido: quanto é <code>0b1011 & 0b0110</code>?</summary>

Bit a bit, só fica 1 onde os dois têm 1: `0b0010`.
</details>

#### 1.2.2 Porta lógica Or 🔧

**Regra:** a saída é 1 quando **pelo menos uma** entrada é 1. Só dá 0 se todas forem 0.

| a | b | Or(a, b) |
|---|---|---|
| 0 | 0 | **0** |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

**Intuição:** é um alarme com vários sensores. Basta **um** disparar para o alarme tocar.

**Notação:** `a Or b`, `a + b`, `a ∨ b`. Cuidado com o `+`: aqui `1 + 1 = 1`. Não é soma aritmética.

**Em Java:**
```java
boolean alarme = sensorPorta || sensorJanela;
int perms = perms | WRITE;   // liga o bit WRITE sem mexer nos outros
```

**Físico:** duas chaves em **paralelo**. Qualquer caminho fechado acende a lâmpada.

**Leis:**
- `a Or 0 = a` → aqui o 0 é o **neutro**.
- `a Or 1 = 1` → aqui o 1 **domina**.
- `a Or a = a`.
- `a Or Not(a) = 1` → ou é, ou não é: sempre um dos dois vale.

Compare com o And: os papéis de 0 e 1 **trocam**. Essa simetria se chama **dualidade**. Toda lei do And vira uma lei do Or trocando `And↔Or` e `0↔1`.

**Com mais de 2 entradas:** `Or(a, b, c, …)` é 1 se **qualquer** uma for 1. Das 2ᴺ linhas, **só uma** dá 0: a de tudo zero. O chip `Or8Way` do Project 1 é exatamente isso, com 8 entradas.

**Pegadinha:** é um "ou" **inclusivo**. `1 Or 1 = 1`. O "ou" do português ("café ou chá?") costuma ser exclusivo, e esse é o Xor (§1.2.6).

<details><summary>Teste rápido: <code>x | 0b0100</code> faz o quê com o bit 2 de x? E com os outros bits?</summary>

Força o bit 2 para 1 (o 1 domina). Os outros bits ficam como estavam (o 0 é neutro).
</details>

#### 1.2.3 Porta lógica Not 🔧

**Regra:** inverte a entrada. Recebe 0 e devolve 1, recebe 1 e devolve 0. É a única porta aqui com **uma entrada só**.

| in | Not(in) |
|---|---|
| 0 | 1 |
| 1 | 0 |

**Intuição:** um interruptor ao contrário. Apertou, apaga. Soltou, acende.

**Notação:** `Not(a)`, `¬a`, `a'`, `ā` (barra em cima).

**Em Java:**
```java
boolean fechado = !aberto;
int inv = ~x;   // inverte os 32 bits do int
```

**Físico:** um transistor que, quando a entrada liga, "puxa" a saída para o 0. Em CMOS é o circuito mais simples que existe: 2 transistores.

**Leis:**
- `Not(Not(a)) = a` → inverter duas vezes devolve o original.
- `Not(0) = 1` e `Not(1) = 0`.

**Pegadinha (Java):** `~5` dá **-6**, não 2. O `~` inverte **todos** os 32 bits do int, inclusive os zeros à esquerda e o bit de sinal (complemento de dois, que volta no Cap. 2). O Not só inverte os bits que existem, e um int tem 32.

<details><summary>Teste rápido: na §1.4, como sai um Not usando só Nand?</summary>

`Not(a) = Nand(a, a)`. Com as duas entradas iguais, a tabela do Nand só usa as linhas `00` e `11`, que dão 1 e 0.
</details>

#### 1.2.4 Porta lógica Nand 🔧

**Regra:** é um **And seguido de Not**. A saída só é 0 quando todas as entradas são 1.

| a | b | Nand(a, b) |
|---|---|---|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | **0** |

**Intuição:** "pelo menos uma entrada está desligada?". Se sim, a saída é 1.

**Notação:** `a Nand b`, `Not(a And b)`, `a ↑ b` (seta de Sheffer).

**Em Java:** `!(a && b)`. Não tem operador próprio.

**Físico:** em CMOS, o Nand de 2 entradas usa **4 transistores**. O And usa **6**, porque é um Nand (4) mais um Not (2) na saída. Por isso, no hardware real, o Nand é a porta "natural" e o And é derivado.

**Por que importa: universalidade.**
1. Com Nand se faz Not: `Nand(a, a)`.
2. Com Nand e Not se faz And: `Not(Nand(a, b))`.
3. Com Not e And se faz Or (De Morgan).
4. Com Not, And e Or se faz **qualquer** tabela-verdade (DNF).

Logo, só Nand basta (§1.4). No curso é a única porta que vem pronta, e todo o resto você constrói a partir dela.

**Com mais de 2 entradas:** `Nand(a, b, c)` = `Not(And(a, b, c))`. Das 2ᴺ linhas, só a de tudo 1 dá 0.

**Pegadinha:** Nand **não é associativo**. `Nand(Nand(a, b), c)` ≠ `Nand(a, Nand(b, c))`. Teste com `a=0, b=0, c=0`: o primeiro dá 1 e o segundo dá 1… agora teste com `a=0, b=1, c=1` e veja divergir. Por isso não dá para encadear Nand como se encadeia And.

<details><summary>Teste rápido: calcule os dois lados da pegadinha com a=0, b=1, c=1.</summary>

`Nand(Nand(0,1), 1) = Nand(1, 1) = 0`.
`Nand(0, Nand(1,1)) = Nand(0, 0) = 1`.
Resultados diferentes, logo não é associativo.
</details>

#### 1.2.5 Porta lógica Nor 🔧

**Regra:** é um **Or seguido de Not**. A saída só é 1 quando todas as entradas são 0.

| a | b | Nor(a, b) |
|---|---|---|
| 0 | 0 | **1** |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |

**Intuição:** "nenhum dos dois". "Nem chove, nem venta → saio de bicicleta."

**Notação:** `a Nor b`, `Not(a Or b)`, `a ↓ b` (seta de Peirce).

**Em Java:** `!(a || b)`.

**Relação com o Nand:** é o espelho dele pela dualidade.
- De Morgan: `Nor(a, b) = Not(a) And Not(b)`, e `Nand(a, b) = Not(a) Or Not(b)`.
- Também é universal: `Nor(a, a) = Not(a)`, e daí se monta o resto.

**Curiosidade:** o computador de bordo da Apollo (AGC) foi construído só com portas Nor. Usar um tipo só de porta simplificava a fabricação e o controle de qualidade. O Nor não está entre os 15 chips do Project 1.

**Pegadinha:** `Nor(a, b)` é `Not(a) And Not(b)`, **não** `Not(a) Or Not(b)`. Essa segunda é o Nand. É fácil trocar as duas.

<details><summary>Teste rápido: na tabela, Nor(a,b) é igual a And de quais duas coisas?</summary>

`And(Not(a), Not(b))`. A única linha que dá 1 é a `00`, que é justamente onde `Not(a)=1` e `Not(b)=1`.
</details>

#### 1.2.6 Porta lógica Xor 🔧

**Regra:** *eXclusive Or*. A saída é 1 quando as entradas são **diferentes**.

| a | b | Xor(a, b) |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | **1** |
| 1 | 0 | **1** |
| 1 | 1 | 0 |

**Intuição:** o interruptor de escada (*three-way switch*). Duas chaves controlam a mesma lâmpada, e mexer em **qualquer uma** inverte o estado da lâmpada.

**Notação:** `a Xor b`, `a ⊕ b`.

**Em Java:**
```java
boolean diferentes = a != b;   // para boolean, Xor é "diferente"
x = x ^ mask;                  // inverte só os bits marcados na máscara
```

**Comparando com o Or:** só a última linha muda. `1 Or 1 = 1`, mas `1 Xor 1 = 0`.

**Três leituras da mesma tabela:**
1. **Detector de diferença:** 1 quando `a ≠ b`. O inverso (Xnor) é o detector de **igualdade**.
2. **Inversor controlado:** `Xor(a, 0) = a` e `Xor(a, 1) = Not(a)`. A entrada `b` decide se `a` passa normal ou invertido.
3. **Soma de 1 bit sem o vai-um:** `1 + 1 = 10` em binário, e o bit da direita é `1 Xor 1 = 0`. Isso volta no Half Adder (Cap. 2).

**Leis:**
- `a Xor 0 = a` e `a Xor a = 0`.
- É associativo e comutativo, então a ordem não importa.
- Por causa das duas leis acima, `(a Xor b) Xor b = a`. Aplicar a mesma máscara duas vezes desfaz. É a base da criptografia de *one-time pad* e da paridade em RAID.

**Com mais de 2 entradas:** `Xor(a, b, c, …)` é 1 quando a **quantidade de 1 é ímpar**. Por isso se chama **paridade**.

**Pegadinha:** Xor de 3 entradas **não é** "exatamente uma é 1". Com `1 Xor 1 Xor 1`: `(1 Xor 1) = 0`, depois `0 Xor 1 = 1`. Três uns, quantidade ímpar, saída 1.

<details><summary>Teste rápido: escreva o Xor de 2 entradas usando só And, Or e Not.</summary>

Sem resposta aqui de propósito, porque é o `Xor.hdl` do Project 1. Dica: aplique a DNF (§1.4 e Complemento "DNF passo a passo") nas duas linhas que dão 1.
</details>

### 1.3 Avaliação de função booleana `[s23–s24]`

Avaliar `Not(x Or (y And z))` para, digamos, x = 0, y = 1, z = 1:

```
Not(0 Or (1 And 1)) =
Not(0 Or 1) =
Not(1) =
0
```

### 1.4 O poder expressivo do Nand `[s26–s28]`

**Observações** → **Portanto:**

| Observação | Conclusão |
|---|---|
| `Not(x) = x Nand x` | Not pode ser realizado com Nand |
| `x And y = Not(x Nand y)` | And pode ser realizado com Nand |
| `x Or y = Not(Not(x) And Not(y))` (De Morgan) | Or pode ser realizado com Nand |

**Teorema:** qualquer função booleana pode ser realizada usando apenas Nand.

**Prova:** qualquer função booleana pode ser expressa como tabela-verdade. Qualquer tabela-verdade pode ser expressa como função booleana usando apenas Not, And e Or (sintetizada como **DNF**, *Disjunctive Normal Form*). Combinando com as observações acima, c.q.d.

**Implicação:** qualquer computador pode ser construído apenas com portas Nand.

> **Computadores:** máquinas que realizam funções booleanas: `f(bits de entrada) = bits de saída`.

OK, então *podemos* construir um computador só com Nand. **Mas… como fazer isso de verdade?**
É disso que trata o curso Nand to Tetris!

---

## Parte 2 — Prática

### 2.1 Portas lógicas `[s31–s35]`

- **Portas elementares** (Nand, And, Or, Not, …)
- **Portas compostas** (Mux, Adder, …)

#### Portas elementares `[s32]`

| Porta | Comportamento |
|---|---|
| Nand | `if (a==1 and b==1) then out=0 else out=1` |
| And | `if (a==1 and b==1) then out=1 else out=0` |
| Or | `if (a==1 or b==1) then out=1 else out=0` |
| Not | `if (in==0) then out=1 else out=0` |

**Por que focar nessas portas em particular?**
- Porque {Nand}, ou {And, Or, Not} (e também outros subconjuntos de operadores booleanos), bastam para gerar qualquer função booleana.
- Porque elas têm implementações em hardware eficientes e diretas.

**Implementações de circuito (conceituais)** `[s33]`:
- **Circuito And:** chaves `a` e `b` em **série** com fonte e lâmpada. A lâmpada só acende se as duas estiverem fechadas.
- **Circuito Or:** chaves `a` e `b` em **paralelo**. A lâmpada acende se qualquer uma estiver fechada.

> 🔧 Essa equivalência série = And / paralelo = Or é a descoberta de Claude Shannon (1937). Ver [nota](../../../notes/P1-boolean-logic/claude-shannon.md).

#### Portas compostas `[s34–s35]`

**And3:** `if (a==1 and b==1 and c==1) then out=1 else out=0`

Implementações possíveis:
- **Física:** três chaves em série (circuito elétrico).
- **Lógica:** dois And encadeados: `And(And(a, b), c)`.

> Este curso **não** trata de implementações físicas (circuitos, transistores… isso é Engenharia Elétrica, não Ciência da Computação). Vamos focar em **implementações lógicas**.

### 2.2 HDL — Hardware Description Language `[s37–s48]`

#### Construindo um chip: o processo `[s37–s38]`

1. ✓ Projetar a arquitetura do chip
2. ✓ Especificar a arquitetura em HDL
3. ✓ Testar o chip num simulador de hardware
4. Otimizar o projeto
5. Realizar o projeto otimizado em silício

(Neste curso fazemos os passos 1–3.)

#### Projeto: requisitos `[s39]`

**Requisito:** construir um chip que entregue esta funcionalidade (Xor):

```
if ((a == 0 and b == 1) or (a == 1 and b == 0))
    out = 1
else
    out = 0
```

| a | b | out |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

**Arquivo stub HDL** (esqueleto fornecido):

```
/** out = (a And Not(b)) Or (Not(a) And b)) */
CHIP Xor {
    IN a, b;
    OUT out;
    PARTS:
    // Implementação faltando
}
```

**Conjunto de chips (APIs)** disponíveis:

```
Not (in= , out= );
And (a= , b= , out= );
Or  (a= , b= , out= );
Xor (a= , b= , out= );
...
```

#### Projeto: implementação `[s40–s45]`

**Diagrama de portas:** `a` e `b` entram; cada um passa por um Not (gerando os fios `nota` e `notb`); um And combina `a` com `notb` (fio `aAndNotb`); outro And combina `nota` com `b` (fio `notaAndb`); um Or junta os dois e produz `out`.

Do diagrama para o HDL, uma linha por porta:

```
/** out = (a And Not(b)) Or (Not(a) And b)) */
CHIP Xor {
    IN a, b;
    OUT out;
    PARTS:
    Not (in=a, out=nota);
    Not (in=b, out=notb);
    And (a=a, b=notb, out=aAndNotb);
    And (a=nota, b=b, out=notaAndb);
    Or  (a=aAndNotb, b=notaAndb, out=out);
}
```

**Pergunta do slide 44:** qual é a linha HDL que falta? → a do `Or` (slide 45).

#### Interface / Implementação `[s46]`

- `CHIP`, `IN`, `OUT` (e o comentário) = **interface da porta**
- `PARTS:` e as linhas abaixo = **implementação da porta**

Uma porta lógica tem:
- **uma** interface;
- **muitas** implementações possíveis.

#### Linguagens de descrição de hardware `[s47–s48]`

**Observações:**
- HDL é uma linguagem **funcional / declarativa**.
- Um programa HDL pode ser visto como uma especificação textual e declarativa de um diagrama de chip.
- **A ordem das instruções HDL não importa.**

**HDLs comuns:** VHDL, Verilog, …

**Nossa HDL:**
- captura a essência das outras HDLs;
- é mínima e simples;
- fornece tudo o que você precisa neste curso;
- documentação: *The Elements of Computing Systems*, **Apêndice 2: HDL**.

### 2.3 Simulação de hardware `[s50–s61]`

O código HDL é **carregado** no simulador de hardware (há a versão desktop/legada e a IDE web).

**Opções de simulação:**
- **Interativa**
- **Baseada em script**

#### Simulação interativa `[s51–s52]`

1. Carregar um programa HDL
2. Manipular os pinos de entrada
3. Avaliar a lógica do chip
4. Inspecionar os pinos de saída
5. Inspecionar os pinos internos (ex.: `nota`, `notb`, `aAndNotb`, `notaAndb`)

#### Simulação baseada em script `[s53–s54]`

`Xor.tst`:

```
load Xor.hdl;
set a 0, set b 0, eval;
set a 0, set b 1, eval;
set a 1, set b 0, eval;
set a 1, set b 1, eval;
```

**Script de teste** = sequência de comandos para o simulador.

**Benefícios:**
- testes "automáticos";
- testes replicáveis.

#### Com arquivo de saída `[s55–s59]`

```
load Xor.hdl,
output-file Xor.out,
output-list a b out;
set a 0, set b 0, eval, output;
set a 0, set b 1, eval, output;
set a 1, set b 0, eval, output;
set a 1, set b 1, eval, output;
```

`Xor.out` é o **arquivo de saída**, criado pelo script de teste como efeito colateral da simulação:

```
| a | b |out|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |
```

**A lógica de um script de teste típico:**
- **Inicializa:**
  - carrega um arquivo HDL;
  - cria um arquivo de saída vazio;
  - lista os nomes dos pinos cujos valores serão escritos no arquivo de saída.
- **Repete:**
  - `set` (entradas), `eval` (lógica do chip), `output` (imprime).

No simulador: (1) carregar o script de teste → (2) rodar o script → inspecionar o arquivo de saída.

#### Com arquivo de comparação `[s60–s61]`

```
load Xor.hdl,
output-file Xor.out,
compare-to Xor.cmp,
output-list a b out;
set a 0, set b 0, eval, output;
...
```

**Lógica da simulação com arquivo de comparação:**
- quando cada comando `output` é executado, a linha gerada é comparada com a linha correspondente do arquivo de comparação (`.cmp`);
- se as duas linhas forem diferentes, o simulador lança um **erro de comparação**.

### 2.4 Barramentos multi-bit `[s63–s72]`

- Às vezes queremos manipular uma **sequência de bits** como uma única entidade.
- Essa entidade multi-bit se chama **barramento** (*bus*).

**Exemplo: barramento de 16 bits**

```
índice: 15 14 13 12 11 10  9  8  7  6  5  4  3  2  1  0
valor:   1  0  0  0  1  0  0  0  1  1  0  1  1  1  0  1
        ↑                                            ↑
       MSB                                          LSB
```

- **MSB** (*Most significant bit*) = bit mais significativo (índice 15)
- **LSB** (*Least significant bit*) = bit menos significativo (índice 0)

#### Trabalhando com barramentos `[s64–s66]`

```
/* Soma dois valores de 16 bits. */
CHIP Adder {
    IN a[16], b[16];
    OUT out[16];
    PARTS:
    ...
}

/* Soma três entradas de 16 bits. */
CHIP Adder3Way {
    IN a[16], b[16], c[16];
    OUT out[16];
    PARTS:
    Adder(a=a,  b=b, out=ab);
    Adder(a=ab, b=c, out=out);
}
```

- Um valor de *n* bits (barramento) pode ser tratado como **uma única entidade**.
- `out=ab` cria um **pino interno de barramento** (`ab`).

#### Trabalhando com bits individuais dentro de barramentos `[s67–s72]`

```
/* And de 4 vias: faz And entre 4 bits. */
CHIP And4Way {
    IN a[4];
    OUT out;
    PARTS:
    And(a=a[0],   b=a[1], out=and01);
    And(a=and01,  b=a[2], out=and012);
    And(a=and012, b=a[3], out=out);
}
```

→ **Pinos de entrada de barramento podem ser indexados.**

```
/* And bit a bit de duas entradas de 4 bits */
CHIP And4 {
    IN a[4], b[4];
    OUT out[4];
    PARTS:
    And(a=a[0], b=b[0], out=out[0]);
    And(a=a[1], b=b[1], out=out[1]);
    And(a=a[2], b=b[2], out=out[2]);
    And(a=a[3], b=b[3], out=out[3]);
}
```

Exemplo: `a = 0101`, `b = 0011` → `out = 0001`

→ **Pinos de saída de barramento podem ser indexados.**

Dicas essenciais adicionais: *The Elements of Computing Systems*, **Apêndice 2: HDL**.

---

## Parte 3 — Project 1

### 3.1 Chips embutidos (*built-in*) `[s75]`

Fornecemos versões embutidas dos chips construídos neste curso (em `tools/builtInChips`). Por exemplo:

```
/** Faz out = a Xor b */
CHIP Xor {
    IN a, b;
    OUT out;
    BUILTIN Xor;
    // Implementado em Java / JavaScript / ...
}
```

Um chip embutido tem **a mesma interface** do chip normal, mas uma **implementação diferente** (interna, em vez de HDL).

**Simulação comportamental:**
- antes de construir um chip em HDL, é possível implementar a lógica dele numa linguagem de alto nível;
- isso permite experimentar e testar a abstração do chip antes de construí-lo de fato;
- permite planejar e testar arquiteturas de hardware em alto nível.

### 3.2 Projetos de construção de hardware `[s76]`

**Papéis principais:**
- **Arquiteto:** decide quais chips são necessários e especifica os chips.
- **Desenvolvedores:** constroem e testam os chips.

**No Nand to Tetris:** o arquiteto é a equipe do curso; os desenvolvedores são os alunos.

Para cada chip, o arquiteto fornece:
- a API do chip (programa HDL esqueleto = *stub file*);
- o script de teste;
- o arquivo de comparação;
- o chip embutido.

**Com esses recursos, os desenvolvedores (alunos) constroem os chips.**

### 3.3 A visão do desenvolvedor `[s77–s78]`

Os arquivos especificam:
- a interface do chip (`.hdl`);
- como o chip deve se comportar (`.cmp`);
- como testar o chip (`.tst`).

**Tarefa do desenvolvedor:** implementar o chip (completar o arquivo `.hdl` fornecido) usando esses recursos.

### 3.4 Os 15 chips `[s79–s81]`

Na plataforma de hardware, cada nível usa o de baixo: Nand → **portas lógicas elementares (p1)** → ALU, RAM (p2, p3) → computador (p4, p5) ← assembler (p6) ← linguagem de máquina.

**Dado:** Nand
**Objetivo:** construir as seguintes portas:

| Portas lógicas elementares | Variantes de 16 bits | Variantes multi-via |
|---|---|---|
| Not | Not16 | Or8Way |
| And | And16 | Mux4Way16 |
| Or | Or16 | Mux8Way16 |
| Xor | Mux16 | DMux4Way |
| Mux | | DMux8Way |
| DMux | | |

**Por que essas 15 portas em particular?**
- São portas de uso comum.
- Incluem todas as portas lógicas elementares necessárias para construir nosso computador.

### 3.5 Multiplexador / Demultiplexador `[s83–s88]`

```
Mux:                          DMux:
if (sel == 0)                 if (sel == 0)
    out = a                       {a, b} = {in, 0}
else                          else
    out = b                       {a, b} = {0, in}
```

**Muito usados em:**
- projeto de hardware;
- redes de comunicação.

#### Exemplo 1: usando Mux para construir uma porta programável `[s84–s85]`

```
if (sel == 0)
    out = a And b
else
    out = a Or b
```

| a | b | sel | out |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 0 |
| 1 | 0 | 0 | 0 |
| 1 | 1 | 0 | 1 |
| 0 | 0 | 1 | 0 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 |

Com `sel == 0` a porta age como **And**; com `sel == 1`, como **Or**.

```
CHIP AndMuxOr {
    IN a, b, sel;
    OUT out;
    PARTS:
    And (a=a, b=b, out=andOut);
    Or  (a=a, b=b, out=orOut);
    Mux (a=andOut, b=orOut, sel=sel, out=out);
}
```

#### Exemplo 2: usando Mux para construir um canal intercalado `[s86]`

Um Mux na origem e um DMux no destino, com os `sel` alimentados por osciladores que alternam entre 0 e 1. As duas mensagens (`…1011` e `…0110`) viajam intercaladas numa única linha (`…01101101`) e são separadas de novo no destino.

- Permite transmitir várias mensagens ao mesmo tempo usando uma única linha de comunicação compartilhada.
- *É só conceitual e não tem relação com este curso.*

#### Multiplexador `[s87]`

| a | b | sel | out |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 0 |
| 1 | 0 | 0 | 1 |
| 1 | 1 | 0 | 1 |
| 0 | 0 | 1 | 0 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 1 | 1 |

**Tabela-verdade abreviada:**

| sel | out |
|---|---|
| 0 | a |
| 1 | b |

```
CHIP Mux {
    IN a, b, sel;
    OUT out;
    PARTS:
    // Coloque seu código aqui:
}
```

**Dica de implementação:** pode ser implementado com And, Or e Not.

#### Demultiplexador `[s88]`

| in | sel | a | b |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |

- É o "inverso" de um multiplexador.
- Encaminha o único valor de entrada para um de dois destinos possíveis.

```
CHIP DMux {
    IN in, sel;
    OUT a, b;
    PARTS:
    // Coloque seu código aqui:
}
```

**Dica de implementação:** tabela-verdade simples, implementação simples.

### 3.6 And16 `[s91]`

```
a   = 1 0 1 0 1 0 1 1 0 1 0 1 1 1 0 0
b   = 0 0 1 0 1 1 0 1 0 0 1 0 1 0 1 0
-------------------------------------
out = 0 0 1 0 1 0 0 1 0 0 0 0 1 0 0 0
```

```
CHIP And16 {
    IN a[16], b[16];
    OUT out[16];
    PARTS:
    // Coloque seu código aqui:
}
```

**Dica de implementação:** é uma extensão direta, para 16 bits, da porta And elementar (veja a documentação HDL sobre barramentos multi-bit).

### 3.7 Multiplexador de 16 bits e 4 vias `[s94]`

| sel[1] | sel[0] | out |
|---|---|---|
| 0 | 0 | a |
| 0 | 1 | b |
| 1 | 0 | c |
| 1 | 1 | d |

```
CHIP Mux4Way16 {
    IN a[16], b[16], c[16], d[16],
       sel[2];
    OUT out[16];
    PARTS:
    // Coloque seu código aqui:
}
```

**Dica de implementação:** pode ser construído com portas Mux16.

### 3.8 Diretrizes: arquivos `[s97–s98]`

Para cada chip do curso (usando o Xor como exemplo), fornecemos três arquivos: `Xor.hdl` (stub), `Xor.tst` e `Xor.cmp`.

**O contrato:**
> Ao rodar o seu `Xor.hdl` com o `Xor.tst` fornecido, o seu `Xor.out` deve ser igual ao `Xor.cmp` fornecido.

- **Pasta do Project 1** (arquivos `.hdl`, `.tst`, `.cmp`): `nand2tetris/projects/1`
- **Ferramentas:**
  - editor de texto (para completar os `.hdl`);
  - simulador de hardware: `nand2tetris/tools`.

### 3.9 Interfaces dos chips `[s99–s100]`

*"Se eu quero usar alguns chips como peças, como descubro as assinaturas deles?"*

**API do conjunto de chips Hack:** abra numa janela e copie e cole as assinaturas no seu HDL quando precisar.

```
Add16 (a= ,b= ,out= );
ALU (x= ,y= ,zx= ,nx= ,zy= ,ny= ,f= ,no= ,out= ,zr= ,ng= );
And16 (a= ,b= ,out= );
And (a= ,b= ,out= );
ARegister (in= ,load= ,out= );
Bit (in= ,load= ,out= );
CPU (inM= ,instruction= ,reset= ,outM= ,writeM= ,addressM= ,pc= );
DFF (in= ,out= );
DMux4Way (in= ,sel= ,a= ,b= ,c= ,d= );
DMux8Way (in= ,sel= ,a= ,b= ,c= ,d= ,e= ,f= ,g= ,h= );
DMux (in= ,sel= ,a= ,b= );
DRegister (in= ,load= ,out= );
FullAdder (a= ,b= ,c= ,sum= ,carry= );
HalfAdder (a= ,b= ,sum= ,carry= );
Inc16 (in= ,out= );
Keyboard (out= );
Memory (in= ,load= ,address= ,out= );
Mux16 (a= ,b= ,sel= ,out= );
Mux4Way16 (a= ,b= ,c= ,d= ,sel= ,out= );
Mux8Way16 (a= ,b= ,c= ,d= ,e= ,f= ,g= ,h= ,sel= ,out= );
Mux8Way (a= ,b= ,c= ,d= ,e= ,f= ,g= ,h= ,sel= ,out= );
Mux (a= ,b= ,sel= ,out= );
Nand (a= ,b= ,out= );
Not16 (in= ,out= );
Not (in= ,out= );
Or16 (a= ,b= ,out= );
Or8Way (in= ,out= );
Or (a= ,b= ,out= );
PC (in= ,load= ,inc= ,reset= ,out= );
PCLoadLogic (cinstr= ,j1= ,j2= ,j3= ,load= ,inc= );
RAM16K (in= ,load= ,address= ,out= );
RAM4K (in= ,load= ,address= ,out= );
RAM512 (in= ,load= ,address= ,out= );
RAM64 (in= ,load= ,address= ,out= );
RAM8 (in= ,load= ,address= ,out= );
Register (in= ,load= ,out= );
ROM32K (address= ,out= );
Screen (in= ,load= ,address= ,out= );
Xor (a= ,b= ,out= );
```

### 3.10 Usando chips embutidos `[s101]`

**P:** como posso brincar com um chip, ou usá-lo como peça, antes de implementá-lo?
**R:** o simulador tem implementações embutidas dos chips.

Para forçar o simulador a usar a versão embutida de uma peça, digamos `Bar`:

**Na IDE web:** quando a peça não tem implementação HDL, o simulador usa a versão embutida.

**No simulador de hardware desktop:**
- normalmente `Bar.hdl` é um stub, ou um arquivo com implementação incompleta;
- para usar `Bar` como chip embutido, remova `Bar.hdl` da pasta do projeto (ou renomeie, por exemplo, para `Bar1.hdl`);
- resultado: sempre que `Bar` for usado como peça em algum chip, o simulador não vai encontrar `Bar.hdl` na pasta atual e vai usar a versão embutida.

### 3.11 E agora? `[s102–s103]`

- **Project / Capítulo 1:** construir 15 portas lógicas elementares.
- **Próximo, Project / Capítulo 2:** construir chips que fazem aritmética, terminando numa **ALU**, usando os chips construídos no Project 1.

---

## 🔧 Complementos

> Adições nossas ao longo do estudo. Cada entrada traz a data e a qual seção da lecture se liga.

### [2026-10-16] Por que `nand(a, a)` é um Not → §1.4
`And(a, a) = a` (a=0 → 0; a=1 → 1). Então `Nand(a, a) = Not(And(a, a)) = Not(a)`. Ligar as duas entradas do Nand no mesmo fio transforma ele num inversor.

### [2026-10-16] DNF passo a passo → §1.4
Pra transformar uma tabela-verdade em expressão: (1) pegue cada linha em que a saída é 1; (2) monte um And das entradas dessa linha, com Not em cada entrada que vale 0; (3) junte todos esses Ands com Or.
Exemplo com o Xor (linhas `01` e `10`): `(Not(a) And b) Or (a And Not(b))`. É exatamente a implementação do slide 45.

### [2026-10-16] Short-circuit: software ≠ hardware → §1.3
O `&&` do Java não avalia o lado direito se o esquerdo já for `false`. No hardware isso não existe: todas as portas calculam o tempo todo, em paralelo, porque são fios fixos. HDL não tem `if` nem fluxo de execução, por isso **a ordem das linhas não importa** (§2.2).

### [2026-10-16] De Morgan no dia a dia → §1.4
`!(a && b) == (!a || !b)` e `!(a || b) == (!a && !b)`. Toda vez que você simplifica a condição de um `if` negado em Java, está usando De Morgan.
Ver nota: [Leis de De Morgan](../../../notes/P1-boolean-logic/leis-de-de-morgan.md) (com prova por tabela-verdade).

### [2026-10-16] Por que computadores são binários → §1.1
Em 1937, Claude Shannon mostrou na tese de mestrado (MIT) que a álgebra de Boole descreve circuitos de relés e chaves. Foi aí que a lógica do século XIX virou engenharia elétrica, e é a base de todo hardware digital.
Ver notas: [George Boole](../../../notes/P1-boolean-logic/george-boole.md) · [Claude Shannon](../../../notes/P1-boolean-logic/claude-shannon.md).

### [2026-10-16] Java ↔ Nand2Tetris
`.java` = Programa (Jack) · `javac` = compilador (P10–11) · `.class`/bytecode = código de VM · JVM = VM translator (P7–8), só que traduz **em tempo de execução**, com JIT.
