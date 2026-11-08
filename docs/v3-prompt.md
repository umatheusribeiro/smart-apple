# Prompt — Atualização da Landing Page Valore (v3)

Você vai editar o arquivo `smartapple_landing_page_v2.html` (landing page da **Valore**, marca de produtos Apple sob encomenda direto dos EUA). A página já usa o nome Valore e a paleta da marca, mas ainda tem resquícios da marca antiga (Smart Apple) e informações que precisam sair ou mudar.

Aplique **todas** as alterações abaixo, mantendo o layout escuro, as animações, o comportamento responsivo e tudo o que não for citado aqui exatamente como está. Ao final, salve o arquivo como **`valore_landing_page_v3.html`**.

---

## 0. Regras gerais (valem para a página inteira)

1. **Nunca usar "comprar" e derivados** (comprar, compra, compre, comprou, comprando) em nenhum texto da página: títulos, menus, botões, cards, FAQ, rodapé, `<title>` e meta tags. Sempre usar **"encomendar"** e derivados (encomenda, encomende, encomendou). Lista de trocas na seção 13.
2. **Remover todas as menções a "+2.000"** (envios, pedidos, clientes) e a **"⭐ 5.0"** em qualquer lugar da página (lista na seção 3).
3. **Paleta da marca** (já em uso, manter): verde-limão `#bfcb5b` (destaques/acento), verde petróleo `#003434`, marrom `#6c4128`, marrom escuro `#514138`.
4. Não alterar: textos e vídeos da prova social em vídeo, mascotes (`mascot-gym.png`, `mascot-mac.png`), seção "Sobre o Gustavo" (exceto o trecho citado), links do WhatsApp (`wa.me/5521969662237`), Instagram (`@gu.valore`), Google Tag Manager (`GTM-WRDXQKMP`) e horário de atendimento.

---

## 1. Tipografia — trocar a página inteira para Neue Montreal

- **Remover** o carregamento da Plus Jakarta Sans do Google Fonts (os `<link>` de `fonts.googleapis.com` e `fonts.gstatic.com`).
- **Carregar Neue Montreal localmente** via `@font-face`, arquivos `.woff2` na pasta `./fonts/`:
  - `./fonts/NeueMontreal-Light.woff2` → `font-weight: 300`
  - `./fonts/NeueMontreal-Regular.woff2` → `font-weight: 400`
  - `./fonts/NeueMontreal-Medium.woff2` → `font-weight: 500`
  - `./fonts/NeueMontreal-Bold.woff2` → `font-weight: 600`
  - Usar `font-display: swap` em todos.
- `font-family` global: `'Neue Montreal', -apple-system, BlinkMacSystemFont, 'Helvetica Neue', Arial, sans-serif`.
- Hierarquia de pesos:
  - Títulos (h1, h2) → **600 (Bold)**
  - Subtítulos, h3, destaques em `<strong>`, botões → **500 (Medium)**
  - Texto corrido → **400 (Regular)**
  - Textos de apoio, legendas, rótulos pequenos, rodapé → **300 (Light)**
- Revisar `letter-spacing` e `line-height` dos títulos depois da troca. A Neue Montreal é mais estreita que a Plus Jakarta, então ajuste para não ficar apertado nem largo demais (sugestão para títulos grandes: `letter-spacing: -0.02em`).

---

## 2. Header / navegação — logo da Valore

- **Substituir o texto "Valore"** do header (ao lado do ícone ☰) pelo **logo**: ícone vetorial seguido do texto "Valore".
- **Ícone:** arquivo `./media/logo-valore.svg` (SVG fornecido). Ajustes obrigatórios no SVG:
  - Recortar o `viewBox` para o desenho: `viewBox="233 326 631 428"` (o original é `0 0 1080 1080` e tem muita margem).
  - Remover o comentário do Illustrator e os `<g>` desnecessários.
  - Aplicar `fill="currentColor"` nos dois `<path>` para a cor vir do CSS.
  - Pode ser inline no HTML (preferível, por causa do `currentColor`) ou carregado como arquivo.
- **Texto "Valore"** ao lado do ícone: Neue Montreal **Bold (600)**.
- Cor do conjunto: `#bfcb5b`.
- Ordem e alinhamento: **☰ → ícone → "Valore"**, alinhado à esquerda, centralizado verticalmente na altura do header. Espaço entre ícone e texto: ~8px.
- Tamanho do ícone: ~28px de altura no mobile, ~34px no desktop. O texto acompanha proporcionalmente.
- O logo inteiro é um link para o topo da página (`href="#"` ou âncora do topo).
- O botão "📦 Fazer Encomenda →" à direita continua igual.

---

## 3. Remover "+2.000" e "⭐ 5.0" da página inteira

