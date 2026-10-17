---
name: anota
description: Registra uma nota de estudo do nand2tetris (um arquivo .md por nota) para revisão posterior. Use quando o usuário digitar /anota ou pedir para anotar/registrar algo.
---

1. Leia `~/workspace/.polis/protocol/LEVEL.yaml` para saber o módulo atual e mapeie para a pasta do projeto atual, ex.: `notes/P1-boolean-logic/`.
2. Pegue o conteúdo de $ARGUMENTS. Se vier vazio, use o último conceito discutido na conversa. Instruções embutidas no argumento (ex.: "linka com X") também valem.
3. Crie **um arquivo novo por nota**: `notes/<projeto-atual>/<slug-kebab-case>.md`. Se já existir nota sobre o mesmo conceito, complemente a existente em vez de duplicar.
4. Escreva uma nota **completa** em PT-BR (não resumida ao extremo), com termos técnicos em EN quando é assim que aparecem em código. Estrutura:
   - `# Título` + linha com data (YYYY-MM-DD), projeto e tags
   - **O que é**: definição precisa
   - **Por que importa**: onde entra no nand2tetris e na computação em geral
   - **Como funciona**: mecanismo, com tabela-verdade/código/diagrama quando couber
   - **Conexões**: interleaving com outras camadas (Java/JVM, SO, hardware real) e links para notas relacionadas
   - **Ligações**: links relativos para o material (ex.: `materials/.../lecture 1 - PT-BR.md`, citando a seção)
   - **Recall**: 2–3 perguntas que testam o conceito sem consultar
5. Se a nota se liga a um material traduzido (`lecture N - PT-BR.md`), adicione o link de volta na seção relevante ou em 🔧 Complementos desse material.
6. Atualize o índice `notes/<projeto-atual>/README.md` (uma linha por nota: link + gancho de 1 frase).
7. Confirme em 1 linha: caminho do arquivo + título da nota.
