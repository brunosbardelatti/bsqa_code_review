[![Code Review AI](https://github.com/brunosbardelatti/bsqa_code_review/actions/workflows/ai-code-review.yml/badge.svg)](https://github.com/brunosbardelatti/bsqa_code_review/actions/workflows/ai-code-review.yml)

# 🤖 BSQA Code Review AI

Este repositório centraliza um GitHub Action reutilizável para **Code Review automatizado com IA**, utilizando prompts hospedados via Gist e execução pela plataforma StackSpot.  
O objetivo é padronizar e facilitar revisões automáticas de código em múltiplos projetos, com agilidade, segurança e rastreabilidade.

---

## 📦 Estrutura

```
bsqa_code_review/
├── .github/
│   ├── templates/
│   │   └── comment_template_code-review.md  # Template do comentário final
│   └── workflows/
│       └── ai-code-review.yml              # Workflow principal reutilizável
└── README.md
```

---

## 🚀 Como usar este workflow em outro repositório

### 1. Adicione o job no repositório consumidor:

```yaml
name: Code Review powered by BSQA AI

on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: read
  pull-requests: write

jobs:
  code-review:
    uses: brunosbardelatti/bsqa_code_review/.github/workflows/ai-code-review.yml@main
    with:
      gist_url: "https://gist.githubusercontent.com/USUARIO/ID_DO_GIST/raw/NOME_DO_ARQUIVO.txt"
    secrets:
      STACKSPOT_CLIENT_ID: ${{ secrets.STACKSPOT_CLIENT_ID }}
      STACKSPOT_CLIENT_SECRET: ${{ secrets.STACKSPOT_CLIENT_SECRET }}
      STACKSPOT_AGENT_ID: ${{ secrets.STACKSPOT_AGENT_ID }}
```

### 2. Crie os segredos no repositório consumidor:

Acesse: `Settings > Secrets and variables > Actions`

| Nome                      | Descrição                                              |
|---------------------------|--------------------------------------------------------|
| `STACKSPOT_CLIENT_ID`     | Client ID fornecido pela StackSpot                    |
| `STACKSPOT_CLIENT_SECRET` | Client Secret associado                               |
| `STACKSPOT_AGENT_ID`      | ID do agente configurado para análises de código      |

---

## 📝 Template de Comentário no Projeto Consumidor

Para que o comentário da IA seja publicado automaticamente no PR, é necessário que o projeto consumidor contenha o seguinte arquivo:

```
.github/workflows/codeReview/comment_template_code-review.md
```

> O caminho e o nome devem coincidir com o esperado pelo script no repositório principal.

Este arquivo deve conter os seguintes placeholders, que serão substituídos dinamicamente:

- `{{PR_NUMBER}}`
- `{{PR_TITLE}}`
- `{{HEAD}}`
- `{{BASE}}`
- `{{STATUS}}` (`APROVADO`, `REPROVADO`, `INDETERMINADO`)
- `{{REVIEW_CONTENT}}`
- `{{DATE}}`

**Exemplo de template:**

```
🤖 Code Review Automática - BSQA  
PR: #{{PR_NUMBER}}  
Título: {{PR_TITLE}}  
Branch: {{HEAD}} → {{BASE}}  
Status Code Review: {{STATUS}}

📋 Resultado da análise  
{{REVIEW_CONTENT}}

Gerado em {{DATE}} (Horário de Brasília)
```

---

## 📄 Prompt via Gist

O conteúdo que estrutura o prompt da IA é hospedado em um **Gist secreto**, permitindo centralizar e atualizar o texto sem acoplar diretamente ao repositório.

### Como criar o seu Gist:

1. Acesse [gist.github.com](https://gist.github.com)
2. Crie um novo Gist com o conteúdo do prompt desejado
3. Marque como **Secret**
4. Copie a **Raw URL** e utilize no parâmetro `gist_url`

Exemplo:

```
https://gist.githubusercontent.com/seuusuario/hashdoarquivo/raw/NOME_DO_ARQUIVO.txt
```

---

## 🧠 O que este workflow faz

- Gera o `git diff` da branch do PR em relação à base
- Baixa o prompt do Gist
- Concatena prompt e diff para envio
- Autentica na StackSpot
- Envia a requisição para o agente de IA configurado
- Publica o comentário com o resultado no Pull Request
- Classifica automaticamente como `APROVADO`, `REPROVADO` ou `INDETERMINADO`

---

## ✨ Exemplo de resultado

![Exemplo de comentário do bot](https://placehold.co/600x200?text=Code+Review+AI+Comment)

---

## 🛠️ Requisitos

- GitHub Actions habilitado no projeto consumidor
- Permissão para reuso de workflows de terceiros (ativado por padrão em repositórios públicos)
- Agente configurado e funcional na StackSpot

---

## 💬 Suporte e Contribuições

Tem sugestões de novos prompts, melhorias no fluxo ou novas validações de código?  
Colabore abrindo uma [issue](https://github.com/brunosbardelatti/bsqa_code_review/issues) ou enviando um pull request.  
Seu feedback é muito bem-vindo!

---

## 🔒 Licença

Este projeto é mantido por:

**Bruno Sbardelatti**  
BSQA Qualidade de Software LTDA  
CNPJ: 45.524.623/0001-00  

📧 E-mail pessoal: [bruno.sbardelatti@gmail.com](mailto:bruno.sbardelatti@gmail.com)  
📧 E-mail comercial: [contato.bsqa@gmail.com](mailto:contato.bsqa@gmail.com)  
🔗 LinkedIn: [linkedin.com/in/brunosbardelatti](https://www.linkedin.com/in/brunosbardelatti/)

Este repositório está licenciado sob a [Licença MIT](LICENSE).  
O uso do conteúdo é livre para fins educacionais e profissionais, desde que respeitadas as condições da licença.  
A BSQA não se responsabiliza por eventuais danos decorrentes da má utilização dos workflows aqui presentes.