1. **Selo acima do título do hero** (`.hero-pill`: "🇺🇸 Produtos originais direto dos EUA · +2.000 envios") → **remover o elemento inteiro**. O hero passa a abrir direto no título.
2. **Faixa de números abaixo do hero** (`.stats-strip`: +2.000 / 100% / ⭐ 5.0 / NF) → **remover inteira** e substituir pela faixa de confiança (seção 5) e pela tira de unboxing (seção 6).
3. **Seção "Sobre o Gustavo"**: trocar "Já foram mais de 2.000 envios com nota fiscal e frete grátis." por **"Todo envio sai com nota fiscal e frete grátis."**
4. **Topo do bloco de avaliações** ("⭐ Avaliações de clientes +2.000 pedidos") → remover o "+2.000 pedidos" e deixar só **"⭐ Avaliações de clientes"**.
5. **CTA final**: trocar "Mais de 2.000 clientes já fizeram a encomenda deles com segurança, produto lacrado, nota fiscal e frete grátis. Agora é a sua vez." por **"Encomende com segurança: produto lacrado, nota fiscal, contrato e frete grátis. Agora é a sua vez."**

Depois das trocas, buscar no arquivo por "2.000", "2000" e "5.0" para garantir que não sobrou nenhuma ocorrência.

---

## 4. Hero — novo subtítulo

O título (h1 com o efeito de digitação "Encomende seu [iPhone] direto dos EUA com a Valore.") **não muda**.

Substituir o parágrafo `.hero-sub` inteiro por:

> Importação, taxas e burocracia? **A Valore cuida de tudo.** Você recebe o seu produto **lacrado e original** e **acompanha cada etapa** até a entrega na sua casa.

- Os trechos em negrito ficam em `#bfcb5b` (mesmo estilo de destaque usado hoje: `<strong style="color:var(--accent)">`), peso 500.
- O restante do texto mantém a cor cinza-claro atual.

---

## 5. Nova faixa de confiança (abaixo do hero, no lugar da `.stats-strip`)

Faixa com 4 itens, mesmo fundo/borda sutis da faixa que está saindo:

| Ícone | Texto |
|---|---|
| 🧾 | Nota fiscal em todo pedido |
| 📄 | Contrato assinado antes do pagamento |
| 📦 | Frete grátis |
| 💬 | Atendimento consultivo pra te ajudar a escolher o produto ideal |

- **Desktop:** uma linha só, 4 colunas. O 4º item é mais longo, então use colunas flexíveis (`flex: 1 1 auto`) ou permita quebra em 2 linhas dentro do item, sem desalinhar os demais.
- **Mobile:** grade 2×2.
- Ícone acima ou à esquerda do texto (manter o padrão visual da faixa antiga). Texto em Neue Montreal 500.

---

## 6. Nova tira de unboxing de clientes (logo abaixo da faixa de confiança)

- Tira horizontal com **4 fotos** de unboxing de clientes reais.
- Arquivos (placeholders, o cliente vai substituir): `./media/unboxing/unboxing-1.jpg` a `unboxing-4.jpg`.
- Cada foto com uma legenda curta abaixo: **primeiro nome + produto** (placeholder: "Nome · Produto").
- Cantos arredondados no mesmo raio dos outros cards da página, `loading="lazy"`, `object-fit: cover`, proporção 4:5.
- **Desktop:** 4 fotos lado a lado. **Mobile:** rolagem horizontal com scroll-snap (sem biblioteca).
- Sem título de seção. É uma prova visual rápida, não uma seção nova.

---

## 7. Seção de prazos ("Você sabe exatamente quando vai receber.")

Trocar os dois cards por:

**Card 1**
> 📦 **Encomenda nacional**
> Quando o seu modelo já está disponível no Brasil, a entrega leva de 4 a 7 dias úteis após a confirmação do pagamento.

**Card 2**
> 🇺🇸 **Importação dos EUA**
> Entrega em 20 a 30 dias úteis do seu produto lacrado americano.

O resto da seção (título, subtítulo, nota "🔍 Tudo rastreável…", botões) continua igual.

---

## 8. Passo a passo (`#como-comprar`) — de 7 para 6 etapas

- Renomear o `id` da seção para `#como-encomendar` e atualizar **todos** os links/âncoras que apontam para ela (menu desktop, menu mobile, quick nav, rodapé).
- Título da seção: **"Como Encomendar?"**
- Substituir as 7 etapas por estas 6:

