# ðŸŽ¬ video-codigo â€” vÃ­deos feitos com cÃ³digo

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![VÃ­deo com cÃ³digo](guia/assets/banner.jpg)](https://inematds.github.io/video-codigo/guia/)

**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

## O que Ã©

O video-codigo Ã© uma skill para o Claude Code (e o Codex) que faz vÃ­deos sem cÃ¢mera e sem editor: cada quadro Ã©
escrito em cÃ³digo (HTML, Canvas ou SVG) e vira MP4. Serve para quem quer mostrar um produto, um projeto ou uma ideia em
vÃ­deo â€” lanÃ§amento, anÃºncio, explicativo, sÃ©rie de produtos ou aula. Se o vÃ­deo for narrado, a voz Ã© gerada no seu
computador (sem pagar por voz) e cada imagem entra no instante em que a palavra Ã© dita. Precisa de Node, Python, FFmpeg e,
para a voz, do inemavox com GPU NVIDIA (sem ela, o vÃ­deo sai sÃ³ com mÃºsica).

## ðŸ“– Guia de uso

Guia completo (landing + passo a passo + exemplos): **https://inematds.github.io/video-codigo/guia/**

## O que ele faz

- **TrÃªs perguntas antes de comeÃ§ar:** sobre o que Ã© e para quem; com voz ou sÃ³ com mÃºsica; qual estilo.
- **LÃª a fonte:** documento, pasta, repositÃ³rio ou site â€” "lÃª tudo e me propÃµe um vÃ­deo".
- **Voz local sincronizada:** `scripts/voz.py` gera a narraÃ§Ã£o com a voz clonada (Chatterbox) e mede o tempo de cada
  palavra (Whisper); o motor `assets/voz-sync/` prende cada imagem a uma palavra (`pal('frase', 'palavra')`).
- **Voz conferida pelo Whisper:** cada frase Ã© ouvida de novo e refeita se nÃ£o bate (semelhanÃ§a ou nÃºmero errado);
  tudo fica em `voz/<id>/checagem.md`, com o que o Whisper ouviu. `scripts/checar-fala.py` confere depois.
- **MÃºsica original sintetizada e presa aos cortes:** `scripts/mezcla.py` (120 ou 84 BPM), bem baixa debaixo da voz;
  cada corte de cena cai num tempo forte, na troca de acorde, sem susto de volume e sem whoosh â€” e ele confere isso.
- **Verificador de tempo de leitura:** `scripts/checar-leitura.mjs` lÃª todo texto da tela e reprova o que some antes de dar
  tempo de ler (1 s + 0,3 s por palavra, mÃ­nimo 1,5 s).
- **b-roll automÃ¡tico:** vÃ­deo de pessoa falando + transcriÃ§Ã£o â†’ plano frase a frase (rosto, traÃ§o do conceito com a
  pessoa num cÃ­rculo, grÃ¡fico em tela cheia) e o molde que renderiza esse plano (`scripts/broll-plano.py`, `assets/broll/`).
- **16:9 e 9:16 do mesmo projeto:** o layout se recalcula por formato.
- **Prova antes do render inteiro** e revisÃ£o do MP4 com folhas de quadros (`scripts/inspect-video.py`).
- **Tema INEMA pronto:** `assets/inema/` (cores, fontes locais, legenda com contorno).

## O processo, do inÃ­cio ao fim

Quem faz: ðŸ§  sessÃ£o com vocÃª Â· âš™ï¸ script Â· ðŸ¤– agente (Claude Code seguindo o `SKILL.md`). Nos comandos, `P` Ã© a pasta do vÃ­deo (ex.: `~/videos/trabalho/2026-11-06-meuapp`) e `scripts/` Ã© a do kit (rode de dentro de `~/projetos/video-codigo` ou use o caminho completo).

| # | Passo | Comando / aÃ§Ã£o | Quem |
|---|---|---|---|
| 1 | Pedir o vÃ­deo | "/video-codigo faz um vÃ­deo de 40 s do meu produto, lÃª a pasta X" | ðŸ§  |
| 2 | Responder as 3 perguntas | assunto e pÃºblico Â· com voz ou sÃ³ mÃºsica Â· estilo (padrÃ£o INEMA ou referÃªncia) | ðŸ§  |
| 3 | Ler a fonte e propor direÃ§Ã£o | brief, roteiro de planos, duraÃ§Ã£o e formatos | ðŸ¤– |
| 4 | Aprovar a proposta (se vocÃª quiser aprovar) | o agente mostra a proposta; ordem explÃ­cita de produzir jÃ¡ vale | ðŸ§  |
| 5 | Roteiro â†’ voz com tempo por palavra, conferida pelo Whisper | `python3 scripts/voz.py --projeto P P/roteiro.json [--tentativas 3]` â†’ `voz.wav`, `tiempos.js`, `checagem.md` | âš™ï¸ |
| 6 | Ler a checagem da voz | abrir `P/voz/<id>/checagem.md` (frases âš  e o que o Whisper ouviu); refazer com `--refazer --frase <id>` | ðŸ¤– |
| 7 | Escrever as cenas | partir de `exemplos/explicativo-narrado/` (voz) ou `exemplos/lancamento/` (sÃ³ mÃºsica) e trocar as cenas | ðŸ¤– |
| 8 | Tempo de leitura + cortes | `node scripts/checar-leitura.mjs --project P --salvar-cortes P/audio/<id>/cortes.json` (e `--width 1080 --height 1920`) â†’ tem de sair 0 | âš™ï¸ + ðŸ¤– |
| 9 | MÃºsica presa aos cortes e mixagem | `python3 scripts/mezcla.py --projeto P --id <id> --musica tranquila --lufs -14` â†’ `mezcla.wav` + `trilha.json` (sai 1 se algo falhar) | âš™ï¸ |
| 10 | Prova: quadros soltos e um trecho | `node scripts/render-video.mjs --project P --output prova.png --still 3` (e `--start/--end`) | âš™ï¸ + ðŸ¤– |
| 11 | Render 16:9 e 9:16 | `node scripts/render-video.mjs --project P --output P/renders/x.mp4 --audio P/audio/<id>/mezcla.wav` (+ `--width 1080 --height 1920`) | âš™ï¸ |
| 12 | RevisÃ£o | `python3 scripts/inspect-video.py P/renders/x.mp4 --output-dir P/reviews` e olhar as folhas | âš™ï¸ + ðŸ¤– |
| 13 | Entregar | MP4 + capa + projeto editÃ¡vel; publicar sÃ³ com o seu OK | ðŸ§  |

## Modo b-roll (vÃ­deo de pessoa falando)

Para aula, depoimento ou avatar falando: a gravaÃ§Ã£o nÃ£o Ã© cortada; o kit decide, frase a frase, o que a tela mostra.

| Tratamento | Quando | Na tela |
|---|---|---|
| `rosto` | abertura, opiniÃ£o, histÃ³ria, chamada, fechamento | a pessoa inteira (ou meia tela com `--rosto-tamanho meia`) |
| `pip` | a frase tem um conceito | a pessoa num cÃ­rculo no canto + o conceito desenhado em traÃ§o (Ã­cone + palavra) |
| `cheio` | nÃºmero, comparaÃ§Ã£o, passos | grÃ¡fico em tela cheia; o nÃºmero cai NA palavra dita |

Regras conferidas pelo gerador: abre e fecha no rosto, rosto â‰¤ 8 s seguidos, cheio de 3 a 8 s, nunca dois cheios
colados, traÃ§o curto (< 1,8 s) volta ao rosto, e todo texto fica o tempo de ser lido (o visual segura sobre a frase
seguinte quando precisa).

```bash
K=~/projetos/video-codigo; P=~/videos/trabalho/2026-11-07-aula
# 1. transcriÃ§Ã£o com tempo por palavra (local)
cd ~/projetos/inemavox && venv/bin/python transcrever_v1.py --in aula.mp4 --outdir $P/transcricao --src pt --words
# 2. plano + molde + quadros + Ã¡udio  (--rosto = centro x, centro y e raio do rosto; x e y em fraÃ§Ã£o da largura/altura, raio em fraÃ§Ã£o da altura)
python3 $K/scripts/broll-plano.py --fala aula.mp4 --transcricao $P/transcricao/transcript.json --projeto $P --rosto 0.58,0.45,0.38
# 3. revisar $P/plano.md; editar $P/plano.json (tratamento, visual.titulo, visual.icone, itens) e refazer o plano.js
python3 $K/scripts/broll-plano.py --projeto $P --replanejar
# 4. quadros de prova (confira o rosto no cÃ­rculo) e tempo de leitura
node $K/scripts/render-video.mjs --project $P --output $P/prova.png --still 6
node $K/scripts/checar-leitura.mjs --project $P
# 5. render com o som original
node $K/scripts/render-video.mjs --project $P --output $P/renders/aula-broll.mp4 --audio $P/audio/fala.wav
```

**Para o agente que vai fazer o filme:** o plano Ã© um rascunho por heurÃ­stica (sem IA paga). Leia `plano.md` inteiro,
troque o que nÃ£o serve (um nÃºmero que o Whisper ouviu errado nunca vai para a tela), escolha Ã­cones melhores
(`assets/broll/broll.mjs â€º ICONES`; Ã­cone novo = caminhos SVG em viewBox 100Ã—100) ou escreva um visual novo em
`VISUAIS` (funÃ§Ã£o `(caixa, v, G) â†’ (t) => pinta`). O vÃ­deo vira quadros JPG em `P/quadros/` (o Chromium do render nÃ£o
toca H.264 e precisa de quadro exato); nÃ£o versione essa pasta. Formatos aceitos de transcriÃ§Ã£o: `transcript.json` e
`words.json` do inemavox, `tempos.json` do `voz.py` ou do r7ex, `.srt` (sem tempo por palavra: pior sincronia).
Regra INEMA de avatar: no mÃ¡ximo meia tela â€” para avatar do Nei use `--rosto-tamanho meia`.

## InstalaÃ§Ã£o

```bash
git clone https://github.com/inematds/video-codigo ~/projetos/video-codigo
bash ~/projetos/video-codigo/instalar.sh     # liga a skill no Claude Code/Codex e instala o Playwright (sem sudo)
```

PrÃ©-requisitos: Node 18+, Python 3 com `numpy`, `scipy` e Pillow, FFmpeg. (Ubuntu/Debian: `sudo apt install python3-numpy python3-scipy python3-pil`.) O `instalar.sh` instala o Playwright e o Chromium (sem sudo); o resto ele confere e mostra o comando. **Voz (opcional):** o
[inemavox](https://github.com/inematds/inemavox) em `~/projetos/inemavox` com o ambiente Chatterbox (`~/miniconda3/envs/chatterbox`) e o Whisper do
`venv` dele; a voz de referÃªncia Ã© um `.wav` de 10â€“20 s (`"voz": "/caminho/minha-voz.wav"` no roteiro). Sem voz, a
skill funciona sÃ³ com mÃºsica.

```bash
# voz local (testado: Python 3.11, chatterbox-tts 0.1.6, GPU NVIDIA, Miniconda; os modelos baixam alguns GB no 1Âº uso)
# conda ou inemavox em outro lugar: CHATTERBOX_PYTHON=/caminho/python3  INEMAVOX_DIR=/caminho/inemavox
git clone https://github.com/inematds/inemavox ~/projetos/inemavox
conda create -n chatterbox python=3.11 -y
~/miniconda3/envs/chatterbox/bin/pip install chatterbox-tts==0.1.6
cd ~/projetos/inemavox && python3 -m venv venv && venv/bin/pip install faster-whisper soundfile numpy
```

Teste rÃ¡pido da instalaÃ§Ã£o (sem voz, ~1 min):
`cd ~/projetos/video-codigo/exemplos/lancamento && node ../../scripts/render-video.mjs --project . --output teste.mp4 --audio audio/lancamento/mezcla.wav --overwrite`

## Exemplos (feitos com o prÃ³prio kit)

| Exemplo | Tipo | Arquivos |
|---|---|---|
| Filme de produto do atende-leads (telas reais, cursor, voz presa Ã  palavra) | filme de produto narrado, 63 s; versÃ£o ES 61 s | `exemplos/filme-produto/` Â· `renders/atende-16x9.mp4` Â· `renders/atende-9x16.mp4` Â· [YouTube](https://www.youtube.com/watch?v=tWv1mEIK62c) Â· ES: `exemplos/filme-produto-es/` |
| Este vÃ­deo nÃ£o tem cÃ¢mera | explicativo narrado (voz local), 44 s | `exemplos/explicativo-narrado/` Â· `renders/explicativo-16x9.mp4` Â· `renders/explicativo-9x16.mp4` Â· [YouTube](https://www.youtube.com/watch?v=tkT2I6nrMAk) |
| INEMA.CLUB em um plano | lanÃ§amento sÃ³ com mÃºsica, 21 s | `exemplos/lancamento/` Â· `renders/lancamento-16x9.mp4` Â· `renders/lancamento-9x16.mp4` Â· [YouTube](https://www.youtube.com/watch?v=IJ-qZQ1drBc) |
| b-roll sobre um avatar falando (33 s): plano gerado + folha de quadros do render | modo b-roll (rosto Â· traÃ§o Â· passos em tela cheia) | `exemplos/broll-plano/` (`plano.md`, `plano.json`, `folha.jpg`; o vÃ­deo de origem nÃ£o vai no repositÃ³rio) |

O roteiro do exemplo narrado usa a voz `nei`, que sÃ³ existe na mÃ¡quina do autor: para gerar de novo a voz, rode com `VC_VOZ=/caminho/sua-voz.wav`. Para refazer sÃ³ o vÃ­deo (o Ã¡udio jÃ¡ estÃ¡ no repositÃ³rio): `cd exemplos/lancamento && node ../../scripts/render-video.mjs --project . --output x.mp4 --audio audio/lancamento/mezcla.wav`.

## Estrutura

```
SKILL.md              instruÃ§Ãµes da skill (o agente segue isto)
references/           brief, direÃ§Ã£o, movimento, som, voz, render, revisÃ£o, prompts de filme
scripts/              voz.py (voz local) Â· checar-fala.py Â· mezcla.py Â· checar-leitura.mjs Â· broll-plano.py Â· render-video.mjs Â· â€¦
assets/broll/         molde do b-roll (pessoa falando + traÃ§o + grÃ¡fico)
assets/inema/         tema INEMA + fontes locais
assets/voz-sync/      motor de tempo (imagem presa Ã  palavra)
exemplos/ renders/    dois vÃ­deos completos e os MP4
guia/                 pÃ¡gina de guia (PT/EN/ES)
```

---
[INEMA.CLUB](https://inema.club)

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>