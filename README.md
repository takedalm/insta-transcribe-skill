# insta-transcribe-skill

Skill segura para agentes de IA (Antigravity, Claude Code, Cursor, GitHub Copilot, Windsurf, Cline) para baixar e transcrever áudios e vídeos de **Instagram Reels, Instagram Stories, YouTube, TikTok, X/Twitter** e arquivos locais de mídia.

## Destaques

- **Foco em Segurança (Hardened):**
  - Validação estrita de URLs (`urllib.parse`) e bloqueio de *Option/Command Injection*.
  - Delimitadores explícitos (`<<<TRANSCRIPT_START>>>` / `<<<TRANSCRIPT_END>>>`) contra *Indirect Prompt Injection*.
  - Execução da biblioteca `yt_dlp` diretamente via API Python (sem chamadas de shell interpoladas).
- **Instagram Reels & Stories com Proteção Anti-Ban:**
  - **Reels públicos:** Download 100% anônimo (sem cookies) por padrão.
  - **Stories / Conteúdo restrito:** Suporte a `cookies.txt` isolado com *cooldown* local obrigatório e controle de taxa de requisições (`sleep-requests`).
- **Leve para GPUs de Entrada (2GB+ VRAM) e CPUs:**
  - Quantização `int8` nativa + filtro de voz **Silero VAD** via `faster-whisper`.
  - Fallback automático de `CUDA` para `CPU` e bloqueio preventivo de modelos que excedam 2GB de VRAM.
  - Suporte opcional à nuvem via API da Groq (`--engine groq` com `whisper-large-v3-turbo`).

## Instalação Rápida (`skills.sh`)

```bash
npx skills add <owner>/insta-transcribe-skill
```

## Pré-requisitos

- **Python 3.9+**
- **ffmpeg** instalado no `PATH` do sistema
- Pacotes Python:
  ```bash
  pip install faster-whisper yt-dlp
  ```

## Uso Básico

Verificar o ambiente e hardware:
```bash
python skills/insta-transcribe-skill/scripts/transcribe_media.py --check
```

Transcrever uma URL ou arquivo local:
```bash
python skills/insta-transcribe-skill/scripts/transcribe_media.py --input "https://www.instagram.com/reel/..." --model small --timestamps
```

Para configurar suporte a **Instagram Stories** ou à **API da Groq**, consulte [references/instagram_stories_guide.md](./skills/insta-transcribe-skill/references/instagram_stories_guide.md).

## Licença

MIT
