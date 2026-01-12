# Publicação Inicial do Projeto no GitHub (Windows)

Este guia explica como publicar este projeto como se fosse a primeira vez, começando de um diretório local limpo (sem Git) e enviando para o repositório remoto.

- Repositório remoto (SSH): git@github.com:vilsonvitalino/SuperExplorer.git
- Usuário GitHub: vilsonvitalino
- Pasta local do projeto: d:\projetos\explorer

> Dica: Execute todos os comandos em PowerShell, dentro da pasta do projeto.

---

## 1. Pré‑requisitos
- Git instalado (https://git-scm.com/downloads)
- Conta GitHub acessível (https://github.com)

Opcional (recomendado para SSH):
- OpenSSH Client (normalmente já presente no Windows 10/11)

Verifique versões rapidamente:

```powershell
git --version
ssh -V
```

---

## 2. Escolha o método de autenticação
Você pode publicar via SSH (recomendado) ou via HTTPS. Este guia prioriza SSH. Se preferir HTTPS, veja a seção 7.

---

## 3. Configurar SSH (uma vez por máquina)
Se você ainda não tem uma chave SSH cadastrada no GitHub:

1) Gerar chave ED25519
```powershell

ssh-keygen -t ed25519 -C "vilsonvitalino@gmail.com" -f "$env:USERPROFILE\.ssh\id_ed25519"

Your identification has been saved in C:\Users\Vilson\.ssh\id_ed25519
Your public key has been saved in C:\Users\Vilson\.ssh\id_ed25519.pub
The key fingerprint is:
SHA256:v47XmzQfbcFPN1XK+YcSknLLYLOI6EYafoI+E58K2J8 vilsonvitalino@gmail.com


ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHJcSZkY3RNy2AcP7BuYerv9VM0yv14AaL6xEOnMHT9p vilsonvitalino@gmail.com


```

2) Copiar a chave pública para a área de transferência e cadastrar no GitHub
```powershell
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub | Set-Clipboard
```
- Vá em GitHub → Settings → SSH and GPG keys → New SSH key
- Title: "Máquina Windows" (ou o nome que preferir)
- Cole a chave e salve

3) Testar o acesso SSH ao GitHub
```powershell
ssh -T git@github.com
```
Você deve ver uma mensagem de "Hi <seu-usuario>! ...". Se aparecer "Permission denied (publickey)", revise o cadastro da chave.

---

## 4. Inicializar o repositório local
Entre na pasta do projeto e inicialize o Git com a branch principal `main`:

```powershell
cd d:\projetos\explorer
git init -b main
```

---

## 5. Criar .gitignore (recomendado)
Crie um arquivo `.gitignore` para não versionar arquivos temporários/locais. Exemplo mínimo para Python/Windows:

```gitignore
build/
dist/
__pycache__/
*.pyc
.venv/
*.spec
.local/
.DS_Store
```

Crie o arquivo no editor ou via PowerShell:
```powershell
@"
build/
dist/
__pycache__/
*.pyc
*.md
*.log
.venv/
*.spec
.local/
.DS_Store
"@ | Out-File -Encoding UTF8 .gitignore
```

---

## 6. Primeiro commit (local)

```powershell
git add .
git commit -m "Initial commit"
```

---

## 7. Adicionar remoto e fazer push (SSH recomendado)

```powershell
git remote add origin git@github.com:vilsonvitalino/SuperExplorer.git
git remote -v
```

- Se o repositório do GitHub estiver vazio (sem README/.gitignore/LICENSE), faça o push direto:
```powershell
git push -u origin main
```

- Se o repositório do GitHub JÁ tiver arquivos (ex.: README, LICENSE, .gitignore):
```powershell
git fetch origin
# Rebasa seu commit por cima do remoto
git pull --rebase origin main

# Se aparecerem conflitos em .gitignore, README.md, LICENSE e você quiser manter os do GitHub:
git checkout --theirs .gitignore README.md LICENSE
git add .gitignore README.md LICENSE

git rebase --continue

git push -u origin main
```

> Observação: Se preferir simplesmente começar do estado do remoto e commitar tudo depois, use:
```powershell
git fetch origin
git reset --hard origin/main
git add .
# Para manter os arquivos do GitHub antes de commitar o restante:
git restore --staged .gitignore README.md LICENSE
git checkout -- .gitignore README.md LICENSE

git commit -m "Initial project files"
git push -u origin main
```

---

## 8. Alternativa via HTTPS (sem SSH)
1) Configure o remoto HTTPS:
```powershell
git remote add origin https://github.com/vilsonvitalino/SuperExplorer.git
```
2) Push inicial:
```powershell
git push -u origin main
```
Na primeira vez, o Git pedirá credenciais:
- Username: vilsonvitalino
- Password: um Personal Access Token (PAT) com escopo "repo"
Crie em: GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)

---

## 9. Comandos úteis
- Ver remotos:
```powershell
git remote -v
```
- Ver estado:
```powershell
git status
```
- Trocar URL do remoto (SSH ↔ HTTPS):
```powershell
git remote set-url origin <nova-url>
```
- Abortar rebase em andamento:
```powershell
git rebase --abort
```
- Restaurar ao estado do remoto (cuidado: descarta mudanças locais não commitadas):
```powershell
git fetch origin
git reset --hard origin/main
```

---

## 10. Checklist rápido
- [ ] `.git` existe (git init feito)
- [ ] `.gitignore` criado (evita subir `.venv/` e artefatos)
- [ ] Commit inicial criado
- [ ] Remoto `origin` aponta para `git@github.com:vilsonvitalino/SuperExplorer.git`
- [ ] `git push -u origin main` concluído sem erros

Pronto! Após isso, o repositório no GitHub refletirá seu código local.