1. 💬 **Contato inicial**: Me chama no WhatsApp com o produto que você quer.
2. 📱 **Escolha seu modelo**: Te ajudo a escolher o produto ideal pro seu uso, ou [faça o quiz]. *(manter o link do quiz em destaque, como hoje)*
3. 📄 **Contrato de encomenda**: Modelo, cor, valor e prazo por escrito, com assinatura digital e validade jurídica.
4. 💳 **Pagamento**: Em até 12x, só depois do contrato assinado.
5. 🇺🇸 **Importação por nossa conta**: Taxas, impostos e burocracia ficam com a Valore. Você só acompanha cada etapa do trajeto.
6. 📦 **Entrega na sua casa**: Produto lacrado e original na sua porta. Unboxing inesquecível. 🎉

- **Destaque no passo 5:** borda do círculo em `#bfcb5b` mais forte/espessa que os demais (ou leve glow) e o trecho "por nossa conta" do título em `#bfcb5b`. O passo 6 mantém o círculo preenchido de verde-limão como hoje (etapa final).
- Ajustar a linha vertical da timeline para 6 etapas.

---

## 9. Remover a seção "Produtos em Destaque" (`#produtos`)

Remover **a seção inteira**:
- rótulo "Veja a qualidade", título "Produtos em Destaque" e subtítulo
- carrossel/marquee de produtos (`produto-1.jpg` a `produto-4.jpg`)
- frase "Todos os produtos chegam em perfeito estado, lacrados e com nota fiscal."
- botão "📦 Escolha seu modelo →"

Remover também o CSS e o JS usados **somente** por essa seção (ex.: o marquee de produtos).

**Swiper:** o JS já tem os comentários "prodSwiper removido" e "depSwiper removido". Confirme que nenhuma outra parte usa Swiper e, se for o caso, **remova** o `<link>` do `swiper-bundle.min.css` e o `<script>` do `swiper-bundle.min.js`.

---

## 10. Seção de pagamento (`#pagamento`)

- Título: trocar "Flexibilidade para comprar do seu jeito." por **"Flexibilidade para pagar do seu jeito."**
- **Card "💳 Cartão de Crédito"**: remover o item "Via link de pagamento seguro" e trocar "Aceita mais de 1 cartão" por **"Podendo usar mais de 1 cartão"**. O card fica com:
  - ✓ Parcelamento em até 12x
  - ✓ Podendo usar mais de 1 cartão
- **Remover** o aviso "⚠️ Atenção: Para segurança de ambas as partes, não aceitamos parcelamento em boleto." (o elemento inteiro).
- Os 3 cards (PIX, Combinado, Cartão) devem continuar com **a mesma altura**, com o conteúdo alinhado no topo, mesmo que o card de Cartão tenha menos itens.

---

## 11. Seção do quiz

- Subtítulo novo:
  > Responda algumas perguntas rápidas e descubra qual o produto Apple ideal para o seu uso, antes de fazer sua encomenda.
- **Trocar o link do quiz** em **todas** as ocorrências (hoje são 3, apontando para `https://smartapplequiz.lovable.app/`) por **`[URL_DO_QUIZ]`**. Deixe exatamente esse marcador para ser substituído depois. Pode centralizar numa variável JS/constante ou só trocar as 3 ocorrências.

---

## 12. Seção do contrato (`#contrato`, "Segurança em primeiro lugar.")

- **Remover o card mockup** "📋 CONTRATO DE ENCOMENDA" (`.contract-mock`, com Produto / Configuração / Valor combinado / Prazo / Origem / Garantia / assinaturas).
- O mascote `mascot-ipad.png` que fica nessa coluna pode continuar, pequeno, ao lado ou acima do texto, se couber bem no layout. Se ficar estranho sem o card, remova.
- A seção passa a ter **uma coluna**, com o texto centralizado e largura máxima confortável de leitura (~640px). Não deixar espaço vazio onde estava o card.
- Novo conteúdo:

> **SUA PROTEÇÃO** *(rótulo)*
> ## Segurança em **primeiro lugar.** *(manter o destaque em verde em "primeiro lugar.")*
>
> Antes de qualquer pagamento, te enviamos o **contrato de encomenda** com todos os seus dados e os nossos. Ele existe pra te assegurar de tudo que possa acontecer com a sua encomenda.
>
> ✓ Seus dados e os dados da empresa
> ✓ O produto exato (modelo, cor, armazenamento)
> ✓ Valor e forma de pagamento
> ✓ Prazo total de entrega
> ✓ Cláusulas de atraso, taxas alfandegárias, extravio e mais
>
> Tudo autenticado e assinado digitalmente. **Você paga depois de assinar. Não antes.**

- O bloco "🤔 Ainda não sabe qual modelo é o ideal? … 🎯 Fazer o Quiz →" que fica nessa seção continua, com o link trocado para `[URL_DO_QUIZ]`.

---

## 13. FAQ (`#faq`)

- Título: trocar "Tudo o que você precisa saber antes de comprar." por **"Tudo o que você precisa saber antes de encomendar."**
- Atualizar estas respostas (as demais continuam iguais):

