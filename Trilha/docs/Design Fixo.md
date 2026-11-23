# TRILHA — Identidade Visual & Diretrizes de Design.

Este documento é a fonte da verdade para qualquer alteração visual no projeto TRILHA. Ele deve ser consultado antes de qualquer mudança em templates, CSS ou novos componentes.

---

## 1. Logotipo Oficial

O logotipo é composto por **símbolo + wordmark**. As imagens .png em /static/img devem ser utilizadas como logotipo oficial em todo o projeto.

### Versões
- **Horizontal (completo):** símbolo + "Trilha." — uso padrão no header e footer.
- **Símbolo isolado:** círculo com arcos verde-escuro e "v" mostarda — uso em favicon, avatar, marcas d'água, miniaturas.
- **Wordmark isolado:** "Trilha." em serifa verde-escuro com ponto mostarda — uso em contextos onde o símbolo já está presente.

---

## 2. Paleta de Cores

| Token | Hex | Uso |
|---|---|---|
| `--cream` | `#FAF6EE` | Fundo principal, texto sobre verde |
| `--sand` | `#F0E9DA` | Seções alternadas, faixas |
| `--deep` | `#216B58` | Marca, CTAs primários, títulos de destaque |
| `--dark` | `#243A31` | Texto principal, títulos |
| `--pale` | `#DCE8D4` | Fundos de cartões, ícones, destaques suaves |
| `--sage` | `#9DBBA1` | Barras decorativas, elementos secundários |
| `--mustard` | `#E6A447` | Acentos, traços de rótulo, ponto da marca, foco |
| `--muted` | `#718078` | Texto de apoio, legendas |
| `--line` | `#D7D2C7` | Divisórias finas |
| `--white` | `#FFFDF8` | Cartões, superfícies elevadas |

**Regra:** verde profundo é a cor de marca; mostarda é sempre **acento**, nunca fundo dominante.

---

## 3. Tipografia

| Papel | Fonte | Peso | Uso |
|---|---|---|---|
| Display / Títulos | **Fraunces** | 400 (regular), itálico para ênfase | H1, H2, citações, números serifados |
| Texto / UI | **Manrope** | 400, 500, 600, 700 | Navegação, corpo, botões, legendas, microtexto |

### Regras
- Títulos com `letter-spacing: -.047em` (tracking negativo) e `line-height` entre 0.99 e 1.1.
- Corpo em 14–15px, `line-height: 1.65–1.9`.
- Microtexto (eyebrows, legendas) em 8–11px, `letter-spacing: .14–.16em`, `text-transform: uppercase`.
- **Nunca** usar Fraunces em botões ou navegação; **nunca** usar Manrope em títulos grandes.
- Itálico verde em palavras-chave de títulos é assinatura da marca (ex.: *melhorar*, *conversa*).

---

## 4. Espaçamento, Grid e Layout

- **Container:** `width: min(100% - 56px, 1192px)` (desktop) / `100% - 40px` (mobile) / `100% - 32px` (≤420px).
- **Seções:** `padding-block: clamp(78px, 9vw, 125px)`.
- **Grid editorial:** duas colunas em desktop (texto | demonstração), empilhado em mobile **mantendo a ordem narrativa**.
- **Ritmo:** generoso. Espaço em branco é parte da identidade. Nunca comprimir seções para "caber mais".
- **Bordas:** raio máximo de 3px em botões e cartões. Sem `border-radius` grande (evitar estética "app genérico").
- **Sombras:** deslocadas e planas (`box-shadow: 8px 9px 0 var(--pale)`, `11px 12px 0 ...`). **Nunca** sombras difusas ou `blur` alto.

---

## 5. Componentes de Assinatura

- **Rótulo (eyebrow):** traço mostarda de 27px + texto em maiúsculas verde profundo.
- **Botão primário:** fundo `--deep`, texto creme, seta `↗`, hover escurece para `--dark` e sobe 2px.
- **Botão claro:** fundo creme, texto verde, usado apenas sobre fundo verde.
- **Link de texto:** cor verde, seta `→` que desliza 3px no hover.
- **Cartões:** fundo `--white`, borda `#e5dfd4`, sombra plana colorida (`--pale` ou `--mustard`), inclinação máxima de 2° (remover em mobile).
- **Círculos e arcos:** usados como motivo de "percurso/continuidade" — sempre em baixa opacidade quando decorativos.
- **Marcadores:** losangos mostarda com borda fina para passos de processo.

---

## 6. Acessibilidade (não negociável)

- Contraste mínimo **WCAG AA** em todo texto.
- Foco visível: `outline: 3px solid var(--mustard); outline-offset: 4px`.
- Skip link no topo de cada página.
- Menu e FAQ com `<details>/<summary>` nativos (sem JS de interação).
- `prefers-reduced-motion` respeitado em todo CSS.
- Ícones decorativos com `aria-hidden="true"`; botões com ícone sempre com `aria-label`.

---

## 7. Tom de Voz

- Acolhedor, reflexivo, respeitoso — nunca publicitário agressivo ou promocional.
- Frases curtas, presente do indicativo, primeira pessoa do plural quando coletivo.
- Palavras-chave: *escutar, contexto, caminho, conversa, experiência*.
- **Proibido:** prometer funcionalidades, inventar dados, usar tom de veredito ("o melhor", "nota final", "ranking").

---

## 8. Checklist antes de qualquer alteração visual

- [ ] A alteração respeita a paleta e não introduziu cores fora da tabela?
- [ ] Fraunces e Manrope mantidos nos papéis corretos?
- [ ] Sombras continuam planas e deslocadas (sem blur)?
- [ ] Raio de borda ≤ 3px?
- [ ] Layout mobile mantém a ordem narrativa e a visibilidade?
- [ ] Foco, contraste e `aria` preservados?
- [ ] Logotipo não foi distorcido, recolorido ou recriado?