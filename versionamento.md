# Guia de Versionamento e Releases

Este documento define como versionar e publicar novas versões do projeto SuperExplorer.

- Repositório: git@github.com:vilsonvitalino/SuperExplorer.git
- Branch principal: `main`
- Padrão de versão: SemVer (MAJOR.MINOR.PATCH)

## Objetivos
- Ter um processo simples e repetível para releases.
- Padronizar mensagens de commit e tags.
- Facilitar geração de changelog e anexos de release (binários em `dist/`).

## Padrão de Versão (SemVer)
- `MAJOR`: quebra de compatibilidade externa.
- `MINOR`: novas funcionalidades compatíveis.
- `PATCH`: correções e melhorias internas sem quebrar compatibilidade.

Exemplos: `v1.0.0`, `v1.1.0`, `v1.1.1`.

## Convenção de Commits (recomendado)
Use Conventional Commits para facilitar changelog e entendimento do histórico:
- `feat:` nova funcionalidade
- `fix:` correção de bug
- `docs:` documentação
- `refactor:` refatoração (sem mudança de comportamento)
- `perf:` desempenho
- `test:` testes
- `chore:` tarefas gerais (build, deps, etc.)

Opcional: acrescente escopo, exemplo: `feat(ui): atalho para copiar caminho`.

## Fluxo de Branches
- `main`: sempre estável; releases saem daqui.
- `feature/<nome>`: desenvolvimento de novas features.
- `fix/<nome>`: correções.
- Mescle por PR, squash opcional para manter histórico limpo.

## Processo de Release (simples)
1) Garanta `main` atualizado localmente
```powershell
git checkout main
git pull origin main
```

2) Defina a versão alvo e crie a tag anotada
```powershell
# Substitua X.Y.Z pela versão (ex.: 1.0.0)
git tag -a vX.Y.Z -m "Release vX.Y.Z"
```

3) Publique a tag no remoto
```powershell
git push origin --tags
```

4) Crie a Release no GitHub
- Acesse o repositório → Releases → "Draft a new release".
- Escolha a tag `vX.Y.Z`.
- Preencha notas de release (veja template abaixo).
- Anexe artefatos (ex.: executáveis/instaladores de `dist/`).

5) (Opcional) Atualize CHANGELOG
- Você pode gerar base com:
```powershell
git log --pretty=format:"* %s (%h)" vA.B.C..vX.Y.Z
```
- Onde `vA.B.C` é a tag anterior.

## Hotfix
- Para correção urgente:
```powershell
git checkout -b fix/hotfix-descricao main
# faça o fix, commits
git checkout main
git merge --no-ff fix/hotfix-descricao
# versão PATCH
git tag -a vX.Y.Z -m "Hotfix: ..."
git push origin main --tags
```

## Comandos Úteis
- Ver última tag:
```powershell
git describe --tags --abbrev=0
```
- Diferenças desde a última tag:
```powershell
$last = git describe --tags --abbrev=0
git log --oneline "$last"..HEAD
```
- Subir apenas a tag mais recente:
```powershell
git push origin vX.Y.Z
```

## Template de Release Notes
Título: `vX.Y.Z` – resumo curto

Mudanças
- Feat: …
- Fix: …
- Docs: …
- Refactor/Perf/Chore: …

Compatibilidade
- Quebra? (sim/não). Se sim, detalhe a migração.

Artefatos
- Explorer: `dist/Explorer.exe`
- ExplorerSSH: `dist/ExplorerSSH.exe`

Notas
- Dependências atualizadas (se houver)
- Observações de instalação/upgrade

## Automação (futuro opcional)
- `gh release` para criar releases via CLI.
- GitHub Actions para build automático (gerar binários e anexar na release).

---
Manter este arquivo versionado para consulta rápida do time.