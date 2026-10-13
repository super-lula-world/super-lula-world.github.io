# Jogos Educativos — contexto do projeto

Projeto guarda-chuva: uma **coleção de jogos educativos** em pixel art. Cada jogo tem
seu próprio tema histórico/educativo. Este repositório é o site da coleção (GitHub Pages).

O primeiro jogo é o **super-lula-world**, em **pixel art** sobre a **ditadura militar
brasileira (1964–1985)**. O jogador é uma pessoa comum que **atravessa a história**: anda
pelo mapa, explora o cenário e vive os acontecimentos de cada período. Imersão histórica
+ um desafio por fase. (As seções abaixo descrevem esse jogo.)

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

> **Mapa principal / hub:** a cidade viva de `vespera-do-golpe.html` passou a ser o **hub
> do jogo** — o lugar de onde se parte. O jogador começa **dentro de casa**, sai pra rua e
> circula pela cidade. Os demais capítulos históricos abaixo serão **mapas acessados a
> partir do hub** (por pontos de saída: uma esquina, a estrada, a estação — reservar 1–2
> desses lugares no layout mesmo antes de ligá-los).

1. **Véspera do golpe** (30/03/1964) — prólogo / **hub** ✅ protótipo
2. **1964 — a praça ocupada** ✅ primeira versão
3. **1968 — o AI-5**
4. **Anos de chumbo** (perseguição, stealth)
5. **Milagre e censura**
6. **Resistência e exílio** (ramifica finais)
7. **Diretas Já → 1985 — o fim**

**Ritmo de quarteirão (identidade de cidade):** pensar a cidade em quarteirões —
rua → **calçada portuguesa** → **muro com portão** → quintal → casa. Esse ritmo repetido
é o que faz parecer cidade de verdade, não casas soltas na grama.

## Cuidado com o tema

É um jogo sobre um período real e traumático (presos, mortos, desaparecidos, tortura,
censura). Manter **rigor histórico** e **respeito**: mostrar pela perspectiva de quem
sofre e resiste, nunca glamourizar o regime. Desafios de **tensão**, não de punição.

## Direção de arte — Brasil, início dos anos 1960

O mapa tem que "cheirar" a Brasil de 1964. Referências concretas pra usar:

- **Calçada portuguesa** (pedras pretas e brancas em ondas/mosaico) — é a assinatura
  visual mais brasileira que existe. Usar nas calçadas e praças.
- **Arquitetura:** sobrados coloniais com janelas de veneziana e sacada de ferro,
  casinhas geminadas, prédios art déco baixos, igreja barroca.
- **Muros (identidade anos 60):** **muro caiado branco** (às vezes com grade de ferro em
  cima) e, principalmente, **cobogó** (bloco vazado de concreto) — o maior "cheiro de
  época" que existe. Usar em muros e muretas, com **portão de ferro** virado pra calçada.
- **Veículos de época:** Fusca, Kombi, DKW-Vemag, Aero Willys, ônibus/“circular” antigo,
  bonde (em cidade grande). O blindado/tanque e o jipe do Exército para a tensão.
- **Letreiros esmaltados e placas pintadas à mão:** BAR, BOTEQUIM, PADARIA, FARMÁCIA,
  ARMARINHO, CINE (marquise), BONDE. Tipografia da época, cores saturadas e gastas.
- **Objetos de rua:** poste de concreto com fios, **orelhão ainda NÃO (só pós-1972)** →
  usar **telefone público DE PAREDE** da época (caixa metálica com fone no gancho), fixado
  num poste ou na parede de uma casa; banca de jornal; caixa de correio; lixeira; toldos
  listrados; varal de roupa entre janelas; vasos e trepadeiras.
- **Natureza:** ipê, flamboyant, mangueira, bananeira — folhagem tropical, não pinheiro.
- **Pessoas:** homens de terno e chapéu, operários de camisa, mulheres de vestido rodado,
  senhores idosos, e **crianças** (menino de bermuda, menina de vestidinho — brincando
  perto da praça). **Variar** pele, cabelo e roupa pra ninguém ficar igual ao vizinho.
  Jornais da época (Última Hora, O Globo, Correio da Manhã).
- **Paleta:** terrosos quentes, ocre, telha, verde-oliva, azul desbotado; luz de fim de
  tarde (golden hour) já implementada.

## Pipeline de arte (PixelLab → jogo)

Agora que rodamos localmente, PODEMOS usar imagens (o Artifact não deixava). Plano:

1. Gerar sprites no **PixelLab** (MCP já conectado) e salvar em `assets/` (subpastas:
   `assets/chars/`, `assets/objs/`, `assets/tiles/`).
2. Carregar com `new Image()` + um pequeno **loader** que espera tudo carregar antes de
   iniciar o jogo; desenhar com `ctx.drawImage`, mantendo `imageSmoothingEnabled=false`.
3. Substituir os desenhos procedurais **um de cada vez** (primeiro o jogador, depois NPCs,
   depois objetos, por fim tiles), comparando antes/depois.
4. Spritesheets de personagem: 4 direções × 4 frames de caminhada, idle à parte.
   Tamanho sugerido: múltiplos de 16 (ex.: char 32×32 ou 32×48; tiles 16×16 ou 32×32).

Consistência visual no PixelLab: repetir sempre o mesmo estilo no prompt, ex.:
*"16-bit SNES pixel art, top-down 3/4 view, bold dark outline, warm 1960s palette,
limited palette, flat shading"*.

## Próximos passos (fazer em ordem, um por vez)

- **Fase A — Pipeline:** criar `assets/` + loader de imagens e trocar UM sprite (o jogador)
  por arte do PixelLab, pra validar o fluxo ponta a ponta.
- **Fase B — Definição:** substituir NPCs e objetos-chave (tanque, casas, árvores) por
  sprites do PixelLab; aumentar nº de frames.
- **Fase C — Movimento/vida:** NPCs que andam em trajetos, varal/bandeira balançando,
  pássaros, cão de rua, luzes piscando, mais partículas.
- **Fase D — Brasil 1964:** vestir o mapa com calçada portuguesa, Fusca/Kombi, letreiros
  esmaltados, postes com fios, folhagem tropical.
- **Fase E — Imersão:** transição dia→noite, (opcional) som ambiente de rádio/rua,
  diálogos com retrato do personagem.
