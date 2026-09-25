# DESIGN.md — Contrato Visual

> **Fonte da verdade dos tokens deste projeto.** Versionado. Se um valor visual não está aqui,
> ele não existe no código. Ver `AGENTS.md` §29.
>
> **Status:** `TEMPLATE` — preencher ao iniciar o projeto. Trocar para `ATIVO` depois.

---

## 0. Como usar este arquivo

1. Ao iniciar o projeto, preencha as seções marcadas com `<!-- PREENCHER -->`.
2. Toda decisão visual nova entra aqui **antes** de entrar no código.
3. Mudou um token aqui → propaga para `design-system/tokens/` → propaga para os componentes.
   Nunca o caminho inverso.
4. Se você se pegou escrevendo `#3B82F6` ou `padding: 13px` num componente, pare: ou o token
   existe e você não usou, ou ele não existe e precisa ser criado aqui primeiro.

---

## 1. Identidade

<!-- PREENCHER -->

| Campo | Valor |
|-------|-------|
| Produto | _nome_ |
| Categoria | _SaaS B2B / B2C / interno_ |
| Personalidade visual | _ex: técnico e denso / calmo e espaçado / editorial_ |
| Referências | _2–3 produtos cuja linguagem visual serve de norte_ |
| Anti-referências | _o que explicitamente não queremos parecer_ |

**Princípio norteador:** <!-- uma frase. ex: "densidade de informação sem ruído visual" -->

---

## 2. Cor

Tokens semânticos, nunca literais. `--color-danger`, não `--color-red`.

### 2.1 Escala base

<!-- PREENCHER — substituir pelos valores reais do projeto -->

```css
:root {
  /* Superfícies */
  --surface-base:      #FFFFFF;
  --surface-raised:    #FAFAFA;
  --surface-overlay:   #FFFFFF;
  --surface-sunken:    #F4F4F5;

  /* Texto */
  --text-primary:      #18181B;
  --text-secondary:    #52525B;
  --text-tertiary:     #A1A1AA;
  --text-on-accent:    #FFFFFF;

  /* Bordas */
  --border-subtle:     #E4E4E7;
  --border-default:    #D4D4D8;
  --border-strong:     #A1A1AA;

  /* Ação */
  --accent-default:    #2563EB;
  --accent-hover:      #1D4ED8;
  --accent-active:     #1E40AF;
  --accent-subtle:     #EFF6FF;

  /* Estados semânticos */
  --success-default:   #16A34A;
  --success-subtle:    #F0FDF4;
  --warning-default:   #D97706;
  --warning-subtle:    #FFFBEB;
  --danger-default:    #DC2626;
  --danger-subtle:     #FEF2F2;
}
```

### 2.2 Tema escuro

Redefinir os mesmos tokens — **nunca** criar tokens paralelos com sufixo `-dark`.

```css
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) { /* … redefinição … */ }
}
:root[data-theme="dark"] { /* … mesma redefinição … */ }
```

### 2.3 Regras de cor

- Contraste mínimo **4.5:1** para texto normal, **3:1** para texto grande e elementos de UI
  (WCAG 2.2 AA). Validar antes de aprovar qualquer par.
- Cor nunca é o único portador de significado. Erro tem ícone e texto, não só vermelho.
- Paleta de gráficos vive em `design-system/tokens/chart.css`, separada da paleta de UI.
  Ver skill `dataviz`.

---

## 3. Tipografia

<!-- PREENCHER -->

| Token | Família | Peso | Tamanho | Line-height | Uso |
|-------|---------|------|---------|-------------|-----|
| `--text-display` | — | 700 | 2.25rem | 1.15 | Título de página |
| `--text-h1` | — | 600 | 1.75rem | 1.2 | Seção |
| `--text-h2` | — | 600 | 1.25rem | 1.3 | Subseção |
| `--text-body` | — | 400 | 0.9375rem | 1.6 | Corpo |
| `--text-small` | — | 400 | 0.8125rem | 1.5 | Auxiliar, legenda |
| `--text-mono` | — | 400 | 0.875rem | 1.5 | Código, IDs, valores |

**Carregamento das fontes:** <!-- PREENCHER: next/font, @font-face local, Google Fonts -->

Regras:
- Máximo **2 famílias** (+1 mono). Mais que isso é ruído.
- Números em tabelas e métricas usam `font-variant-numeric: tabular-nums`.
- Nenhum tamanho fora da escala. Precisa de um novo? Adicione à tabela primeiro.

---

## 4. Espaçamento e Layout

Escala base de **4px**. Todo espaçamento é múltiplo dela.

