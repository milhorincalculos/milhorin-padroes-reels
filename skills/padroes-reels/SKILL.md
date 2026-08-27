---
name: reels
version: "0.1.0"
description: Assiste um Reel/vídeo curto (URL ou caminho local), gera um relatório de formato/estrutura/desempenho e atualiza a base de padrões acumulada em PADROES.md. Use para analisar vídeos de referência (próprios ou de terceiros) no contexto de conteúdo jurídico de direito do trabalho.
argument-hint: "<video-url-ou-path> [contexto opcional, ex: 'meu vídeo, foi mal' ou 'referência de concorrente']"
allowed-tools: Bash, Read, Write, Edit, AskUserQuestion
user-invocable: true
---

# /reels — leitor de vídeos + base de padrões

## Missão

Este skill existe a serviço de uma missão maior: virar o **top 1 criador de conteúdo
no mercado jurídico de direito do trabalho**. Todo vídeo analisado — próprio ou de
concorrente/referência de fora do nicho — é matéria-prima para entender o que
realmente prende atenção e converte, e alimentar `PADROES.md` com isso.

Cada vídeo analisado deixa dois rastros:
1. Um **relatório individual** em `D:\Claude\Contents\relatorios\<slug>.md`.
2. Um **incremento em `PADROES.md`** (neste mesmo diretório) — a base viva de
   padrões. `PADROES.md` nunca é reescrito do zero: só cresce e se refina.

## Passo 1 — assistir o vídeo

Use o skill `watch` (`skills/watch` em `../../../claude-video/skills/watch/SKILL.md`,
relativo à raiz do repo) para baixar/extrair frames/transcrever. Se o skill `watch`
estiver disponível via `Skill` tool nesta sessão, invoque-o normalmente. Caso não
esteja, rode manualmente:

```bash
SKILL_DIR="<repo-root>/claude-video/skills/watch"
python "$SKILL_DIR/scripts/setup.py" --check   # confirma ffmpeg/yt-dlp/python prontos
python "$SKILL_DIR/scripts/watch.py" "<url-ou-path>"
```

No Windows: usar `python`, não `python3`. Ler todos os frames retornados (tool
`Read`, em paralelo) + a transcrição antes de seguir pro passo 2.

**Nota de compatibilidade:** a cópia local do `watch` script (em
`claude-video/skills/watch/scripts/frames.py`) foi corrigida para usar `-fps_mode vfr`
em vez de `-vsync vfr` (removido em ffmpeg 9.x). Se clonar uma cópia nova do
upstream, reaplicar essa correção antes de usar no Windows.

## Passo 2 — ficha estruturada (tabela rápida)

Preencher esta tabela ANTES da análise narrativa — é a camada rápida de
comparação entre vídeos, pensada pra virar planilha depois. Duas colunas
distintas de "tipo", não confundir:

- **Formato** = formato de publicação: `Reels` | `Stories` | `Carrossel` |
  `TikTok` | `Shorts` | `Vídeo longo/IGTV` | `outro`.
- **Conteúdo** = gênero do conteúdo: `Storytelling` | `Educacional/Explicativo`
  | `Notícia/Newsjacking` | `Opinião/Polêmica` | `Humor/Meme` | `Caixinha de
  pergunta` | `Depoimento` | `Tutorial/Passo-a-passo` | `Yapcontent-lofi`
  (falando direto pra câmera, sem produção) | `outro`.

| Campo | O que preencher |
|---|---|
| **Perfil** | @handle / nome do criador ou página (via metadata do `yt-dlp`: uploader/channel) |
| **Conteúdo** | gênero, ver lista acima |
| **Link** | URL do vídeo |
| **Formato** | formato de publicação, ver lista acima |
| **Linguagem** | registro: `Falado` (tom coloquial/confessional, mesmo só em texto) \| `Formal` \| `Técnico` \| `Institucional` \| `Informal-com-gírias` |
| **Tema** | assunto central, em poucas palavras |
| **Gancho textual** | a frase/texto que abre o vídeo (primeiro texto na tela) |
| **Gancho auditivo** | som/música/efeito sonoro de abertura que puxa atenção — **sem contar fala**. Se não houver nada marcante além da fala, registrar "não identificado" (é um dado útil: mostra vídeo fraco nesse gancho específico) |
| **Gancho visual** | a primeira imagem/cena que choca, contrasta ou prende o olho |
| **Gancho falado** | a primeira frase DITA em voz — **vazio quando não há narração falada** (não é erro, é dado real) |
| **Promessa** | o que o vídeo promete entregar (dita ou implícita) |
| **Expressões** | expressões faciais/corporais marcantes do criador; quando não há talking-head, registrar o que faz esse papel (emoji, ✅, escolha de fotos) |
| **CTA** | a chamada pra ação final, literal |

Os "4 ganchos" citados no framework original (Passo 3) são exatamente estes
quatro: **textual, auditivo, visual, falado**. Nem todo vídeo usa os 4 —
registrar quais aparecem é, em si, um dado (ver `PADROES.md`).

## Passo 3 — mapear a estrutura de retenção

