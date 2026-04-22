# Como Criar um Projeto de Aulas com Reveal.js e Tema Inteli

## Começando

Você quer replicar a estrutura de um projeto de apresentações interativas? Este guia cobre tudo que você precisa saber, desde a organização inicial até o deploy.

O projeto que você tem como referência usa Reveal.js 5 para apresentações HTML, com um tema visual corporativo da Inteli. Inclui 6 aulas, componentes reutilizáveis (cards, tabelas, timers) e um fluxo de desenvolvimento limpo com Prettier e Husky.

## O que você vai precisar

Antes de começar, tenha isso à mão:

- Node.js 14 ou mais novo
- npm instalado
- Git
- Um editor de código (VS Code funciona bem)
- Noções básicas de HTML e CSS

## A estrutura do projeto

Crie esta árvore de pastas:

```
meu-modulo-aulas/
├── aulas/
│   ├── assets/
│   │   ├── css/
│   │   │   ├── inteli-theme.css
│   │   │   └── inteli-print.css
│   │   ├── js/
│   │   │   ├── inteli-quiz.js
│   │   │   └── inteli-zoom.js
│   │   └── img/
│   ├── aula01.html
│   ├── aula02.html
│   ├── index.html
│   └── index.md
├── .github/workflows/
│   └── deploy-pages.yml
├── .husky/
│   └── pre-commit
├── .gitignore
├── .prettierignore
├── package.json
├── package-lock.json
└── README.md
```

## Passo 1: Preparar o Git

```bash
mkdir meu-modulo-aulas
cd meu-modulo-aulas
git init
git config user.name "Seu Nome"
git config user.email "seu.email@inteli.edu.br"
```

## Passo 2: Configurar Node

```bash
npm init -y
```

Depois atualize o `package.json`:

```json
{
  "name": "meu-modulo-aulas",
  "version": "1.0.0",
  "description": "Apresentações do módulo de [Seu Módulo]",
  "scripts": {
    "lint": "prettier --check .",
    "format": "prettier --write ."
  },
  "devDependencies": {
    "husky": "^8.0.0",
    "lint-staged": "^13.0.0",
    "prettier": "^2.8.0"
  }
}
```

Depois instale:

```bash
npm install
```

## Passo 3: Criar os diretórios

```bash
mkdir -p aulas/assets/css aulas/assets/js aulas/assets/img
mkdir -p .github/workflows .husky
```

## Cores que você vai usar

O Inteli tem uma paleta bem definida. Use essas cores:

| Cor | Hex | Onde usar |
|-----|-----|-----------|
| Purple | #2E2640 | Fundos e títulos |
| Coral | #FF4545 | Destaques, setas |
| Lilac | #90A5E5 | Cards secundários |
| Green | #89CEA5 | Status positivo |
| Dark Green | #066D73 | Bordas de destaque |

Tipografia: Use **Manrope** para títulos e texto, **Space Mono** para código.

## O arquivo inteli-theme.css

Crie `aulas/assets/css/inteli-theme.css` com as variáveis de cor:

```css
:root {
  --inteli-purple: #2E2640;
  --inteli-coral: #FF4545;
  --inteli-lilac: #90A5E5;
  --inteli-green: #89CEA5;
  --inteli-dark-green: #066D73;
  --inteli-white: #FFFFFF;
  --inteli-gray-light: #F5F5F5;
  --inteli-gray-dark: #333333;
  --inteli-bg-soft: #F9F9F9;
  --inteli-lilac-soft: #E8EFFE;
  --inteli-purple-deep: #1a1428;
  --inteli-mono: 'Space Mono', monospace;
}

/* Variáveis do Reveal.js */
:root {
  --r-font-family-headings: 'Manrope', sans-serif;
  --r-font-family: 'Manrope', sans-serif;
  --r-heading1-size: 3em;
  --r-heading2-size: 2.5em;
  --r-heading3-size: 2em;
  --r-heading4-size: 1.5em;
  --r-heading5-size: 1.3em;
  --r-heading6-size: 1.1em;
  --r-link-color: var(--inteli-coral);
  --r-link-color-hover: #ff2f2f;
  --r-text-color: var(--inteli-purple);
}

/* Estilos globais */
body {
  background: var(--inteli-white);
  color: var(--inteli-purple);
}

.reveal {
  font-family: var(--r-font-family);
  color: var(--r-text-color);
}

.reveal h1, .reveal h2, .reveal h3, .reveal h4, .reveal h5, .reveal h6 {
  font-family: var(--r-font-family-headings);
  color: var(--inteli-purple);
  font-weight: 700;
}
```

## Os componentes de slide

Cada aula tem um padrão. Conheça os principais:

### Slide de capa

```html
<section class="title-slide">
  <h1>Título da Aula</h1>
  <p class="subtitle">Subtítulo descritivo</p>
  <div class="aula-meta">
    <span class="aula-number">Aula 01</span>
    <span class="aula-date">Data</span>
  </div>
</section>
```

### Slide de conteúdo

