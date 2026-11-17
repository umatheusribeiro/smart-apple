# 🚀 Deploy no Vercel (preview pra aprovação)

Enquanto a HostGator não sobe, use o Vercel pra gerar um link público com SSL em ~2 minutos. Ideal pra mandar pro Gustavo aprovar antes de subir no domínio final.

**O que você vai ter ao final:** uma URL tipo `https://valore-xxxxx.vercel.app` que atualiza sozinha toda vez que você fizer push na main.

---

## PASSO 1 — Criar conta no Vercel (1x só)

1. Abre https://vercel.com/signup
2. Clica em **"Continue with GitHub"**
3. Autoriza o Vercel a acessar sua conta GitHub

Pronto, conta criada.

---

## PASSO 2 — Importar o projeto

1. No dashboard do Vercel, clica em **"Add New..." → "Project"**
2. Procura por `smart-apple` na lista de repositórios
3. Clica em **"Import"**

---

## PASSO 3 — Configurar o projeto (tela de setup)

**É o passo mais importante.** Configure assim:

| Campo | Valor |
|-------|-------|
| **Project Name** | `valore` (ou outro, tanto faz) |
| **Framework Preset** | `Other` |
| **Root Directory** | **`landing/v3`** ← essencial! clica em "Edit" e aponta pra essa pasta |
| **Build Command** | deixa vazio (ou `None`) |
| **Output Directory** | deixa vazio |
| **Install Command** | deixa vazio |

Depois clica em **"Deploy"**.

---

## PASSO 4 — Esperar o primeiro deploy

Leva ~30 segundos. Vai aparecer uma animação de confete e o botão **"Continue to Dashboard"**.

Clica. Copia o link do domínio (algo tipo `https://valore-abc123.vercel.app`).

**Abre o link no navegador.** A LP tem que carregar inteira — logo, fontes, fotos, vídeos.

---

## PASSO 5 — Mandar pro Gustavo aprovar

Manda o link do Vercel pra ele. Pede feedback por áudio ou texto.

Sugestão de mensagem:

> *"Gu, ficou pronta a nova versão da LP (V3). Dá uma olhada aqui: https://valore-xxxxx.vercel.app — testa no celular e no computador, anota tudo que achar que tem que mudar. Esse link não é o site final ainda, é só pra aprovação."*

---

## PASSO 6 — Fazendo ajustes depois do feedback

Cada push na branch `main` **atualiza o link do Vercel automaticamente**. Então o fluxo é:

1. Abre VS Code
2. Pede a mudança pro Claude
3. Fala *"commita e sobe"*
4. Em 30s o link do Vercel já atualizou com a mudança
5. Avisa o Gustavo pra dar F5

Esse ciclo se repete até ele aprovar tudo.

---

## PASSO 7 — Quando for aprovado, subir na HostGator

Aí sim você usa o [`INSTRUCOES-DEPLOY-CLAUDE.md`](./INSTRUCOES-DEPLOY-CLAUDE.md) pra subir no domínio final `sejavalore.com.br`.

O Vercel pode continuar ligado como staging/preview — útil pra toda nova mudança futura antes de subir na HostGator.

---

## 🧠 Como a configuração funciona (pra você entender)

- **Root Directory `landing/v3`** — faz o Vercel enxergar essa pasta como se fosse a raiz do site. As pastas `fonts/` e `media/` ficam no lugar certo.
- **`vercel.json` dentro de `landing/v3/`** — tem um rewrite que faz `/` servir o arquivo `valore_landing_page_v3.html` (porque o Vercel procura `index.html` por padrão, e o nosso tem outro nome).
- **GTM já tá no HTML** — vai trackear visitas no domínio do Vercel também. Se quiser separar dados de preview vs produção no GTM, cria uma variável de ambiente lá.

---

## 🆘 Deu erro?

| Problema | O que fazer |
|----------|-------------|
| Página em branco | Confere se o Root Directory é `landing/v3` (não deixa vazio nem usa raiz) |
| 404 nas fontes/fotos | Caminho relativo errado. Pergunta pro Claude: *"abre o vercel.json e me explica se tá configurado certo"* |
| Vercel não aparece meu repo | Precisa dar permissão de acesso: https://github.com/settings/installations → Vercel → All repositories |
| Deploy falha | Olha o log do build na tela do Vercel. Copia o erro e cola no Claude: *"deploy no Vercel falhou com esse erro: ..."* |
