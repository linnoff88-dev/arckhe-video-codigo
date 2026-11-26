ï»¿# ê°Å¸Å½Â¬ video-codigo ê¢â¬â vêÂ­deos feitos com cêÂ³digo

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![VêÂ­deo com cêÂ³digo](guia/assets/banner.jpg)](https://linnoff88-dev.github.io/arckhe-/guia/)

**ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

## O que êÂ©

O video-codigo êÂ© uma skill para o Claude Code (e o Codex) que faz vêÂ­deos sem cêÂ¢mera e sem editor: cada quadro êÂ©
escrito em cêÂ³digo (HTML, Canvas ou SVG) e vira MP4. Serve para quem quer mostrar um produto, um projeto ou uma ideia em
vêÂ­deo ê¢â¬â lanêÂ§amento, anêÂºncio, explicativo, sêÂ©rie de produtos ou aula. Se o vêÂ­deo for narrado, a voz êÂ© gerada no seu
computador (sem pagar por voz) e cada imagem entra no instante em que a palavra êÂ© dita. Precisa de Node, Python, FFmpeg e,
para a voz, do ARCKHÉvox com GPU NVIDIA (sem ela, o vêÂ­deo sai sêÂ³ com mêÂºsica).

## ê°Å¸ââ Guia de uso

Guia completo (landing + passo a passo + exemplos): **https://linnoff88-dev.github.io/arckhe-/guia/**

## O que ele faz

- **TrêÂªs perguntas antes de comeêÂ§ar:** sobre o que êÂ© e para quem; com voz ou sêÂ³ com mêÂºsica; qual estilo.
- **LêÂª a fonte:** documento, pasta, repositêÂ³rio ou site ê¢â¬â "lêÂª tudo e me propêÂµe um vêÂ­deo".
- **Voz local sincronizada:** `scripts/voz.py` gera a narraêÂ§êÂ£o com a voz clonada (Chatterbox) e mede o tempo de cada
  palavra (Whisper); o motor `assets/voz-sync/` prende cada imagem a uma palavra (`pal('frase', 'palavra')`).
- **Voz conferida pelo Whisper:** cada frase êÂ© ouvida de novo e refeita se nêÂ£o bate (semelhanêÂ§a ou nêÂºmero errado);
  tudo fica em `voz/<id>/checagem.md`, com o que o Whisper ouviu. `scripts/checar-fala.py` confere depois.
- **MêÂºsica original sintetizada e presa aos cortes:** `scripts/mezcla.py` (120 ou 84 BPM), bem baixa debaixo da voz;
  cada corte de cena cai num tempo forte, na troca de acorde, sem susto de volume e sem whoosh ê¢â¬â e ele confere isso.
- **Verificador de tempo de leitura:** `scripts/checar-leitura.mjs` lêÂª todo texto da tela e reprova o que some antes de dar
  tempo de ler (1 s + 0,3 s por palavra, mêÂ­nimo 1,5 s).
- **b-roll automêÂ¡tico:** vêÂ­deo de pessoa falando + transcriêÂ§êÂ£o ê¢â â plano frase a frase (rosto, traêÂ§o do conceito com a
  pessoa num cêÂ­rculo, grêÂ¡fico em tela cheia) e o molde que renderiza esse plano (`scripts/broll-plano.py`, `assets/broll/`).
- **16:9 e 9:16 do mesmo projeto:** o layout se recalcula por formato.
- **Prova antes do render inteiro** e revisêÂ£o do MP4 com folhas de quadros (`scripts/inspect-video.py`).
- **Tema ARCKHÉ pronto:** `assets/ARCKHÉ/` (cores, fontes locais, legenda com contorno).

## O processo, do inêÂ­cio ao fim

Quem faz: ê°Å¸Â§Â  sessêÂ£o com vocêÂª êÂ· ê¢Å¡â¢ê¯Â¸Â script êÂ· ê°Å¸Â¤â agente (Claude Code seguindo o `SKILL.md`). Nos comandos, `P` êÂ© a pasta do vêÂ­deo (ex.: `~/videos/trabalho/2026-11-06-meuapp`) e `scripts/` êÂ© a do kit (rode de dentro de `~/projetos/video-codigo` ou use o caminho completo).

| # | Passo | Comando / aêÂ§êÂ£o | Quem |
|---|---|---|---|
| 1 | Pedir o vêÂ­deo | "/video-codigo faz um vêÂ­deo de 40 s do meu produto, lêÂª a pasta X" | ê°Å¸Â§Â  |
| 2 | Responder as 3 perguntas | assunto e pêÂºblico êÂ· com voz ou sêÂ³ mêÂºsica êÂ· estilo (padrêÂ£o ARCKHÉ ou referêÂªncia) | ê°Å¸Â§Â  |
| 3 | Ler a fonte e propor direêÂ§êÂ£o | brief, roteiro de planos, duraêÂ§êÂ£o e formatos | ê°Å¸Â¤â |
| 4 | Aprovar a proposta (se vocêÂª quiser aprovar) | o agente mostra a proposta; ordem explêÂ­cita de produzir jêÂ¡ vale | ê°Å¸Â§Â  |
| 5 | Roteiro ê¢â â voz com tempo por palavra, conferida pelo Whisper | `python3 scripts/voz.py --projeto P P/roteiro.json [--tentativas 3]` ê¢â â `voz.wav`, `tiempos.js`, `checagem.md` | ê¢Å¡â¢ê¯Â¸Â |
| 6 | Ler a checagem da voz | abrir `P/voz/<id>/checagem.md` (frases ê¢Å¡Â  e o que o Whisper ouviu); refazer com `--refazer --frase <id>` | ê°Å¸Â¤â |
| 7 | Escrever as cenas | partir de `exemplos/explicativo-narrado/` (voz) ou `exemplos/lancamento/` (sêÂ³ mêÂºsica) e trocar as cenas | ê°Å¸Â¤â |
| 8 | Tempo de leitura + cortes | `node scripts/checar-leitura.mjs --project P --salvar-cortes P/audio/<id>/cortes.json` (e `--width 1080 --height 1920`) ê¢â â tem de sair 0 | ê¢Å¡â¢ê¯Â¸Â + ê°Å¸Â¤â |
| 9 | MêÂºsica presa aos cortes e mixagem | `python3 scripts/mezcla.py --projeto P --id <id> --musica tranquila --lufs -14` ê¢â â `mezcla.wav` + `trilha.json` (sai 1 se algo falhar) | ê¢Å¡â¢ê¯Â¸Â |
| 10 | Prova: quadros soltos e um trecho | `node scripts/render-video.mjs --project P --output prova.png --still 3` (e `--start/--end`) | ê¢Å¡â¢ê¯Â¸Â + ê°Å¸Â¤â |
| 11 | Render 16:9 e 9:16 | `node scripts/render-video.mjs --project P --output P/renders/x.mp4 --audio P/audio/<id>/mezcla.wav` (+ `--width 1080 --height 1920`) | ê¢Å¡â¢ê¯Â¸Â |
| 12 | RevisêÂ£o | `python3 scripts/inspect-video.py P/renders/x.mp4 --output-dir P/reviews` e olhar as folhas | ê¢Å¡â¢ê¯Â¸Â + ê°Å¸Â¤â |
| 13 | Entregar | MP4 + capa + projeto editêÂ¡vel; publicar sêÂ³ com o seu OK | ê°Å¸Â§Â  |

## Modo b-roll (vêÂ­deo de pessoa falando)

Para aula, depoimento ou avatar falando: a gravaêÂ§êÂ£o nêÂ£o êÂ© cortada; o kit decide, frase a frase, o que a tela mostra.

| Tratamento | Quando | Na tela |
|---|---|---|
| `rosto` | abertura, opiniêÂ£o, histêÂ³ria, chamada, fechamento | a pessoa inteira (ou meia tela com `--rosto-tamanho meia`) |
| `pip` | a frase tem um conceito | a pessoa num cêÂ­rculo no canto + o conceito desenhado em traêÂ§o (êÂ­cone + palavra) |
| `cheio` | nêÂºmero, comparaêÂ§êÂ£o, passos | grêÂ¡fico em tela cheia; o nêÂºmero cai NA palavra dita |

Regras conferidas pelo gerador: abre e fecha no rosto, rosto ê¢â°Â¤ 8 s seguidos, cheio de 3 a 8 s, nunca dois cheios
colados, traêÂ§o curto (< 1,8 s) volta ao rosto, e todo texto fica o tempo de ser lido (o visual segura sobre a frase
seguinte quando precisa).

```bash
K=~/projetos/video-codigo; P=~/videos/trabalho/2026-11-07-aula
# 1. transcriêÂ§êÂ£o com tempo por palavra (local)
cd ~/projetos/ARCKHÉvox && venv/bin/python transcrever_v1.py --in aula.mp4 --outdir $P/transcricao --src pt --words
# 2. plano + molde + quadros + êÂ¡udio  (--rosto = centro x, centro y e raio do rosto; x e y em fraêÂ§êÂ£o da largura/altura, raio em fraêÂ§êÂ£o da altura)
python3 $K/scripts/broll-plano.py --fala aula.mp4 --transcricao $P/transcricao/transcript.json --projeto $P --rosto 0.58,0.45,0.38
# 3. revisar $P/plano.md; editar $P/plano.json (tratamento, visual.titulo, visual.icone, itens) e refazer o plano.js
python3 $K/scripts/broll-plano.py --projeto $P --replanejar
# 4. quadros de prova (confira o rosto no cêÂ­rculo) e tempo de leitura
node $K/scripts/render-video.mjs --project $P --output $P/prova.png --still 6
node $K/scripts/checar-leitura.mjs --project $P
# 5. render com o som original
node $K/scripts/render-video.mjs --project $P --output $P/renders/aula-broll.mp4 --audio $P/audio/fala.wav
```

**Para o agente que vai fazer o filme:** o plano êÂ© um rascunho por heurêÂ­stica (sem IA paga). Leia `plano.md` inteiro,
troque o que nêÂ£o serve (um nêÂºmero que o Whisper ouviu errado nunca vai para a tela), escolha êÂ­cones melhores
(`assets/broll/broll.mjs ê¢â¬Âº ICONES`; êÂ­cone novo = caminhos SVG em viewBox 100êâ100) ou escreva um visual novo em
`VISUAIS` (funêÂ§êÂ£o `(caixa, v, G) ê¢â â (t) => pinta`). O vêÂ­deo vira quadros JPG em `P/quadros/` (o Chromium do render nêÂ£o
toca H.264 e precisa de quadro exato); nêÂ£o versione essa pasta. Formatos aceitos de transcriêÂ§êÂ£o: `transcript.json` e
`words.json` do ARCKHÉvox, `tempos.json` do `voz.py` ou do r7ex, `.srt` (sem tempo por palavra: pior sincronia).
Regra ARCKHÉ de avatar: no mêÂ¡ximo meia tela ê¢â¬â para avatar do Nei use `--rosto-tamanho meia`.

## InstalaêÂ§êÂ£o

```bash
git clone https://github.com/linnoff88-dev/video-codigo ~/projetos/video-codigo
bash ~/projetos/video-codigo/instalar.sh     # liga a skill no Claude Code/Codex e instala o Playwright (sem sudo)
```

PrêÂ©-requisitos: Node 18+, Python 3 com `numpy`, `scipy` e Pillow, FFmpeg. (Ubuntu/Debian: `sudo apt install python3-numpy python3-scipy python3-pil`.) O `instalar.sh` instala o Playwright e o Chromium (sem sudo); o resto ele confere e mostra o comando. **Voz (opcional):** o
[ARCKHÉvox](https://github.com/linnoff88-dev/ARCKHÉvox) em `~/projetos/ARCKHÉvox` com o ambiente Chatterbox (`~/miniconda3/envs/chatterbox`) e o Whisper do
`venv` dele; a voz de referêÂªncia êÂ© um `.wav` de 10ê¢â¬â20 s (`"voz": "/caminho/minha-voz.wav"` no roteiro). Sem voz, a
skill funciona sêÂ³ com mêÂºsica.

```bash
# voz local (testado: Python 3.11, chatterbox-tts 0.1.6, GPU NVIDIA, Miniconda; os modelos baixam alguns GB no 1êÂº uso)
# conda ou ARCKHÉvox em outro lugar: CHATTERBOX_PYTHON=/caminho/python3  ARCKHÉVOX_DIR=/caminho/ARCKHÉvox
git clone https://github.com/linnoff88-dev/ARCKHÉvox ~/projetos/ARCKHÉvox
conda create -n chatterbox python=3.11 -y
~/miniconda3/envs/chatterbox/bin/pip install chatterbox-tts==0.1.6
cd ~/projetos/ARCKHÉvox && python3 -m venv venv && venv/bin/pip install faster-whisper soundfile numpy
```

Teste rêÂ¡pido da instalaêÂ§êÂ£o (sem voz, ~1 min):
`cd ~/projetos/video-codigo/exemplos/lancamento && node ../../scripts/render-video.mjs --project . --output teste.mp4 --audio audio/lancamento/mezcla.wav --overwrite`

## Exemplos (feitos com o prêÂ³prio kit)

| Exemplo | Tipo | Arquivos |
|---|---|---|
| Filme de produto do atende-leads (telas reais, cursor, voz presa êÂ  palavra) | filme de produto narrado, 63 s; versêÂ£o ES 61 s | `exemplos/filme-produto/` êÂ· `renders/atende-16x9.mp4` êÂ· `renders/atende-9x16.mp4` êÂ· [YouTube](https://www.youtube.com/watch?v=tWv1mEIK62c) êÂ· ES: `exemplos/filme-produto-es/` |
| Este vêÂ­deo nêÂ£o tem cêÂ¢mera | explicativo narrado (voz local), 44 s | `exemplos/explicativo-narrado/` êÂ· `renders/explicativo-16x9.mp4` êÂ· `renders/explicativo-9x16.mp4` êÂ· [YouTube](https://www.youtube.com/watch?v=tkT2I6nrMAk) |
| ARCKHÉ AI em um plano | lanêÂ§amento sêÂ³ com mêÂºsica, 21 s | `exemplos/lancamento/` êÂ· `renders/lancamento-16x9.mp4` êÂ· `renders/lancamento-9x16.mp4` êÂ· [YouTube](https://www.youtube.com/watch?v=IJ-qZQ1drBc) |
| b-roll sobre um avatar falando (33 s): plano gerado + folha de quadros do render | modo b-roll (rosto êÂ· traêÂ§o êÂ· passos em tela cheia) | `exemplos/broll-plano/` (`plano.md`, `plano.json`, `folha.jpg`; o vêÂ­deo de origem nêÂ£o vai no repositêÂ³rio) |

O roteiro do exemplo narrado usa a voz `nei`, que sêÂ³ existe na mêÂ¡quina do autor: para gerar de novo a voz, rode com `VC_VOZ=/caminho/sua-voz.wav`. Para refazer sêÂ³ o vêÂ­deo (o êÂ¡udio jêÂ¡ estêÂ¡ no repositêÂ³rio): `cd exemplos/lancamento && node ../../scripts/render-video.mjs --project . --output x.mp4 --audio audio/lancamento/mezcla.wav`.

## Estrutura

```
SKILL.md              instruêÂ§êÂµes da skill (o agente segue isto)
references/           brief, direêÂ§êÂ£o, movimento, som, voz, render, revisêÂ£o, prompts de filme
scripts/              voz.py (voz local) êÂ· checar-fala.py êÂ· mezcla.py êÂ· checar-leitura.mjs êÂ· broll-plano.py êÂ· render-video.mjs êÂ· ê¢â¬Â¦
assets/broll/         molde do b-roll (pessoa falando + traêÂ§o + grêÂ¡fico)
assets/ARCKHÉ/         tema ARCKHÉ + fontes locais
assets/voz-sync/      motor de tempo (imagem presa êÂ  palavra)
exemplos/ renders/    dois vêÂ­deos completos e os MP4
guia/                 pêÂ¡gina de guia (PT/EN/ES)
```

---
[ARCKHÉ AI](https://ARCKHÉ AI)

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>