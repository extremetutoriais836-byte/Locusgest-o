# Landing Page — Locus Company

Página institucional e simulador de gestão da Locus, para delivery no iFood e 99Food.

Site estático de arquivo único: todo o CSS, JavaScript, fontes (Poppins) e imagens estão embutidos em `index.html` via base64. Sem build, sem dependências, sem requisições externas.

---

## Estrutura

```
.
├── index.html            ← a página inteira (fontes, logotipo, logos de clientes e fotos embutidos)
├── depoimento-igor.m4a   ← depoimento em áudio (Safari, Chrome, Edge)
├── depoimento-igor.ogg   ← depoimento em áudio (Firefox)
├── og-image.png          ← preview no WhatsApp, Instagram e redes (1200×630)
├── favicon-32.png        ← ícone da aba
├── icon-192.png          ← ícone Android / PWA
├── icon-512.png          ← ícone Android / PWA
├── apple-touch-icon.png  ← ícone iOS (tela de início)
├── site.webmanifest      ← manifesto PWA
├── vercel.json           ← headers de cache e segurança
├── robots.txt
├── sitemap.xml
└── .gitignore
```

**Todos os arquivos ficam na raiz, sem subpastas.** Isso é proposital: subpastas se perdem com facilidade no upload pela interface web do GitHub, e foi o que aconteceu numa versão anterior — as imagens e o áudio sumiram do site porque a pasta `media/` não subiu junto.

Hoje só existem dois arquivos que o `index.html` busca de fora: `depoimento-igor.m4a` e `depoimento-igor.ogg`. **Se esses dois não estiverem no repositório, o player mostra um aviso** em vez de um botão morto. Todo o resto — fontes, logotipo, os 140 logos de clientes, a foto do CEO e a capa do vídeo — está embutido dentro do próprio HTML e não depende de nenhum arquivo externo.

Os ícones e o card social usam o **símbolo oficial da Locus**. O logotipo completo (wordmark) está embutido em base64 dentro do `index.html`, no header e no footer.

**Sobre o peso:** o `index.html` tem ~1,1 MB porque carrega tudo dentro de si. Em troca, a página não quebra se algum arquivo se perder no deploy. O vídeo do CEO fica no YouTube e só é carregado no clique, então **a página abre sem nenhuma requisição externa**.

---

## Deploy no Vercel

**1. Subir para o GitHub**

```bash
git init
git add .
git commit -m "Landing page Locus"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/SEU-REPO.git
git push -u origin main
```

**2. Conectar no Vercel**

