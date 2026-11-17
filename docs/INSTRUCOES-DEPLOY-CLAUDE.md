# 🤖 Instruções pro Claude — Publicar LP Valore na HostGator

> **Como usar este arquivo:** abra uma nova sessão do Claude (Claude Code, Claude.ai ou extensão), **cole o conteúdo deste MD inteiro como primeira mensagem** e anexe junto os arquivos listados na seção "Arquivos do projeto". O Claude vai conduzir o deploy do início ao fim.

---

## 🎯 Contexto

Sou o **Matheus**, dono do projeto **Valore** — importação de produtos Apple dos EUA. Acabei de finalizar a V3 da landing page e preciso publicar no ar na minha hospedagem **HostGator**, apontada pro domínio `sejavalore.com.br` (ou outro que eu te informar).

**Repositório GitHub (opcional):** https://github.com/umatheusribeiro/smart-apple

---

## 🎬 Objetivo desta sessão

Fazer deploy da LP na HostGator, deixar o site acessível no domínio final e confirmar que o Google Tag Manager está rastreando. Depois configurar deploy automático pra eu não precisar repetir isso.

**Divide em 4 fases:**
1. **Fase A** — Upload inicial dos arquivos via FTP/cPanel
2. **Fase B** — Validação: site acessível, GTM disparando, console sem erro
3. **Fase C** — Configurar deploy automático via GitHub Actions
4. **Fase D** — Entrega: me mandar checklist + links

---

## 📎 Arquivos do projeto (anexos nesta mensagem)

Vou anexar:

1. **`valore_landing_page_v3.html`** — arquivo HTML principal da LP (**precisa virar `index.html` no servidor**)
2. **Pasta `fonts/`** — 4 fontes Neue Montreal (.woff2):
   - NeueMontreal-Light.woff2
   - NeueMontreal-Regular.woff2
   - NeueMontreal-Medium.woff2
   - NeueMontreal-Bold.woff2
3. **Pasta `media/`** — assets visuais:
   - `logo-valore.svg` (logo oficial)
   - `unboxing/` (4 fotos: unboxing-1.jpg até unboxing-4.jpg)
   - `depoimentos/` (3 vídeos: video-depoimento-001.mp4, video-depoimento-002.mp4, gustavo-video.mp4)
   - `produtos/` (fotos dos produtos)
   - `mascot-gym.png`, `mascot-mac.png`, `mascot-ipad.png`, `mascots-full.png`
   - `gustavo-hero.jpg`, `macbook-mockup.png`, `macbook-overlay.png`, `video-animado-macbook.mp4`

**Importante:** todos os caminhos no HTML são **relativos** (`./media/...`, `./fonts/...`), então a estrutura de pastas precisa ser preservada tal como está.

---

## ✅ Google Tag Manager — já instalado e validado

**ID configurado:** `GTM-WRDXQKMP`

Confere antes de subir:
- Script síncrono no `<head>` (linhas 4-10 do HTML) ✅
- Noscript iframe fallback logo após `<body>` (linhas 1124-1127) ✅
- Posição e padrão exatamente como o Google recomenda ✅

**Você não precisa mexer no GTM.** Só confirma que subiu certo (Fase B).

---

## 📋 PASSO A PASSO — Execute em ordem

### Fase A — Upload inicial

**A1.** Me pergunta quais credenciais HostGator eu tenho:
- Host FTP (geralmente `ftp.sejavalore.com.br` ou IP)
- Usuário FTP
- Senha FTP
- **OU** acesso ao cPanel (user + senha do painel)

**NÃO execute nada antes de eu confirmar que posso compartilhar.** Me avisa pra eu colar como **credencial segura** (secret, variável de ambiente ou passo manual) — não quero senha em texto puro no chat.

**A2.** Com as credenciais, decide o melhor caminho comigo:
- **Opção 1 (recomendada):** eu subo os arquivos manualmente via cPanel File Manager seguindo seu passo a passo escrito
- **Opção 2:** você gera script de upload FTP em `lftp` ou `curl` pra eu rodar no meu Mac
- **Opção 3:** eu uso FileZilla com interface gráfica

**A3.** Instrução chave para o upload, independente do método:
- Destino: `/public_html/` (raiz do domínio)
- **Renomear** `valore_landing_page_v3.html` → `index.html`
- Preservar estrutura: `/public_html/fonts/`, `/public_html/media/unboxing/`, `/public_html/media/depoimentos/`, etc.
- Garantir que `.woff2`, `.mp4`, `.svg`, `.jpg`, `.png` subam em modo **binário** (não ASCII — se usar FTP cliente antigo)

**A4.** Se já existir conteúdo em `/public_html/` (ex: página padrão da HostGator), me pergunta se pode sobrescrever antes de qualquer ação destrutiva.

