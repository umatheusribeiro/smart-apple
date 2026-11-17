# Como editar a LP Valore e subir no ar

Guia passo a passo pra você fazer **sozinho**, do zero, usando a extensão do Claude. Sem precisar de ninguém técnico ao lado.

**Tempo total:** ~40min na primeira vez. Depois disso, cada edição leva ~2min.

---

## 🎯 O que você vai conseguir ao final

- Editar a landing page conversando com o Claude (sem saber código)
- Subir mudanças no ar automaticamente ao salvar
- Dominar o fluxo: **edita → commita → sobe sozinho**

---

## ✅ Pré-requisitos (abra tudo antes de começar)

1. **Conta GitHub** — já tem: `umatheusribeiro`
2. **VS Code instalado** — https://code.visualstudio.com/download
3. **Extensão Claude pro VS Code** — https://marketplace.visualstudio.com/items?itemName=anthropic.claude-code
4. **Plano HostGator contratado** com acesso ao cPanel
5. **Domínio** (`sejavalore.com.br` ou outro) já registrado

Se faltar algum, resolve ele primeiro e volta aqui.

---

## PASSO 1 — Clonar o projeto no seu computador (1x só)

Abra o **Terminal** do Mac (⌘ + Espaço, digita "terminal").

Cole e aperta Enter:

```bash
cd ~/Projetos
git clone https://github.com/umatheusribeiro/smart-apple.git
cd smart-apple
code .
```

Pronto. O VS Code abre com o projeto todo.

> Se der erro "git: command not found", instala o Xcode Command Line Tools: `xcode-select --install`

---

## PASSO 2 — Ativar o Claude no VS Code

1. No VS Code, clica no ícone do **Claude** na barra lateral esquerda (ícone de asterisco ✱)
2. Faz login com sua conta Anthropic
3. Ele vai pedir permissão pra ler/editar o projeto — **autoriza**

Agora você conversa com o Claude dentro do VS Code e ele edita os arquivos pra você.

---

## PASSO 3 — Como pedir edições pro Claude

Abre o chat do Claude e **fala em português natural** o que quer mudar. Exemplos reais que funcionam:

| O que você quer | Como pedir |
|-----------------|------------|
| Trocar o telefone do WhatsApp | *"troca o número do WhatsApp pra 21 99999-8888 em todos os lugares da LP V3"* |
| Mudar uma foto do unboxing | *"substitui a foto unboxing-2.jpg por essa que arrastei pra pasta"* |
| Alterar texto do FAQ | *"muda a resposta da pergunta sobre garantia pra: 'Garantia Apple internacional de 1 ano...'"* |
| Adicionar depoimento novo | *"adiciona mais um card de depoimento com esse texto: ...'"* |
| Trocar cor de botão | *"deixa o botão 'Fazer Encomenda' mais verde, tipo neon"* |

**Dica de ouro:** sempre peça pra ele **"abrir no navegador pra eu ver"** depois da mudança. Ele faz.

**Arquivo principal da LP:** `landing/v3/valore_landing_page_v3.html`

---

## PASSO 4 — Salvar no GitHub (commit + push)

Depois de aprovar a edição no navegador, no chat do Claude pede:

> *"commita essas mudanças e faz push"*

Ele vai:
1. Salvar as mudanças com uma mensagem descritiva
2. Mandar pro GitHub

Pronto. Em ~1 minuto o site já está no ar (ver Passo 7 — deploy automático).

---

## PASSO 5 — Primeira subida na HostGator (manual, 1x só)

Antes do deploy automático funcionar, precisa subir a primeira versão manualmente.

### 5.1 — Pegar as credenciais FTP

1. Entra no **cPanel** da HostGator (link vem no email de boas-vindas)
2. Procura por **"Contas de FTP"** ou **"FTP Accounts"**
3. Anota em um lugar seguro:
   - **Host FTP:** geralmente `ftp.seudominio.com.br` ou um IP
   - **Usuário:** do tipo `usuario@seudominio.com.br`
   - **Senha:** a que você definiu

### 5.2 — Subir a pasta `landing/v3/` inteira