1. Acesse [vercel.com/new](https://vercel.com/new) e importe o repositório
2. Framework Preset: **Other**
3. Build Command: deixe **vazio**
4. Output Directory: deixe **vazio** (a raiz já é o site)
5. Deploy

A partir daí, todo `git push` na branch `main` publica automaticamente.

**3. Domínio**

Em Settings → Domains, adicione o domínio. Depois disso, troque `https://locus.com.br` pelo domínio real em três lugares:

- `index.html` — nas tags `og:url`, `og:image`, `twitter:image` e `canonical`
- `robots.txt` — na linha do Sitemap
- `sitemap.xml` — na tag `<loc>`

Sem isso o preview no WhatsApp não carrega a imagem.

---

## Como editar

Tudo está em `index.html`. Os pontos que mais mudam:

| O que mudar | Onde procurar |
|---|---|
| Número do WhatsApp | `var WAPP='5582991477682'` |
| Chave PIX | `var PIX='46.073.978/0001-84'` |
| Preços e desconto | `var PRECOS={consultoria:350, implementacao:350, …, descontoTrimestral:50}` |
| Painel ao vivo | `var DASH={mediaMensal, ticketBase}` |
| Clientes em destaque | seção `<section id="clientes">` |
| Entregas de cada semana | `var SEMANAS=[...]` |
| Feed do painel do hero | `var FEED=[...]` |
| Instagram (Locus e CEO) | seção `<section id="lideranca">`, links `.ig` |
| Vídeo do CEO | atributo `data-yt` em `<div class="vwrap">` |
| Número de lojas | busque por `+160` e `160 lojas` |
| Dashboard do hero | bloco `.fat` e `.conta` na seção do topo |
| Botão flutuante | `<a class="fab">` antes do `<dialog>` |
| Perguntas frequentes | seção `<section id="faq">` |
| Instagram dos clientes em destaque | `href` de cada `<a class="cliente">` |
| Instagram da equipe | `href` de cada `<a class="pessoa">` |
| Termos e política | conteúdo dentro de `<dialog class="legal">` |
| Fotos e cargos da equipe | seção `<section id="equipe">` |
| Link do grupo do WhatsApp | busque por `chat.whatsapp.com` (aparece na fita e no botão) |
| Texto da fita de live | `<span class="tick-item">` — as 8 cópias devem ser iguais |
| Outros serviços | seção `<section id="servicos">` |
| Texto do time de CS | bloco `<div class="cs">` |
| Velocidade do carrossel | `style="--dur:150s"` em cada `.mtrack` |
| Cores da marca | bloco `:root{...}` no topo do CSS |
| Logotipo (header e footer) | `<a class="logo">` e `<span class="logo">`, imagem em base64 |

### Preços

`semanas` define quantas semanas entram no total estimado: 12 no plano trimestral (3 meses), 4 no mensal. Se o modelo comercial mudar, é só ajustar esses números — o resumo e a mensagem do WhatsApp recalculam sozinhos.

### Adicionar um cliente novo

O logo precisa estar em base64 dentro do próprio HTML. Converta a imagem (recomendado: WebP, 320×320, recorte circular) e cole no `src`:

```html
<article class="cliente rv">
  <img src="data:image/webp;base64,COLE_AQUI" alt="Logo Nome da Loja" width="94" height="94">
  <h3>Nome da Loja</h3>
  <div class="uf">Cidade · UF</div>
  <div class="stat"><span data-count="1500" data-prefix="+">+1.500</span> pedidos/mês<small>no iFood</small></div>
</article>
```

O atributo `data-count` faz o número subir animado quando entra na tela.

---

## Trocar o vídeo, a foto ou o depoimento

Substitua o arquivo dentro de `media/` mantendo o mesmo nome e o site já usa o novo. Alguns cuidados:

**Vídeo do CEO** — está no YouTube. Para trocar, pegue o ID do link e mude o atributo `data-yt`:

```html
<div class="vwrap" id="vwrap" data-yt="XW_DFctxi_Y">
```

O ID é o trecho depois de `/shorts/` ou de `watch?v=`. Em `https://youtube.com/shorts/XW_DFctxi_Y`, o ID é `XW_DFctxi_Y`.

A capa continua sendo uma imagem local (`a capa do vídeo`), o que faz o bloco aparecer instantaneamente e mantém a identidade da página. Gere uma capa nova a partir do arquivo original (`-ss 40` escolhe o segundo do frame):

```bash
ffmpeg -ss 0.4 -i original.mp4 -frames:v 1 -vf "scale=405:-2" capa.jpg
```

Depois converta para base64 e substitua a string dentro de `<img class="vposter" src="data:image/webp;base64,…">`:

```bash
base64 -w0 capa.webp
```

Depois, ajuste a duração exibida no botão, em `<small>1 min 26 · Isaac Leonardo</small>`.

O player usa `youtube-nocookie.com` e só é injetado no clique — nada do YouTube é carregado antes disso, então a página não abre conexões externas nem cookies de terceiros para quem não assistir.

**Depoimento em áudio** — precisa dos dois formatos, porque nenhum navegador toca os dois:

```bash
ffmpeg -i original.ogg -c:a aac -b:a 32k -ac 1 -ar 24000 depoimento-igor.m4a
ffmpeg -i original.ogg -c:a libvorbis -q:a 0 -ac 1 -ar 24000 depoimento-igor.ogg
```

Áudios de WhatsApp costumam vir sem a duração no cabeçalho, e por isso o player usa um valor de referência fixo. Ao trocar o arquivo, atualize `var DUR=106.7` no JavaScript com a duração real em segundos:

```bash
ffprobe -v error -show_entries format=duration -of csv=p=0 depoimento-igor.ogg
```

---

## Os 140 clientes

Todos os logos ficam em uma única imagem embutida no CSS (a variável `--sprite`), organizada numa grade de **14 colunas por 10 linhas**. Cada chip no carrossel indica sua posição por dois números:

```html
<span class="cchip" style="--i-col:5;--i-row:4"><i></i><b>Casal do Açaí</b><em>São Paulo · SP</em></span>
```

`--i-col` é a coluna (0 a 13) e `--i-row` a linha (0 a 9). O cliente de índice `n` na grade fica em `--i-col: n % 14` e `--i-row: n / 14` (divisão inteira).

Para adicionar clientes, o caminho mais simples é aumentar a grade: monte um sprite novo com o mesmo número de colunas, acrescente as linhas necessárias e atualize `background-size` em `.cchip i` e `.depo-av` — o valor é `(colunas × 100)% (linhas × 100)%`, e os divisores nas contas de `background-position` são `colunas - 1` e `linhas - 1`.

Os chips estão divididos em duas esteiras que rolam em sentidos opostos. Cada esteira repete a própria lista duas vezes — é isso que faz o loop parecer infinito, então **ao editar, mantenha as duas cópias idênticas**.


---

## Clientes em destaque

Os cinco cards da seção "Lojas que já destravaram" são **links para o Instagram de cada loja**. Cada um tem um selo discreto no canto, que fica azul no hover, e o `@` aparece embaixo (no celular, onde não existe hover, o `@` fica sempre visível).

Para trocar ou acrescentar uma loja, mantenha a estrutura do link:

```html
<a class="cliente rv" href="https://www.instagram.com/PERFIL/" target="_blank" rel="noopener"
   aria-label="Ver o Instagram de NOME (@PERFIL)">
```

Esses cinco usam os logos oficiais em alta resolução, embutidos em base64. Os outros 145 do carrossel vêm do sprite e **não são clicáveis** — o link existe só para as lojas em destaque.

---

## A equipe

As nove fotos ficam embutidas em base64 na seção `<section id="equipe">`, todas recortadas em 4:5 com o rosto centralizado e a ~40% do topo. Para trocar ou acrescentar alguém, gere o recorte no mesmo formato, converta para base64 e cole no `src`:

```bash
base64 -w0 foto.webp
```

O card do Isaac tem a classe extra `lider`, que deixa a borda azulada e o cargo em dourado.

A seção fica **depois dos próximos passos**, de propósito: no topo ela competiria com a conversão, e ali embaixo funciona como reforço de confiança para quem já leu a proposta.

---

## Fita de live e grupo do WhatsApp

A fita azul no topo repete a mesma mensagem 8 vezes para o loop parecer contínuo — **ao editar o texto, mude as 8 cópias**. Ela pausa quando o mouse passa por cima e some com `prefers-reduced-motion`.

O link do grupo aparece em dois lugares: na fita e no botão verde da seção de serviços. Se o convite mudar, troque nos dois.



---

## O painel ao vivo do topo

Todo o painel sai de duas constantes:

```js
var DASH = {
  faturamentoAno : 12000000,   // R$ gerados em 12 meses, somando todas as lojas
  ticketBase     : 41.37       // R$ ticket médio por pedido
};
```

Faturamento, pedidos e ticket são derivados daí, então **nunca se contradizem**: dividir o faturamento pelos pedidos dá exatamente o ticket exibido.

**O ritmo dos pedidos não é inventado.** Ele sai da própria base: `faturamentoAno ÷ ticketBase ÷ 365 ÷ 24 ÷ 60` = cerca de **0,55 pedido por minuto**. Sobre isso incide o peso do horário (`PESO_HORA`), então no almoço e no jantar chega perto de 1,2/min e de madrugada cai para 1 a cada 18 minutos.

Na prática o número **fica parado por um a dois minutos e então salta** — é assim que 290 mil pedidos por ano se distribuem no relógio. Por isso o faturamento aparece com centavos e pisca em dourado a cada chegada: como as chegadas são espaçadas, cada uma precisa ser inconfundível.

Se o volume real da base for maior que R$ 12 milhões/ano, basta subir `faturamentoAno` — o ritmo se ajusta sozinho e o painel fica mais movimentado, sem tocar em mais nada.

**Detalhes que evitam cara de número falso:**

- o intervalo entre pedidos segue distribuição exponencial, nunca um tique certinho de X em X segundos
- às vezes dois ou três pedidos entram juntos
- cada pedido vale entre 55% e 145% do ticket, com média exatamente no ticket base, então o ticket exibido não deriva com o tempo
- o acumulado avança um pouco a cada dia e **nunca aparece menor** do que na visita anterior
- o total de pedidos usa espaço como separador de milhar (`297 592`), porque com ponto ele é lido como decimal ao lado do ticket `R$ 41,37`

Com `prefers-reduced-motion` o painel mostra os valores direto, sem animação nem tique.

---

## Desconto do plano trimestral

No trimestral, cada operação além da primeira sai R$ 50 mais barata por semana:

| Operações | Gestão semanal | Desconto |
|---|---|---|
| 1 | R$ 200 | — |
| 2 | R$ 200 + R$ 150 = **R$ 350** | R$ 50/sem |
| 4 | R$ 200 + 3 × R$ 150 = **R$ 650** | R$ 150/sem |

O mensal não tem desconto. O valor está em `PRECOS.descontoTrimestral` e a regra em `semanalUnit()`.

**Atenção:** o desconto conta por *operação*, não por loja. Como cada plataforma é uma operação, uma loja no iFood e no 99Food já ativa o desconto na segunda. Se a intenção for contar só lojas distintas, troque `s.un` por `s.n` em `semanalTotal()`.



## Ordem das seções

A página segue esta narrativa, do resultado à decisão:

1. **Hero** — dashboard de faturamento
2. **Clientes em destaque** — as lojas que mais faturam
3. **Carrossel** — as 160 lojas da base
4. **Liderança** — CEO, vídeo, depoimento e time de CS
5. **Equipe** — quem opera
6. **A gestão na prática** e **o método** — o que é a gestão Locus
7. **Simulação** — o investimento
8. **Próximos passos**, **outros serviços** e **grupo do WhatsApp**

Primeiro a prova (resultado e clientes), depois quem entrega, só então o que é e quanto custa.


---

## Redução de risco: o FAQ

Fica logo depois da simulação, que é onde a objeção nasce — a pessoa acabou de ver o preço. São quatro perguntas em `<details>`, sem JavaScript: prazo de resultado, fidelidade, acessos necessários e se a consultoria obriga a seguir.

A resposta sobre prazo é deliberadamente honesta: **o primeiro mês é de ajuste**, e o que se garante é operação estável com recorrência de pedidos, não um número. Prometer explosão em 30 dias geraria cliente frustrado no mês 2 — o custo de uma promessa quebrada é maior que o de uma venda perdida.

---

## Âncora de faturamento

O campo "faturamento médio por mês" é opcional e não bloqueia o botão do WhatsApp. Preenchido, o recibo mostra quanto a gestão representa da receita da loja:

```
peso = (gestão semanal × 4,33) ÷ faturamento informado
```

O `4,33` é a média de semanas por mês. Uma loja de R$ 30 mil vê "2,9%" em vez de "R$ 866" — mesmo valor, leitura completamente diferente. Abaixo de R$ 500 o bloco não aparece, para evitar percentuais absurdos de quem digitar qualquer coisa.

---

## Como o simulador calcula

A conta gira em torno de **operações gerenciadas**. Cada loja escolhe a própria plataforma, então dá para pedir 3 lojas no 99Food e 1 no iFood sem que o sistema force o mesmo arranjo para todas:

```
operações = soma, loja a loja, de (1 se uma plataforma, 2 se as duas)
```

As três linhas do orçamento seguem regras diferentes, de propósito:

| Item | Como escala | Por quê |
|---|---|---|
| **Consultoria** R$ 350 | por **plataforma distinta** em uso (1 ou 2) | iFood e 99Food têm gestores diferentes, mas o diagnóstico não se repete a cada loja |
| **Implementação** R$ 350 | por **operação** | cada painel precisa ser montado individualmente |
| **Gestão semanal** | por **operação**, com desconto no trimestral | cada painel é trabalhado toda semana |

Na prática: uma loja só no iFood paga uma consultoria (R$ 350); dez lojas só no iFood pagam **a mesma** consultoria; uma única loja nas duas plataformas paga duas (R$ 700), porque envolve dois gestores.

O ponto a ajustar, se a regra mudar, é `valorConsultoria()`.

---

## Fluxo do formulário

O visitante preenche quantidade de lojas, nome de cada uma, plataforma, plano e situação. O resumo à direita calcula em tempo real e o botão do WhatsApp só habilita quando tudo estiver preenchido.

**A chave PIX não aparece na página.** Ela vai apenas na mensagem do WhatsApp, junto da confirmação de que o primeiro pagamento é somente a consultoria de R$ 150.

---

## Identidade visual

Segue o Manual da Marca Locus (2026):

| | |
|---|---|
| Azul Locus | `#5B6FE0` |
| Azul Claro | `#8295FF` |
| Azul Profundo | `#3C4CB0` |
| Azul Glow | `#9FB0FF` |
| Ink · Fundo | `#16161A` |
| Painel | `#1F1F26` |
| Prata · Texto | `#F3F4F8` |
| Cinza | `#7E8190` |
| Verde | `#2E9E6A` |
| Dourado | `#F0B43C` |
| Laranja | `#E07A3C` |
| Vermelho | `#C84B4B` |

Tipografia: Poppins (300 corpo · 600 destaques e labels · 700 títulos).

**Logotipo:** wordmark "Locus" com o símbolo de infinito integrado, na versão branca sobre fundo escuro. O símbolo isolado aparece como ícone do painel no hero, como marca d'água em órbita lenta no fundo e acima do CTA final. Ambos foram exportados com fundo transparente a partir dos arquivos oficiais da marca.

---

## Testar localmente

```bash
python3 -m http.server 8000
```

Depois abra `http://localhost:8000`. Também funciona abrindo o `index.html` direto no navegador.

---

Locus Company Ltda · CNPJ 46.073.978/0001-84 · Maceió/AL
