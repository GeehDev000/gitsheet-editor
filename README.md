# GitSheet Editor

Editor web de planilhas XLSX integrado ao GitHub. O projeto inclui uma interface React/Vite e um Cloudflare Worker para autenticação OAuth e operações GitHub executadas no servidor.

## Recursos
- Login OAuth com GitHub; sessão em cookie HttpOnly, Secure e SameSite=Lax.
- Sessões e estados OAuth armazenados em Cloudflare KV.
- Listagem de repositórios públicos e privados que a conta autenticada pode acessar.
- Localização de arquivos XLSX, download binário e atualização com commit no GitHub.
- Abertura local, edição básica de células e alternância entre abas.

## Requisitos
- Node.js 20+, npm
- Conta Cloudflare com Workers e KV
- GitHub OAuth App

## Desenvolvimento local
1. Execute npm install.
2. Copie .dev.vars.example para .dev.vars e preencha as credenciais. Não versione .dev.vars.
3. Crie um namespace KV e informe seu ID em wrangler.toml.
4. Configure o callback OAuth da GitHub OAuth App para http://localhost:8787/auth/callback.
5. Execute npm run build e npx wrangler dev.

## Configurar OAuth
Crie uma OAuth App em GitHub Developer Settings. Configure a Homepage URL e Authorization callback URL para o domínio do Worker, por exemplo https://SEU-WORKER.workers.dev/auth/callback.

Defina os segredos no Worker:
```bash
npx wrangler secret put GITHUB_CLIENT_ID
npx wrangler secret put GITHUB_CLIENT_SECRET
```
Associe o namespace KV ao binding SESSIONS em wrangler.toml. O token OAuth permanece no Worker; nunca use VITE_* para segredos.

## Implantação
1. Atualize o ID do KV em wrangler.toml.
2. Configure os secrets do Worker.
3. Execute npm run build.
4. Execute npx wrangler deploy.
5. Configure o callback OAuth para o domínio final.

## Endpoints
- GET /auth/login e GET /auth/callback: fluxo OAuth.
- POST /auth/logout: encerra sessão.
- GET /api/me: usuário atual.
- GET /api/repos: repositórios acessíveis.
- GET /api/repos/:owner/:repo/tree?ref=main: arquivos XLSX.
- GET /api/file?owner=...&repo=...&path=...&ref=main: baixa XLSX binário e retorna SHA no header X-GitHub-Content-Sha.
- PUT /api/file: atualiza arquivo e cria commit, exigindo SHA atual para evitar sobrescrita cega.

## Segurança e limitações
- O frontend não recebe nem armazena o token OAuth.
- Cookies de sessão são HttpOnly, Secure e SameSite=Lax.
- O parâmetro state do OAuth é aleatório, temporário e validado no callback.
- Escritas verificam a origem e exigem o SHA atual.
- O escopo OAuth usado nesta versão é repo read:user. Para distribuir publicamente, prefira migrar para GitHub App com permissões de instalação granulares.
- A preservação de macros, gráficos, tabelas dinâmicas, imagens e recursos avançados do Excel ainda não é garantida. Teste cópias antes de editar arquivos críticos.

## Versões
- v0.1.0: editor inicial e acesso direto à API GitHub (protótipo local).
- v0.2.0: Worker OAuth, sessões seguras e proxy GitHub server-side.
