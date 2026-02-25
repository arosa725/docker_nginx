# 🚀 Documentação de Deploy - Laboratório Inetz

Este repositório contém as instruções e os modelos de automação (CI/CD) para a publicação de projetos no servidor do Laboratório Inetz.

## 📋 Pré-requisitos (Configuração Obrigatória)

Para que o deploy funcione, o aluno deve configurar os **Secrets** no repositório do GitHub em:  
`Settings > Secrets and variables > Actions > New repository secret`.

| Secret | Descrição |
| :--- | :--- |
| `SSH_HOST` | IP ou Endereço do Servidor Inetz |
| `SSH_KEY` | Chave RSA privada (fornecida pela coordenação) |
| `ALUNO_RA` | Registro Acadêmico do Aluno (apenas números) |
| `ALUNO_PROJETO` | Nome do projeto (ex: `meu-site-vendas`) |

---

## 🛠️ Modelos de Workflows (`.github/workflows/main.yml`)

Escolha o modelo abaixo que corresponde à tecnologia do seu projeto. Crie um arquivo chamado `main.yml` dentro da pasta `.github/workflows/` no seu repositório.

### 1. Front-End Estático (HTML/CSS/JS)
*Utilizado para projetos básicos de desenvolvimento web.*

```yaml
name: Deploy Front-End Estático
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Publicar para Inetz
        uses: appleboy/scp-action@v0.1.7
        with:
          host: ${{ secrets.SSH_HOST }}
          username: ubuntu
          key: ${{ secrets.SSH_KEY }}
          source: "."
          target: "/var/inetpub/wwwroot/projetos/${{ secrets.ALUNO_RA }}/front/${{ secrets.ALUNO_PROJETO }}"
```
2. Front-End Moderno (React, Vue, Vite)
Utilizado para projetos que exigem processo de build.



```YAML

name: Deploy Framework JS
on:
  push:
    branches: [ main ]
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Instalar Node e Build
        run: |
          npm install
          npm run build
      - name: Publicar para Inetz
        uses: appleboy/scp-action@v0.1.7
        with:
          host: ${{ secrets.SSH_HOST }}
          username: ubuntu
          key: ${{ secrets.SSH_KEY }}
          source: "dist/*" 
          target: "/var/inetpub/wwwroot/projetos/${{ secrets.ALUNO_RA }}/front/${{ secrets.ALUNO_PROJETO }}"
          strip_components: 1
```

3. Back-End (Node.js / Express)
Utilizado para APIs e desenvolvimento servidor.

```YAML

name: Deploy Back-End Node
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Publicar para Inetz
        uses: appleboy/scp-action@v0.1.7
        with:
          host: ${{ secrets.SSH_HOST }}
          username: ubuntu
          key: ${{ secrets.SSH_KEY }}
          source: "."
          target: "/var/inetpub/wwwroot/projetos/${{ secrets.ALUNO_RA }}/back/${{ secrets.ALUNO_PROJETO }}"
```
4. Inteligência Artificial (Python / Prompts)
Utilizado para scripts, modelos e notebooks.

```YAML

name: Deploy IA Python
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Publicar para Inetz
        uses: appleboy/scp-action@v0.1.7
        with:
          host: ${{ secrets.SSH_HOST }}
          username: ubuntu
          key: ${{ secrets.SSH_KEY }}
          source: "."
          target: "/var/inetpub/wwwroot/projetos/${{ secrets.ALUNO_RA }}/ia/${{ secrets.ALUNO_PROJETO }}"

```

🌍 Acesso ao Projeto
Após o deploy ser concluído (ícone verde na aba Actions), o projeto estará disponível na seguinte URL:

https://lab.inetz.com.br/[SEU-RA]/[CATEGORIA]/[NOME-DO-PROJETO]

Categorias aceitas: front, back, ia.
