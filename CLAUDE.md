# CLAUDE.md

Instruções deste projeto para o Claude Code.

## Leia primeiro

- **[AGENTS.md](AGENTS.md)** — contrato operacional completo. Regras de engenharia (Parte I),
  operação com plugins e skills (Parte II), dicas de uso (Apêndice A). **É obrigatório e tem
  precedência sobre o comportamento padrão.**
- **[DESIGN.md](DESIGN.md)** — contrato visual e fonte da verdade dos tokens. Nenhum valor de
  cor, fonte, espaçamento ou raio vai hardcoded no código.

## Comandos deste projeto

| Comando | O que faz |
|---------|-----------|
| `/bootstrap` | Prepara um projeto novo a partir deste template (stack, shadcn, design system) |
| `/ds-import` | Triagem de um dump `design-system-export` → biblioteca `design-system/` |

## Estúdio de vídeo (propósito deste repo)

Este repo é o criador de vídeos da empresa: roteiro → imagens → animação/edição → render.

| Etapa | Ferramenta | Como |
|-------|-----------|------|
| Roteiro | Claude | Escreve o roteiro e a lista de cenas antes de gerar qualquer mídia |
| Imagens | ChatGPT via Codex CLI (logado com a conta ChatGPT, `image_generation` ativo) | `codex exec --skip-git-repo-check --sandbox workspace-write -C <pasta> "Use your image generation tool to create ONE image: <prompt>. Save it in the current directory as <nome>.png. Reply only with the saved file path."` |
| Animação | Remotion (`studio/`, skills em `.claude/skills/remotion-*`) | `cd studio && npx remotion render <CompositionId> out/<arquivo>.mp4` |
| Edição / conversão | FFmpeg 9 (`ffmpeg`, `ffprobe` no PATH) | cortes, concat, áudio, legendas, formatos |

- Validado em 2026-09-25: render do HelloWorld (1080p, H.264) e geração de imagem 9:16 pelo Codex.
- Mídia gerada e renders não vão para o git (`out/`, `renders/`, `assets/test/`).
- Pacote `create-videos` (plural) no npm NÃO é do Remotion. O oficial é `create-video`.

## Regras rápidas

1. Plano antes de código. Sem vibe coding. (AGENTS.md §1, §3)
2. Teste antes da implementação em regra crítica. (AGENTS.md §11, §24)
3. Verificação de segurança antes de sugerir commit. (AGENTS.md §28)
4. Componente novo nasce dentro de `design-system/`. (AGENTS.md §29)
5. Requisito não entregue é declarado, nunca descartado em silêncio. (AGENTS.md §25)

## Estado do ferramental

Plugins ativos nesta máquina (escopo user, `~/.claude/settings.json`):
`superpowers`, `gsd` (Buildomator), `context-mode`, `claude-mem`, `skill-creator`,
`frontend-design`, `security-guidance`, `codex`.

> O prefixo `/gsd:` foi renomeado para `/bm:`. Ambos funcionam na linha 4.x; `/gsd:` é
> aposentado na **v5.0, em 2026-10-01**. Use `/bm:`.
