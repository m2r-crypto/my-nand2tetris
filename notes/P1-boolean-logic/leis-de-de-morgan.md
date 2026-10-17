# Leis de De Morgan

`2026-10-16` · P1 Boolean Logic · #algebra-booleana #nand #refactoring

## O que é

São duas regras para **negar uma expressão composta**: o Not "entra" em cada parte e o operador troca, And vira Or e Or vira And.

```
Not(a And b) = Not(a) Or  Not(b)
Not(a Or  b) = Not(a) And Not(b)
```

Levam o nome de Augustus De Morgan (1806–1871), matemático britânico contemporâneo e correspondente de Boole.

## Por que importa

No nand2tetris, é a ponte que permite montar o **Or a partir de Not e And**, e portanto só de NAND (lecture §1.4, slide 26):

```
a Or b = Not(Not(a) And Not(b))
```

Sem De Morgan, o teorema "qualquer função sai de NAND" fica com uma lacuna: você teria Not e And, mas não Or.

## Como funciona

**Prova por tabela-verdade (1ª lei):** as colunas destacadas são iguais em todas as linhas.

| a | b | a And b | **Not(a And b)** | Not a | Not b | **Not a Or Not b** |
|---|---|---|---|---|---|---|
| 0 | 0 | 0 | **1** | 1 | 1 | **1** |
| 0 | 1 | 0 | **1** | 1 | 0 | **1** |
| 1 | 0 | 0 | **1** | 0 | 1 | **1** |
| 1 | 1 | 1 | **0** | 0 | 0 | **0** |

**Derivando o Or (2ª lei aplicada):**

```
Not(a Or b) = Not(a) And Not(b)        ← 2ª lei
a Or b      = Not(Not(a) And Not(b))   ← nega os dois lados
```

**Em HDL**, isso vira 4 peças (2 Not, 1 And, 1 Not), ou 3 Nands depois de simplificar:

```
a Or b = Nand(Not(a), Not(b))
```

## Conexões

- **Java:** é o refactoring que você já faz em condições.
  ```java
  if (!(user != null && user.isActive()))  // difícil de ler
  if (user == null || !user.isActive())    // De Morgan: mesmo comportamento
  ```
- **SQL:** `NOT (a AND b)` equivale a `NOT a OR NOT b`. O otimizador do banco usa essas leis para reescrever filtros.
- **Hardware real:** projetistas usam De Morgan para trocar portas e aproveitar o tipo mais barato disponível (NAND/NOR em CMOS).

## Ligações

- [lecture 1 - PT-BR.md](../../materials/Part%20I%20-%20Harware/Project%201%20-%20Boolean%20Logic/lecture%201%20-%20PT-BR.md), §1.4 O poder expressivo do Nand e 🔧 "De Morgan no dia a dia"
- Relacionada: [George Boole](george-boole.md)

## Recall

1. Escreva as duas leis de De Morgan sem consultar.
2. Usando uma delas, como construir um Or tendo só Not e And? E só com Nand?
3. Reescreva `!(a || b)` em Java sem a negação externa.
