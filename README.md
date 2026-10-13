# super-lula-world

Protótipos de um jogo em pixel art sobre a ditadura militar brasileira (1964–1985).
O jogador é uma pessoa comum que atravessa a história: anda pelo mapa, explora o
cenário e vive os acontecimentos de cada período.

## Protótipos (pasta `prototipos/`)

São páginas HTML independentes — abra cada arquivo direto no navegador (duplo clique,
ou arraste para uma aba). Não precisa instalar nada.

| Arquivo | O que é |
| --- | --- |
| `prototipos/vespera-do-golpe.html` | **Prólogo — A véspera (30/03/1964).** Fim de tarde dourado; o rádio, a banca, os vizinhos, o bar e um tanque na rua vão contando a tensão antes do golpe. Estilo 2.5D (perspectiva 3/4), arte gerada por código. |
| `prototipos/praca-1964.html` | **Capítulo 1 — A praça ocupada.** Primeira versão top-down do dia seguinte ao golpe. |

### Controles
- Mover: setas ou `W A S D`
- Ação / interagir: `Espaço` ou `E`
- No celular: controle direcional e botão `A` na tela

## Estado atual

Os protótipos são desenhados inteiramente em Canvas 2D (pixel art procedural: sprites
com contorno e sombra, chão pré-renderizado, iluminação de golden hour, partículas).
É a base para, num próximo passo, migrar para uma engine (ex.: Godot ou Phaser) onde
entram tilesets e spritesheets de imagem.

## Ideia geral

Um mapa por fase histórica, cada um com um desafio e o mesmo personagem envelhecendo:
véspera do golpe → 1964 → AI-5 (1968) → anos de chumbo → milagre e censura →
resistência e exílio → Diretas Já → 1985.