**Ordem obrigatória: estrutura nativa primeiro, framework depois.** O
"Framework de referência #1" abaixo (Ganchismo→Promessa→...→CTA) foi dado
pelo usuário só como **um exemplo** de como algumas pessoas montam a
estratégia de um Reels — não é gabarito pra encaixar todo vídeo à força.
Antes de qualquer coisa, descrever a **estrutura nativa** do vídeo: a
sequência real de blocos, na ordem em que ela realmente acontece, com as
próprias palavras do vídeo — mesmo que essa sequência não bata com nenhum
framework catalogado ainda. Só depois de documentar a estrutura nativa,
comparar (opcionalmente) contra os frameworks já em `PADROES.md` (Framework
#1 e o modelo de estrutura do `milhorin-fura-bolha`, ver seção "Estruturas
nativas identificadas") e registrar onde bate, bate parcialmente, ou diverge.
**Nunca pular a etapa nativa e ir direto pro encaixe no framework** — é
assim que se perde uma estrutura genuinamente nova (ver o caso Bruno
Belíssimo: parecia fura-bolha à primeira vista, mas a estrutura real —
comandos diretos ao espectador, CTA em posição variável, fechamento com
assinatura pessoal — é a dele própria, não a do framework).

O objetivo de longo prazo é acumular várias estruturas nativas em
`PADROES.md` e descobrir quais se repetem em quais tipos de vídeo/criador —
não forçar tudo em uma estrutura só.

**Framework de referência #1** (um dos possíveis, não o único):

```
Ganchismo → Promessa aberta → Introdução (até ~10s) → Bloco 1 → Ponta solta 1
→ Bloco 2 → Ponta solta 2 → Solução → CTA
```

- **Ganchismo**: não é 1 gancho, são ~4 empilhados nos primeiros segundos, cada um
  quebrando o padrão de scroll de um jeito diferente (ex: afirmação chocante +
  visual inesperado + pergunta direta + promessa de resultado). Listar quais dos
  ~4 aparecem e com que frase/imagem.
- **Promessa aberta**: o que o vídeo promete entregar, dita ou implícita.
- **Introdução**: o que segura a pessoa até o segundo 10.
- **Bloco 1 / Bloco 2**: os blocos de conteúdo útil de fato.
- **Ponta solta 1 / Ponta solta 2**: frases que geram curiosidade e adiam a
  resposta ("mas essa nem é a pior parte...", "só que existe um erro ainda
  maior...", reticências). Marcar o timestamp de cada uma.
- **Solução**: onde a promessa aberta é finalmente resolvida.
- **CTA**: qual é, e se faz sentido com o estilo do vídeo — polêmico/analítico
  pede CTA de compartilhamento; passo-a-passo/tutorial pede CTA de salvamento;
  outros formatos podem pedir seguir/comentar. Se o CTA não bater com o estilo,
  registrar isso como aprendizado (erro a não repetir).

Se o vídeo bater bem com este framework, dizer isso e preencher normalmente.
Se não bater (faltar parte, ordem diferente, ou a lógica interna for outra),
**não forçar o encaixe** — descrever a estrutura real como ela é, e avaliar
se merece virar uma entrada nova em "Estruturas nativas identificadas"
(`PADROES.md`), sozinha ou reforçando uma já catalogada do mesmo criador/
formato.

## Passo 4 — por que performou bem (ou mal)

Além da estrutura, avaliar: ritmo de edição, se o áudio/legenda carrega o vídeo
sozinho (teste do mudo), se a promessa do gancho realmente é entregue, se cabe
replicar no nicho jurídico sem soar forçado.

## Passo 5 — principais aprendizados

3-6 bullets, sempre **acionáveis** — coisas que dá pra replicar ou evitar no
próximo roteiro. Evitar generalidade ("boa edição"); ser específico
("cortar a cada 2-3s no bloco de contexto, antes de travar no bloco de
solução").

## Passo 5b — qual skill de produção reproduz esse formato

Este passo existe porque analisar padrão sem ligar pra produção é trabalho
perdido — a missão é criar conteúdo, não só cataloga-lo. Perguntar sempre:
**"se eu fosse pedir pra reproduzir esse estilo agora, qual skill eu chamaria?"**

- Comparar o formato do vídeo analisado contra as skills de produção já
  existentes (`milhorin-hooks`, `milhorin-lofi-viral`, `milhorin-fura-bolha`,
  `milhorin-yap-content` — conferir a seção "Skills de produção — cobertura
  por formato" em `PADROES.md` pra não reinventar essa comparação toda vez).
- Se uma delas cobre bem, registrar qual e **o que especificamente vale a
  pena otimizar nela** com esse aprendizado (ex: "reforça o Grupo I de
  `milhorin-hooks`" ou "vira novo bloco em `milhorin-lofi-viral`") — não
  basta apontar a skill, tem que apontar o ajuste concreto.
- Se nenhuma cobre bem — a gramática de produção é realmente diferente
  (ex: edição literal por palavra do Bruno Belíssimo, que não é nem
  monólogo cru, nem blocos lo-fi, nem escalada de fura-bolha) — sinalizar
  isso como **candidata a skill nova**, com o motivo específico (o que a
  torna distinta das 4 existentes) e quantos vídeos já confirmam esse
  formato. Não criar a skill sozinho aqui — só sinalizar; a criação em si
  segue o fluxo já usado (scratch → `skill-creator` → pacote `.skill` →
  `SendUserFile`), só depois de confirmação do usuário.
- Atualizar a seção "Skills de produção — cobertura por formato" em
  `PADROES.md` com o resultado dessa análise (reforçando contador se já
  existia, ou adicionando linha nova).

## Passo 6 — salvar o relatório

Criar `D:\Claude\Contents\relatorios\<slug>.md` (slug = data + fonte curta, ex:
`2026-08-27-instagram-microsoft-quantico.md`). Usar o template:

```markdown
# <título curto do vídeo>

- **Data da análise:** <YYYY-MM-DD>
- **Duração:** <MM:SS>
- **Origem:** <"Top 5 views, run #N, post #M" (veio do dashboard) ou "avulso">

## Ficha

| Perfil | Conteúdo | Link | Formato | Linguagem | Tema |
|---|---|---|---|---|---|
| <@handle> | <gênero> | <url> | <Reels/etc> | <Falado/Formal/...> | <tema> |

| Gancho textual | Gancho auditivo | Gancho visual | Gancho falado | Promessa | Expressões | CTA |
|---|---|---|---|---|---|---|
| <texto> | <som, ou "não identificado"> | <cena> | <frase, ou vazio> | <promessa> | <expressões> | <CTA literal> |

## Por que performou bem

<2-4 parágrafos ou bullets>

## Estrutura

- **Ganchismo:** <descrição dos ~4 ganchos + timestamps>
- **Promessa aberta:** <...>
- **Introdução (0-10s):** <...>
- **Bloco 1:** <...>
- **Ponta solta 1:** <timestamp + frase>
- **Bloco 2:** <...>
- **Ponta solta 2:** <timestamp + frase>
- **Solução:** <...>
- **CTA:** <qual, e se combina com o estilo>

## Principais aprendizados

- <bullet acionável>
- <bullet acionável>

## Skill de produção sugerida

<Qual das skills existentes (`milhorin-hooks` / `milhorin-lofi-viral` /
`milhorin-fura-bolha` / `milhorin-yap-content`) reproduz esse estilo, e o
ajuste concreto que esse vídeo sugere nela — OU "nenhuma cobre bem" +
justificativa, sinalizando candidata a skill nova (ver Passo 5b).>

## Transcrição completa

<Colar aqui a transcrição inteira retornada pelo `watch` (com timestamps,
se o `--detail` usado tiver retornado). Se o vídeo não teve transcrição
(sem fala, ou créditos de Whisper esgotados no momento da análise),
registrar isso explicitamente em vez de deixar em branco — nunca omitir
a seção. Guardar a transcrição aqui evita ter que reprocessar o vídeo se
for preciso reconsultar o que foi dito.>
```

Depois, adicionar uma linha em `relatorios/_INDICE.md` (criar se não existir):
`| data | título | formato | link do relatório |`.

## Passo 7 — atualizar a base de padrões

Abrir `PADROES.md` (neste diretório). Para cada aprendizado novo do vídeo:
- Se já existe um padrão parecido, **reforçar** (ex: incrementar contador de
  quantos vídeos confirmaram aquele padrão, ou adicionar o vídeo como novo
  exemplo).
- Se é um padrão novo, adicionar em seção própria.
- Nunca apagar padrão existente por causa de 1 vídeo contrário — anotar como
  exceção/contraexemplo em vez de remover.
- Se o vídeo veio da fila do dashboard (ver Passo 0), checar
  `PADROES_VALIDADOS.md` (`C:\Users\WINDOWS\Documents\Claude Code\milhorin-dashboard\PADROES_VALIDADOS.md`)
  pra ver se algum padrão de lá bate com o que foi encontrado aqui — se bater,
  citar o dado quantitativo real (views/likes/post_ids) como reforço extra do
  padrão nesta base.

## Passo 0 — de onde veio o vídeo (preencher antes do Passo 1)

Se o link veio da seção "📹 Próximos pra análise frame a frame" do
`PADROES_VALIDADOS.md` (dashboard), anotar no relatório (ver template, campo
**Origem**) o número da run e do post (ex: "Top 5 views, run #32, post #300").
Se foi um vídeo avulso (usuário mandou o link direto, fora do fluxo do
dashboard), registrar "avulso".

O objetivo declarado é essa base ficar "surreal e cada vez mais completa" — ou
seja, cada vídeo é uma oportunidade de refinar, nunca só arquivar.

## Quando usar

- Vídeo de concorrente/referência que performou bem → entender por quê.
- Vídeo próprio (bem ou mal-sucedido) → aprender o que replicar/evitar.
- Usuário manda um link ou path e pede pra "analisar", "ver esse reel",
  "por que viralizou", ou explicitamente `/reels`.
