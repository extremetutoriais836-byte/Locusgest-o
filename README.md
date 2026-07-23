# Landing Page — Locus Company

Página institucional e simulador de gestão da Locus, para delivery no iFood e 99Food.

Site estático de arquivo único: todo o CSS, JavaScript, fontes (Poppins) e imagens estão embutidos em `index.html` via base64. Sem build, sem dependências, sem requisições externas.

---

## Estrutura

```
.
├── index.html            ← a página inteira (HTML + CSS + JS + fontes + logos)
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

Os ícones e o card social usam o **símbolo oficial da Locus**. O logotipo completo (wordmark) está embutido em base64 dentro do `index.html`, no header e no footer.

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
| Preços | `var PRECOS={consultoria:150, implementacao:500, semanal:{trimestral:200, mensal:250}, semanas:{trimestral:12, mensal:4}}` |
| Clientes em destaque | seção `<section id="clientes">` |
| Entregas de cada semana | `var SEMANAS=[...]` |
| Feed do painel do hero | `var FEED=[...]` |
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
