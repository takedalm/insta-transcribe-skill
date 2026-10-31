# Guia Seguro para Transcrever Instagram Stories (`cookies.txt`)

## Por que Stories exigem `cookies.txt` (e Reels públicos não)?

- **Instagram Reels Públicos (`instagram.com/reel/...`):** Podem ser acessados anonimamente pelo `yt-dlp`. Esta skill **nunca envia cookies** na primeira tentativa em Reels públicos, garantindo risco zero para sua conta.
- **Instagram Stories (`instagram.com/stories/usuario/123...`):** Os servidores da Meta bloqueiam 100% do acesso anônimo a Stories e exigem o cookie `sessionid` de uma conta autenticada.

---

## Como exportar o `cookies.txt` com Segurança Máxima (Sem risco de ban na conta principal)

> [!CAUTION]
> **Regra de Ouro:** Nunca use sua conta principal ou comercial do Instagram para automações de terceiros, e **não exporte os cookies de uma aba normal do navegador** (pois quando você fecha a aba ou navega normalmente, o Instagram rotaciona o token e invalida os cookies exportados).

### Passo a Passo Recomendado:

1. **Crie ou use uma Conta Secundária ("Burner Account"):**
   - Use uma conta secundária do Instagram que siga os perfis públicos/privados cujos Stories você deseja transcrever.
2. **Instale a extensão "Get cookies.txt LOCALLY":**
   - Em navegadores baseados em Chromium (Chrome/Opera/Brave/Edge): busque por **Get cookies.txt LOCALLY** (código aberto, não envia dados para a internet) ou equivalente no Firefox/LibreWolf (**cookies.txt**).
   - Nas configurações da extensão, marque **"Allow in Incognito / Permitir em janela anônima"**.
3. **Abra uma Janela Anônima (Privada):**
   - Acesse `https://www.instagram.com` na janela anônima e faça login na **conta secundária**.
4. **Exporte e Feche a Janela Anônima (Sem clicar em "Sair/Logout"):**
   - Clique na extensão e exporte os cookies no formato **Netscape** com o nome `cookies.txt`.
   - Salve o arquivo dentro da pasta raiz da skill (`<skill_dir>/cookies.txt`) ou informe o caminho via `--cookies`.
   - **Feche a janela anônima diretamente no "X"** (não clique em "Sair" dentro do Instagram, caso contrário o servidor da Meta invalidará a sessão exportada).

---

## Como Configurar a API Opcional da Groq (`--engine groq`)

Se desejar transcrever vídeos longos na nuvem em poucos segundos usando o modelo `whisper-large-v3-turbo` sem consumir VRAM/CPU local:
1. Crie uma chave gratuita em `https://console.groq.com/keys`.
2. Salve um arquivo `.env` na pasta raiz da skill (`<skill_dir>/.env`) com o conteúdo:
   ```env
   GROQ_API_KEY=gsk_sua_chave_aqui
   ```
   Ou defina a variável de ambiente `GROQ_API_KEY` no sistema operacional.
3. Execute com `--engine groq` (ou `--engine auto`).

---

## Proteções Anti-Ban Ativas no Script (`transcribe_media.py`)

Esta skill implementa 4 camadas automáticas de proteção quando o `cookies.txt` está presente:
1. **Isolamento de Cookies:** Os cookies **só são lidos** se a URL for explicitamente de `/stories/` ou se o download anônimo falhar com erro de login.
2. **Cooldown Local Obrigatório (`.ig_ratelimit.lock`):** O script impede requisições autenticadas seguidas ao Instagram em intervalos menores que **10 segundos**, aguardando automaticamente caso necessário.
3. **Pausa Interna entre Sub-requisições (`--sleep-requests 2`):** O `yt-dlp` aguarda 2 segundos entre a chamada de metadados GraphQL e o download do arquivo de áudio.
4. **Extração Apenas da Faixa de Áudio (`bestaudio/best`):** Minimiza o tráfego de rede nos servidores da CDN da Meta.