```html
<section class="content-slide">
  <div class="top-bar"></div>
  <span class="inteli-logo-header inteli-wordmark">inteli</span>
  <div class="slide-title-area">
    <div class="accent-bar"></div>
    <h2>Título do Conteúdo</h2>
  </div>
  <div class="slide-body">
    <!-- Seu conteúdo aqui -->
  </div>
  <div class="slide-footer">
    <div class="footer-bar">Aula XX, Tema, Sprint X</div>
    <div class="footer-page">00</div>
  </div>
</section>
```

### Separador de bloco

```html
<section class="section-slide">
  <span class="section-number">Bloco 1, 35 min</span>
  <h2>Título do Bloco</h2>
  <p class="section-description">Descrição breve do que será abordado.</p>
</section>
```

### Com timer (Daily Time)

```html
<section class="daily-slide">
  <span class="daily-tag">Daily, 15 min</span>
  <h2>Daily Time</h2>
  <p class="daily-subtitle">Descrição da atividade.</p>
  <div class="inteli-timer" data-minutes="15">
    <div class="inteli-timer-display">15:00</div>
    <div class="inteli-timer-controls">
      <button data-action="start">Iniciar</button>
      <button data-action="pause">Pausar</button>
      <button data-action="reset">Zerar</button>
    </div>
  </div>
</section>
```

### Cards de conceito

```html
<div class="concept-cards">
  <div class="concept-card">
    <h4>Compreender</h4>
    <p>Descrição da habilidade ou conceito.</p>
  </div>
  <div class="concept-card">
    <h4>Aplicar</h4>
    <p>Outra descrição.</p>
  </div>
</div>
```

## Como criar uma aula

Comece com este template HTML:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=1280, initial-scale=1.0" />
  <title>Aula XX, Tema, Inteli T13</title>

  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=Manrope:wght@400;500;600;700;800&family=Space+Mono:wght@400;700&display=swap"
    rel="stylesheet"
  />

  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/reveal.js/5.1.0/reveal.min.css" />
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/reveal.js/5.1.0/theme/white.min.css" />
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/styles/monokai.min.css" />

  <link rel="stylesheet" href="assets/css/inteli-theme.css" />

  <style>
    /* Estilos personalizados desta aula */
  </style>
</head>
<body>
  <div class="reveal">
    <div class="slides">
      <!-- Seus slides aqui -->
    </div>
  </div>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/reveal.js/5.1.0/reveal.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/reveal.js/5.1.0/plugin/zoom/zoom.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/reveal.js/5.1.0/plugin/notes/notes.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/reveal.js/5.1.0/plugin/search/search.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/reveal.js/5.1.0/plugin/markdown/markdown.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/reveal.js/5.1.0/plugin/highlight/highlight.min.js"></script>

  <script>
    Reveal.initialize({
      hash: true,
      plugins: [RevealZoom, RevealNotes, RevealSearch, RevealMarkdown, RevealHighlight],
      transition: 'slide',
      transitionSpeed: 'default',
    });
  </script>
</body>
</html>
```

## A ordem dos slides

Mantenha essa sequência em cada aula:

1. Capa com título
2. Cronograma/agenda
3. Aquecimento ou Daily Time (com timer de 15 min)
4. Objetivos de aprendizagem (com concept cards)
5. Blocos temáticos (separadores + conteúdo)
6. Encerramento e síntese

## Deploy no GitHub Pages

Crie `.github/workflows/deploy-pages.yml`:

```yaml
name: Deploy to Pages

on:
  push:
    branches: ['main']
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: 'aulas'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

Depois ative em Settings > Pages e escolha "GitHub Actions" como source.

## Rodar localmente

Para ver as aulas enquanto trabalha:

```bash
npx serve aulas
```

Acesse `http://localhost:3000` no navegador.

## Dicas que funcionam

- Manter consistência visual entre aulas
- Testar em Chrome, Firefox e Safari
- Otimizar imagens antes de adicionar
- Usar Prettier para manter código formatado
- Documentar estilos customizados em cada aula
- Revisar contraste de cores (acessibilidade)
- Testar navegação com teclado

## Problemas comuns

**Fonte Manrope não aparece?**
Verifique se os links do Google Fonts no `<head>` estão corretos e acessíveis.

**Tema não está aplicando cores?**
Certifique-se de que `inteli-theme.css` carrega DEPOIS de `white.min.css` do Reveal.

**Deploy não atualiza?**
Ative em Settings > Pages, escolha "GitHub Actions" como source, e aguarde o workflow executar.

## Referências úteis

- [Reveal.js docs](https://revealjs.com/)
- [MDN Web Docs](https://developer.mozilla.org/)
- [GitHub Pages](https://pages.github.com/)
- [Google Fonts](https://fonts.google.com/)

## Resumo

Você agora tem tudo que precisa para criar um projeto profissional de aulas com Reveal.js. A chave é manter consistência nos componentes e cores, revisar em múltiplos navegadores, e documentar o que você customizou.

Boa sorte com suas aulas!