**Os produtos são novos?**
> Sim. Novos, lacrados e com **1 ano de garantia mundial Apple**.

**Qual o prazo de entrega?**
> Depende de onde vem o seu produto. Encomenda nacional: de 4 a 7 dias úteis. Importação direto dos EUA: de 20 a 30 dias úteis. O prazo exato fica no contrato, antes de qualquer pagamento.

**Os produtos são a pronta entrega?**
> Não. Trabalhamos sob encomenda por dois motivos:
> **1. Procedência garantida:** você sabe exatamente de onde vem o seu produto, com toda a licitude.
> **2. Do seu jeito:** você recebe exatamente o modelo, a cor, a memória e a configuração que escolheu.

**Qual a garantia de que vou receber o produto?**
> O contrato. Ele é a sua maior garantia, e por isso nada acontece sem ele.
> O contrato te assegura de tudo que possa acontecer com a sua encomenda: tem cláusulas de atraso e de extravio, e todas as taxas de importação já são pagas pela Valore e estão inclusas no valor do contrato.
> **Você está protegido e assegurado por lei.**

- A pergunta "Qual o CNPJ da empresa?" **continua** no FAQ, como está. O CNPJ aparece só no FAQ e no rodapé. **Não** colocar no hero nem na faixa de confiança.

---

## 14. Trocas de "comprar" → "encomendar" (lista completa)

| Onde | Antes | Depois |
|---|---|---|
| Menu desktop, menu mobile, rodapé | Como Comprar | **Como Encomendar** |
| Quick nav mobile | 📋 Compra | **📋 Encomenda** |
| Prova social (h2) | Veja quem já comprou e aprovou. | **Veja quem já encomendou e aprovou.** |
| "Encomende com segurança" (subtítulo) | Sua compra é formalizada em contrato jurídico… | **Sua encomenda é formalizada em contrato jurídico…** |
| Card "Contrato com validade jurídica" | Sua compra é um documento legal. | **Sua encomenda é um documento legal.** |
| Card "Nota Fiscal em todo pedido" | …documento oficial da sua compra. | **…documento oficial da sua encomenda.** |
| Sobre o Gustavo | …tinham medo de comprar online e perder o dinheiro. | **…tinham medo de encomendar online e perder o dinheiro.** |
| Selo do Gustavo | Você não compra só por fé. Você compra protegido por lei. | **Você não encomenda só na fé. Você encomenda protegido por lei.** |
| Avaliações (subtítulo) | Avaliações reais de quem já comprou com a Valore. | **Avaliações reais de quem já encomendou com a Valore.** |
| Passo a passo (h2) | Como Comprar? | **Como Encomendar?** |
| Passo a passo (etapa 3) | Contrato de Compra | **Contrato de encomenda** *(já na seção 8)* |
| Pagamento (h2) | Flexibilidade para comprar do seu jeito. | **Flexibilidade para pagar do seu jeito.** |
| FAQ (h2) | …antes de comprar. | **…antes de encomendar.** |
| CTA final (rótulo) | Pronto pra comprar? | **Pronto pra encomendar?** |

**Exceção:** as falas dos clientes nos depoimentos (aspas com nome) são falas reais e **não devem ser editadas**. Mantenha o texto delas como está.

Depois das trocas, buscar no arquivo por "compr" (sem diferenciar maiúsculas) e confirmar que só restaram ocorrências dentro das falas de clientes ou em código (nomes de classes/ids, que podem ser renomeados também).

---

## 15. Limpeza final

- Nenhuma referência a "smartapple" / "Smart Apple" no arquivo (texto, URLs, comentários, nomes de classe).
- Nenhum CSS/JS órfão das seções removidas (stats-strip, hero-pill, produtos, contract-mock, aviso de boleto).
- Todas as âncoras do menu funcionando (`#sobre`, `#como-encomendar`, `#depoimentos`, `#pagamento`, `#contrato`, `#faq`).
- Testar em 375px (mobile), 768px (tablet) e 1440px (desktop): nada estourando, faixa de confiança em 2×2 no mobile, tira de unboxing com rolagem horizontal no mobile, cards de pagamento com a mesma altura.
- Salvar como `valore_landing_page_v3.html`.

---

## Arquivos que precisam existir (fornecidos pelo cliente)

- `./media/logo-valore.svg`: ícone vetorizado da Valore (ou inline no HTML)
- `./fonts/NeueMontreal-Light.woff2`, `-Regular.woff2`, `-Medium.woff2`, `-Bold.woff2`
- `./media/unboxing/unboxing-1.jpg` a `unboxing-4.jpg`: fotos reais de unboxing de clientes
- URL nova do quiz → substituir `[URL_DO_QUIZ]`