Opção A — **Pelo cPanel (mais fácil):**
1. No cPanel, abre **"Gerenciador de Arquivos"**
2. Entra em `public_html/`
3. Clica em **"Upload"** e manda TODOS os arquivos de dentro de `landing/v3/` (inclusive subpastas `fonts/`, `media/`)
4. O arquivo `valore_landing_page_v3.html` precisa virar `index.html` lá no servidor — renomeia depois de subir, OU já faz essa renomeação localmente antes de subir

Opção B — **FTP via FileZilla:**
1. Baixa o FileZilla: https://filezilla-project.org/
2. Conecta com as credenciais FTP
3. Arrasta a pasta `landing/v3/*` pro `public_html/` do servidor

### 5.3 — Testar

Abre `https://seudominio.com.br` no navegador. Se aparecer a LP, deu certo.

---

## PASSO 6 — Apontar o domínio (se ainda não apontou)

Se seu domínio foi comprado FORA da HostGator (ex: Registro.br, GoDaddy):

1. Entra no painel do registrador
2. Procura **"Nameservers"** ou **"Servidores DNS"**
3. Troca pelos nameservers da HostGator (vem no email de boas-vindas, tipo `ns1.hostgator.com.br`)
4. Espera **até 48h** pra propagar (geralmente leva 1-2h)

Pode conferir em https://dnschecker.org se o DNS já propagou.

---

## PASSO 7 — Deploy automático (configurar 1x)

Agora a parte mágica: toda vez que você fizer push no GitHub, o site atualiza sozinho.

### 7.1 — Cadastrar credenciais FTP no GitHub

1. Abre https://github.com/umatheusribeiro/smart-apple/settings/secrets/actions
2. Clica em **"New repository secret"** e cria 3 secrets:

| Nome | Valor |
|------|-------|
| `FTP_SERVER` | o host FTP (ex: `ftp.sejavalore.com.br`) |
| `FTP_USERNAME` | seu usuário FTP |
| `FTP_PASSWORD` | sua senha FTP |

### 7.2 — Pedir pro Claude criar o workflow

No chat do Claude, cola:

> *"cria o arquivo `.github/workflows/deploy.yml` que faz FTP upload da pasta `landing/v3/` pro `/public_html/` da HostGator sempre que eu fizer push na main. Usa a action `SamKirkland/FTP-Deploy-Action@v4.3.5` e os secrets `FTP_SERVER`, `FTP_USERNAME`, `FTP_PASSWORD`. Depois commita e faz push."*

Ele vai criar, commitar e subir. **A partir desse momento**, toda edição que você fizer e pushar atualiza o site em ~1min.

---

## 🔁 Rotina do dia a dia (depois de tudo configurado)

Quando quiser editar qualquer coisa:

1. Abre o VS Code (projeto já clonado)
2. No terminal: `git pull` (pega atualizações se alguém mexeu antes)
3. Abre o chat Claude e fala o que quer mudar
4. Confere no navegador (ele abre pra você)
5. Fala: *"commita e sobe"*
6. **Em ~1 minuto o site já atualizou sozinho**

É isso. Sem código, sem FTP toda vez, sem drama.

---

## 🆘 Deu problema? Pergunta pro Claude

Essas frases funcionam pra pedir ajuda:

- *"o site não atualizou depois do push, me ajuda a descobrir o que aconteceu"*
- *"abre o GitHub Actions e me mostra o erro do último deploy"*
- *"o DNS já propagou? confere pra mim"*
- *"desfaz meu último commit, errei a mudança"*

Ele te guia. Não precisa saber o que fazer.

---

## 📁 Mapa do projeto (pra você se achar)

```
smart-apple/
├── landing/v3/
│   ├── valore_landing_page_v3.html   ← ARQUIVO PRINCIPAL (a LP inteira)
│   ├── fonts/                         ← Fontes Neue Montreal
│   └── media/
│       ├── logo-valore.svg            ← Logo
│       ├── unboxing/                  ← Fotos clientes
│       ├── depoimentos/               ← Vídeos de depoimento
│       └── produtos/                  ← Fotos de produtos
├── docs/
│   ├── COMO-EDITAR-E-SUBIR.md         ← Este guia
│   └── stories/                        ← Histórico de mudanças grandes
└── README.md
```

---

**Pronto. Qualquer dúvida, pergunta pro Claude. Ele leu esse guia e te ajuda.**
