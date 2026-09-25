---
description: Prepara um projeto novo duplicado deste template — verifica ferramental, define a stack, instala shadcn/ui e transitions.dev, e materializa o design system a partir do DESIGN.md.
---

Este projeto foi duplicado do TEMPLATE PADRÃO. Sua tarefa é transformá-lo num projeto real,
pronto para desenvolvimento, com o design system já montado e a documentação coerente.

Leia `AGENTS.md` e `DESIGN.md` antes de começar. Eles são o contrato — você está implementando
o que eles descrevem, não reinventando.

Trabalhe de ponta a ponta. Pare apenas nos dois pontos de decisão marcados como **[PERGUNTAR]**
— aí a resposta do humano muda materialmente o resultado e não há default seguro.

---

## 1. Verificação do ferramental

Confirme que os plugins do fluxo estão instalados e habilitados. Leia
`~/.claude/settings.json` (chave `enabledPlugins`) e `~/.claude/plugins/installed_plugins.json`.

Esperado: `superpowers`, `gsd`, `context-mode`, `claude-mem`, `skill-creator`,
`frontend-design`, `security-guidance`.

Reporte o que faltar com o comando de instalação correspondente. **Não tente instalar via
Bash** — `/plugin install` é um comando interativo do Claude Code e só roda numa sessão
interativa.

Verifique também o **runtime Bun** (`bun --version`): o worker do Claude-Mem usa `bun:sqlite` e
não sobe sem ele. Ausente, a memória entre sessões não funciona — reporte, com
`powershell -c "irm bun.sh/install.ps1 | iex"` como correção.

## 2. Definição da stack

**[PERGUNTAR]** Qual o framework e o runtime do projeto?

Apresente as opções com trade-offs curtos (Next.js App Router / Vite + React / Remix / outro) e
aguarde. Sem isso, nada do resto se ancora — o `-t` do shadcn, o carregamento de fontes e a rota
de showcase dependem da resposta.

Depois de definida, inicialize o projeto na stack escolhida e **confirme as versões realmente
instaladas** (não as que você espera). Consulte a documentação atual das ferramentas; as
convenções mudam entre versões.

## 3. Base de UI

```bash
npx shadcn@latest init -t [framework]
```

Substitua `[framework]` pelo definido no passo 2. Responda o wizard de acordo com o
`DESIGN.md` — a escolha de cor base, de variável CSS e de caminho de componente já está decidida
lá, não invente na hora.

Skill de motion (rodar na raiz do projeto):

```bash
npx skills add Jakubantalik/transitions.dev
```

Se o comando falhar por ausência do CLI `skills`, registre isso no relatório e siga — não é
bloqueante, a skill é complementar.

## 4. Preenchimento do DESIGN.md

**[PERGUNTAR]** Qual a identidade visual? Peça, de uma vez só:

- personalidade visual em uma frase;
- 2–3 referências e 1–2 anti-referências;
- cor de ação principal (ou "escolha você" — aí proponha 3 opções com justificativa);
- famílias tipográficas (ou "escolha você").

Com as respostas, preencha **todas** as seções `<!-- PREENCHER -->` do `DESIGN.md`:
identidade, escala de cor (clara e escura), tipografia, breakpoints, estratégia de elevação.

Valide cada par de cores contra **WCAG 2.2 AA** (4.5:1 texto normal, 3:1 texto grande e UI)
antes de gravar. Par que não passa não entra.

Troque o `Status:` do arquivo de `TEMPLATE` para `ATIVO`.

## 5. Materialização do design system

Crie a estrutura e traduza os tokens do `DESIGN.md` para código:

```
design-system/
├── tokens/        # CSS variables — espelho fiel do DESIGN.md
├── components/    # componentes tipados, todas as variantes e os 9 estados
├── patterns/      # composições e templates de tela
├── assets/        # ícones, imagens, fontes
├── guidelines/    # regras de uso, do/don't
└── reference/     # HTMLs de referência visual
```

Regras ao montar:

- Os tokens em `tokens/` são o **espelho fiel** do `DESIGN.md`. Divergiu, é bug.
- Tema escuro redefine os mesmos tokens. Nunca crie tokens paralelos com sufixo `-dark`.
- Todo componente interativo entrega os nove estados: `default`, `hover`, `active`, `focus`,
  `disabled`, `loading`, `erro`, `vazio`, `selecionado`.
- `focus-visible` com anel visível, sempre. `prefers-reduced-motion` respeitado, sempre.

Crie a rota de showcase `/design-system` renderizando a biblioteca inteira, todos os estados,
ambos os temas.

> Se existir um dump `design-system-export/` na raiz, **pare aqui e use `/ds-import`** em vez de
> montar do zero — aquele comando tria o dump e faz a incorporação completa.

## 6. Documentação e higiene

- Atualize o `README.md` com: o que é o projeto, como rodar, onde fica o design system, e o
  link para `/design-system`.
- Confirme que `CLAUDE.md` aponta para `AGENTS.md` e `DESIGN.md`.
- `.gitignore` cobrindo `.env`, `.tmp/`, `node_modules/`, `credentials.json`, `token.json`.
- Crie `.env.example` com as chaves necessárias e **nenhum valor real**.
- Remova do template o que não serve a este projeto (ex: `skills/` que não se aplicam),
  registrando o que foi removido.

## 7. Validação

Rode `build`, `typecheck` e `lint`. Abra `/design-system` e confirme que renderiza nos dois
temas. Conserte o que falhar e rode de novo até passar.

Rode `/code-review` no resultado antes de sugerir o primeiro commit.

## 8. Relatório

- stack e versões realmente instaladas;
- o que foi criado, o que foi removido do template;
- tokens definidos e quais pares de cor foram ajustados por contraste;
- componentes no showcase;
- o que ficou pendente e por quê.
