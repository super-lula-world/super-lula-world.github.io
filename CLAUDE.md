# super-lula-world — contexto do projeto

Jogo em **pixel art** sobre a **ditadura militar brasileira (1964–1985)**. O jogador é
uma pessoa comum que **atravessa a história**: anda pelo mapa, explora o cenário e vive
os acontecimentos de cada período. Imersão histórica + um desafio por fase.

> Idioma do projeto: **português (Brasil)**. Textos de interface, diálogos e commits em pt-BR.

## Estrutura

```
index.html                       Página inicial que lista os protótipos (serve p/ GitHub Pages)
prototipos/
  vespera-do-golpe.html          Prólogo — a véspera do golpe (30/03/1964). Versão mais recente/completa.
  praca-1964.html                Capítulo 1 — a praça ocupada (primeira versão top-down).
README.md
```

Cada protótipo é um **HTML único e autossuficiente** (CSS + JS inline, sem build e sem
dependências externas). Abrir direto no navegador funciona.

## Como rodar

- Abrir o arquivo HTML no navegador (duplo clique), ou
- No VS Code: extensão **Live Server** → botão direito no arquivo → *Open with Live Server*.

Não há passo de build, nem `npm install`. Se um dia migrarmos pra engine, isto muda.

## Como o render funciona hoje (importante antes de editar)

Tudo é desenhado em **Canvas 2D com pixel art procedural** — não há imagens externas.
Decisões já tomadas, manter a coerência:

- **Perspectiva 2.5D (3/4)**, estilo Stardew/SNES: chão visto de cima, casas/árvores com
  fachada e altura.
- **Sprites "assados" uma vez** em canvas offscreen e depois desenhados (`drawImage`),
  com **contorno escuro** (`OL`) e **sombra interna**. Personagens são bitmaps 16×32 com
  4 direções × 4 frames de caminhada + idle (respiração/piscar).
- **Chão pré-renderizado** num canvas do tamanho do mapa (`ground`), desenhado uma vez e
  recortado pela câmera a cada frame.
- **Câmera** segue o jogador e é limitada às bordas do mapa.
- **Golden hour**: tom quente (multiply) + brilho radial nas luzes (lighter) + vinheta.
- **Partículas**: fumaça de chaminé, folhas caindo, poeira ao andar, escapamento do tanque.
- Tipos de NPC parametrizados em `buildChar(col)` via `col.kind`: `civil`, `suit`
  (terno anos 60), `dress` (vestido), `soldier` (farda oliva + capacete).

### Convenções de código (dentro de cada HTML)
- `TS=16` é o tamanho do tile. Mapa em grade (`g[y][x]`), colisão em `solid[y][x]`.
- Entidades: `add(e)` registra e marca colisão; `e.gen(e)` gera o sprite em `e.spr`.
- Interação: pontos em `points{}`/proximidade; textos em `INFO{}`; progresso em `found{}`.
- Ao adicionar arte nova, **gerar o sprite uma vez** (função `genX`) em vez de desenhar
  com `fillRect` a cada frame.

### Limitação que motivou o procedural
No Artifact do claude.ai, a página **não pode carregar imagens externas** (CSP). Por isso
tudo é gerado por código. **Aqui no repositório/local isso não vale** — se quisermos, dá
pra usar tilesets/spritesheets de imagem (ex.: Kenney.nl), ou migrar pra **Godot/Phaser**.

## Roadmap (um mapa por fase, mesmo personagem envelhecendo)

1. **Véspera do golpe** (30/03/1964) — prólogo ✅ protótipo
2. **1964 — a praça ocupada** ✅ primeira versão
3. **1968 — o AI-5**
4. **Anos de chumbo** (perseguição, stealth)
5. **Milagre e censura**
6. **Resistência e exílio** (ramifica finais)
7. **Diretas Já → 1985 — o fim**

## Cuidado com o tema

É um jogo sobre um período real e traumático (presos, mortos, desaparecidos, tortura,
censura). Manter **rigor histórico** e **respeito**: mostrar pela perspectiva de quem
sofre e resiste, nunca glamourizar o regime. Desafios de **tensão**, não de punição.
