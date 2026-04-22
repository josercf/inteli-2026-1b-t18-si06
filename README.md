# Inteli 2026/1B, SI06 Turma T18

Apresentações de orientação da Turma T18 do Módulo 6 de Sistemas de Informação (Inteli, 2026/1B).

## Conteúdo

A pasta `aulas/` reúne os decks em Reveal.js usados nos encontros de orientação da turma. O primeiro deck é o de onboarding e Sprint Planning 1, referente ao encontro de 22/04/2026.

- `aulas/index.html`: índice das aulas de orientação do módulo.
- `aulas/onboarding-sprint-1.html`: deck do dia 22/04, abertura do módulo.
- `aulas/exports/onboarding-sprint-1.pptx`: versão em PowerPoint com slides renderizados como imagem.
- `aulas/assets/css/inteli-theme.css`: tema visual Inteli aplicado aos slides.
- `aulas/assets/img/docentes/`: fotos dos docentes e da coordenação.
- `scripts/build-ppt.py`: regenera o `.pptx` a partir do deck HTML.

## Como rodar localmente

```bash
npx serve aulas
```

Depois acesse `http://localhost:3000/` para abrir o índice ou `http://localhost:3000/onboarding-sprint-1.html` para o deck diretamente.

## Exportar para PDF

Abra o deck e acrescente `?print-pdf` na URL, por exemplo `onboarding-sprint-1.html?print-pdf`. Use `Ctrl+P` ou `Cmd+P` no navegador e salve como PDF, com margem zero e opção de gráficos de fundo ligada.

## Regenerar a versão PowerPoint

```bash
pip install playwright python-pptx --break-system-packages
python3 -m playwright install chromium
python3 scripts/build-ppt.py onboarding-sprint-1
```

O script sobe um servidor estático local, captura cada slide em PNG 1920x1200 e monta um `.pptx` em `aulas/exports/`.

## Estrutura

```
.
├── aulas/
│   ├── assets/
│   │   ├── css/inteli-theme.css
│   │   ├── js/
│   │   └── img/docentes/
│   ├── exports/
│   │   └── onboarding-sprint-1.pptx
│   ├── index.html
│   └── onboarding-sprint-1.html
├── imagens/
│   └── docentes/
├── scripts/
│   └── build-ppt.py
├── GUIA_INSTRUCOES.md
└── README.md
```

## Paleta Inteli aplicada

| Cor | Hex |
|-----|-----|
| Purple | #2E2640 |
| Coral | #FF4545 |
| Lilac | #90A5E5 |
| Green | #89CEA5 |
| Dark Green | #066D73 |

Tipografia: **Manrope** para títulos e corpo, **Space Mono** para trechos de código.
