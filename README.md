# GitSheet Editor

Editor web de planilhas XLSX integrado ao GitHub. A versão inicial permite abrir arquivos locais ou de repositórios públicos/privados, editar valores de células, alternar entre abas, baixar o XLSX e salvar alterações no repositório.

## Requisitos
- Node.js 20+
- npm

## Instalação e execução
```bash
npm install
npm run dev
```
Abra o endereço local indicado pelo Vite.

## Acesso ao GitHub na versão 0.1
- Arquivos públicos podem ser lidos sem token.
- Repositórios privados exigem um fine-grained personal access token.
- Para salvar, o token precisa de permissão Contents: Read and write no repositório.
- O token fica apenas em memória enquanto a página está aberta.

**Atenção:** esta versão 0.1 é um protótipo local. Ela envia o token do navegador diretamente à API do GitHub. Não publique esta versão como serviço público nem use um token amplo. A etapa seguinte substitui esse fluxo por autenticação GitHub OAuth via backend.

## Limitações
- Grade básica limitada a 200 linhas e 40 colunas por aba.
- Sem recálculo de fórmulas.
- A preservação de gráficos, macros, tabelas dinâmicas, imagens e recursos avançados não é garantida.
- O suporte é voltado a .xlsx; .xlsm não é suportado.
- Arquivos grandes podem exceder os limites da API de Contents do GitHub.

## Segurança
Nunca coloque tokens pessoais, client secrets ou chaves privadas no código frontend, em arquivos versionados ou em variáveis Vite expostas ao navegador.