---

### Fase B — Validação pós-upload

**B1.** Me manda um checklist com cada URL pra eu abrir e confirmar:
- [ ] `https://sejavalore.com.br` carrega a LP (não a página default HostGator)
- [ ] Fontes Neue Montreal carregam (texto não aparece com serifa fallback)
- [ ] Logo aparece no header
- [ ] Fotos unboxing carregam no carrossel
- [ ] Vídeos de depoimento tocam
- [ ] Botão "Fazer Encomenda" vai pro WhatsApp
- [ ] Botão quiz vai pra `https://valorequizz.lovable.app/`
- [ ] Links do menu (`#sobre`, `#como-encomendar`, etc.) rolam suave
- [ ] Theme toggle (dark/light) funciona
- [ ] Mobile responsivo (375px, 768px)

**B2.** Me guia pra validar o GTM disparando:
- Instalar extensão **Tag Assistant Companion** do Chrome (link direto)
- Entrar no modo preview do GTM pelo tagmanager.google.com
- Conectar na URL `sejavalore.com.br`
- Confirmar que o container `GTM-WRDXQKMP` dispara no `Page view`

**B3.** Checar console do navegador (F12):
- Zero erros em vermelho
- Avisos amarelos são OK se forem de third-party

**B4.** Se algum item falhar, **investiga o motivo** antes de prosseguir. Erros comuns:
- 404 em fonte → caminho da pasta `fonts/` ficou errado no upload
- 404 em imagem → pasta `media/unboxing/` com nome case-sensitive diferente
- GTM não dispara → DNS ainda propagando ou bloqueado por adblocker

---

### Fase C — Deploy automático via GitHub Actions

Depois do site funcionando na HostGator, configura o workflow pra eu nunca mais fazer upload manual.

**C1.** Me guia a cadastrar 3 secrets em https://github.com/umatheusribeiro/smart-apple/settings/secrets/actions:

| Nome | Valor |
|------|-------|
| `FTP_SERVER` | Host FTP da HostGator |
| `FTP_USERNAME` | Usuário FTP |
| `FTP_PASSWORD` | Senha FTP |

**C2.** Cria o arquivo `.github/workflows/deploy.yml` no repositório com:
- Trigger: `push` em `main`
- Action: `SamKirkland/FTP-Deploy-Action@v4.3.5`
- `local-dir`: `./landing/v3/`
- `server-dir`: `/public_html/`
- Exclui `.git`, `node_modules`, `.DS_Store`
- **Rename do `valore_landing_page_v3.html` pra `index.html`** antes do upload (via step `run: mv`)

**C3.** Commita o workflow, faz push, e acompanha o primeiro run em https://github.com/umatheusribeiro/smart-apple/actions.

**C4.** Confirma que o run ficou verde ✅ e que o site continua funcionando.

---

### Fase D — Entrega final

Me manda um **resumo final em markdown** contendo:

1. **URLs funcionando:**
   - `https://sejavalore.com.br` (status: ✅)
   - `https://www.sejavalore.com.br` (status: ✅ ou ❌ com motivo)

2. **GTM validado** em `GTM-WRDXQKMP` com screenshot do Tag Assistant confirmando o disparo

3. **Deploy automático configurado** — link do workflow + print do primeiro run verde

4. **Próximos passos recomendados** pra mim:
   - Criar tags de conversão no GTM (botão WhatsApp, botão quiz, scroll depth)
   - Conectar GA4
   - Instalar Meta Pixel via GTM
   - Configurar email de contato `contato@sejavalore.com.br`

5. **Fluxo pra editar daqui pra frente:**
   > "Pra editar qualquer coisa: abre VS Code com Claude, pede a mudança em português, pede pra commitar e pushar. Em ~1min o site atualiza sozinho."

---

## ⚠️ Regras importantes (não ignora)

1. **Nunca peça senha em texto puro no chat.** Sempre me oriente a usar secret, env var ou configuração manual isolada.
2. **Nunca sobrescreva arquivos existentes em `/public_html/` sem me perguntar.** Pode ter coisa minha lá.
3. **Nunca deleta nada do GitHub** sem me confirmar primeiro.
4. **Preserva os caminhos relativos** do HTML — não mexe em `./media/` nem `./fonts/`.
5. **Confirma a cada fase** antes de ir pra próxima. Se algo der ruim, para e me pergunta.

---

## 🆘 Se eu travar

Se a HostGator não deixa upload de algum arquivo, ou o DNS não propagou, ou o GTM não dispara, você me ajuda a diagnosticar com comandos práticos (`curl -I`, `dig`, `nslookup`, DevTools Network tab) e me manda as instruções passo a passo.

Pode começar agora pela **Fase A, passo A1**.
