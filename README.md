# my-nand2tetris

Construindo um computador do zero, partindo só da porta NAND, seguindo o curso [nand2tetris](https://www.nand2tetris.org) (livro *The Elements of Computing Systems*, Nisan & Schocken).

## Sessão de estudo

Seg, qui e sex, das 06:40 às 07:30. Um chip por sessão.

1. `git pull`, depois `git log -1` para ver onde parei.
2. Implementar o chip em `projects/<N>/<Chip>.hdl`.
3. Rodar o `.tst` até passar.
4. Commitar, mesmo sem terminar. A mensagem diz o próximo passo:
   `wip(P1/Mux): falta saída quando sel=1`
5. `git push`.

## Como testar um chip

- **Navegador:** [IDE web do nand2tetris](https://nand2tetris.github.io/web-ide). Não precisa instalar nada.
- **Local (precisa de Java):** `tools/HardwareSimulator.sh projects/1/Not.tst`. No Windows, use o `.bat`.

## Estrutura

```
projects/   chips (.hdl), testes (.tst) e saídas esperadas (.cmp)
notes/      notas de estudo por projeto
materials/  material do curso; os PDFs ficam fora do git (copyright)
tools/      simuladores oficiais
```

## Progresso

### P1: Boolean Logic

- [ ] Not
- [ ] And
- [ ] Or
- [ ] Xor
- [ ] Mux
- [ ] DMux
- [ ] Not16
- [ ] And16
- [ ] Or16
- [ ] Mux16
- [ ] Or8Way
- [ ] Mux4Way16
- [ ] Mux8Way16
- [ ] DMux4Way
- [ ] DMux8Way

### P2: Boolean Arithmetic (gate M0)

- [ ] HalfAdder
- [ ] FullAdder