```css
--space-1: 0.25rem;  /*  4px */
--space-2: 0.5rem;   /*  8px */
--space-3: 0.75rem;  /* 12px */
--space-4: 1rem;     /* 16px */
--space-6: 1.5rem;   /* 24px */
--space-8: 2rem;     /* 32px */
--space-12: 3rem;    /* 48px */
--space-16: 4rem;    /* 64px */
```

| Contexto | Token |
|----------|-------|
| Dentro de um componente | `--space-2` a `--space-3` |
| Entre componentes relacionados | `--space-4` |
| Entre blocos de conteúdo | `--space-6` a `--space-8` |
| Entre seções de página | `--space-12` a `--space-16` |

**Breakpoints:** <!-- PREENCHER -->

Regra: `padding: 13px` não existe. Se um valor fora da escala parece necessário, o problema é
outro (alinhamento óptico, border-box, altura de linha) — investigue a causa.

---

## 5. Raio, Sombra e Elevação

```css
--radius-sm: 0.25rem;   /* badge, tag */
--radius-md: 0.5rem;    /* input, botão, card */
--radius-lg: 0.75rem;   /* modal, painel */
--radius-full: 9999px;  /* avatar, pill */

--shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
--shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.08);
--shadow-lg: 0 10px 25px -5px rgb(0 0 0 / 0.12);
```

**Estratégia de elevação:** <!-- PREENCHER: sombra, borda, ou superfície? Escolher UMA. -->

No tema escuro, elevação se expressa por **superfície mais clara**, não por sombra mais forte.

---

## 6. Motion

```css
--duration-fast:   120ms;  /* hover, feedback imediato */
--duration-base:   200ms;  /* transição de estado */
--duration-slow:   320ms;  /* entrada/saída de overlay */
--ease-out:        cubic-bezier(0.16, 1, 0.3, 1);
--ease-in-out:     cubic-bezier(0.65, 0, 0.35, 1);
```

Regras:
- Anima-se `transform` e `opacity`. Não se anima `width`, `height`, `top`, `left`.
- **`prefers-reduced-motion: reduce` é obrigatório** em todo componente animado.
- Movimento comunica causa e efeito. Se não comunica nada, remova.

---

## 7. Estados Obrigatórios

Todo componente interativo entrega **os nove**, sem exceção:

| Estado | Obrigação |
|--------|-----------|
| `default` | — |
| `hover` | Só em ponteiro fino (`@media (hover: hover)`) |
| `active` | Feedback imediato ao toque/clique |
| `focus` | **`focus-visible` com anel visível.** Nunca `outline: none` sem substituto |
| `disabled` | `aria-disabled`, contraste reduzido mas ainda legível |
| `loading` | Sem salto de layout — reserve o espaço |
| `error` | Ícone + texto, associado via `aria-describedby` |
| `empty` | Explica o que é e qual a próxima ação |
| `selected` | Distinguível de `hover` e de `focus` |

---

## 8. Acessibilidade — Piso Inegociável

- WCAG 2.2 nível AA.
- Navegação completa por teclado, ordem de foco lógica, foco nunca preso.
- Alvo de toque mínimo 44×44px.
- Todo input tem `<label>` associado — `placeholder` não é label.
- Landmarks semânticos (`<nav>`, `<main>`, `<aside>`), um `<h1>` por página.
- Conteúdo dinâmico anunciado via `aria-live` quando relevante.

Auditoria: skill `accessibility`.

---

## 9. Mapa da Biblioteca

```
design-system/
├── tokens/        # a materialização em código do que está neste arquivo
├── components/    # componentes tipados, todas as variantes e os 9 estados
├── patterns/      # composições e templates de tela
├── assets/        # ícones, imagens, fontes
├── guidelines/    # regras de uso, do/don't
└── reference/     # HTMLs de referência visual, abrem sozinhos no navegador
```

**Showcase:** rota `/design-system` renderiza a biblioteca inteira, todos os estados, ambos os
temas. Se um componente não aparece lá, ele não está pronto.

### Como criar um componente novo

1. O token de que ele precisa já existe neste arquivo? Se não, **crie aqui primeiro**.
2. Existe componente equivalente na biblioteca? Se sim, **estenda — não duplique**.
3. Implemente em `design-system/components/`, tipado, com os 9 estados.
4. Registre no showcase `/design-system`.
5. Documente variantes e notas de acessibilidade em `guidelines/`.
6. `/code-review` antes do commit.

---

## 10. Registro de Decisões

Decisão visual não-óbvia se registra aqui, com o porquê. Evita re-litigar em sessão futura.

| Data | Decisão | Motivo |
|------|---------|--------|
| | | |
