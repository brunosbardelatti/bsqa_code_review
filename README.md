![GitHub Workflow](https://img.shields.io/github/actions/workflow/status/brunosbardelatti/bsqa_code_review/ai-code-review.yml?label=code%20review%20AI&logo=github&style=flat-square)

# 🤖 BSQA Code Review AI

Este repositório centraliza um GitHub Action reutilizável para **Code Review automatizado com IA** usando prompts hospedados via Gist.  
O objetivo é facilitar o reuso do workflow em múltiplos projetos de forma padronizada e segura.

---

## 📦 Estrutura

```
bsqa_code_review/
├── .github/
│   └── workflows/
│       └── ai-code-review.yml  # Workflow principal (reutilizável)
└── README.md
```

---

## 🚀 Como usar este workflow em outro repositório

1. **Adicione este job no projeto consumidor:**

```yaml
name: BSQA Code Review Reuso

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  call-code-review:
    uses: brunosbardelatti/bsqa_code_review/.github/workflows/ai-code-review.yml@main
    with:
      gist_url: "https://gist.githubusercontent.com/USUARIO/ID_DO_GIST/raw/NOME_DO_ARQUIVO.txt"
    secrets:
      STACKSPOT_CLIENT_ID: ${{ secrets.STACKSPOT_CLIENT_ID }}
      STACKSPOT_CLIENT_SECRET: ${{ secrets.STACKSPOT_CLIENT_SECRET }}
      STACKSPOT_AGENT_ID: ${{ secrets.STACKSPOT_AGENT_ID }}
```

2. **Crie os segredos no repositório consumidor (Settings > Secrets and variables > Actions):**

| Nome                    | Descrição                                                                 |
|-------------------------|---------------------------------------------------------------------------|
| `STACKSPOT_CLIENT_ID`   | Client ID da StackSpot para autenticação da API                           |
| `STACKSPOT_CLIENT_SECRET` | Client Secret correspondente                                             |
| `STACKSPOT_AGENT_ID`    | ID do agente configurado para geração do Code Review                      |

---

## 📄 Prompt

O conteúdo usado para estruturar o prompt da IA é hospedado em um **Gist secreto**.

> ✅ Isso permite centralizar a manutenção do prompt sem acoplar diretamente no repositório.

Para criar o seu:

1. Vá em [gist.github.com](https://gist.github.com)
2. Crie um Gist com o conteúdo do prompt
3. Marque como **Secret**
4. Copie a **Raw URL** do arquivo e use no parâmetro `gist_url`

Exemplo de URL raw:

```
https://gist.githubusercontent.com/brunosbardelatti/f49bcfbabcc3489b4549283cae00cfd7/raw/NOME_DO_ARQUIVO.txt
```

---

## 🧠 O que este workflow faz

- Gera o `diff` entre a branch base e o PR
- Baixa o prompt do Gist
- Prepara o conteúdo para IA
- Autentica na StackSpot
- Envia a análise para o agente configurado
- Publica automaticamente um comentário no Pull Request com o resultado

---

## 📌 Observações

- O repositório `bsqa_code_review` pode ser público, pois **os segredos estão no repositório consumidor**
- O conteúdo do prompt pode ser personalizado por linguagem ou projeto, criando Gists específicos

---

## ✨ Exemplo de resultado

![Exemplo de comentário do bot](https://placehold.co/600x200?text=Code+Review+AI+Comment)

---

## 🛠️ Requisitos

- GitHub Actions habilitado no projeto consumidor
- Autorização de reuso para workflows de terceiros (padrão ativada em repositórios públicos)

---

## 💬 Suporte e Contribuições

Tem ideias para novos templates de prompt, melhorias no fluxo ou sugestões de validação de código? Sua contribuição é muito bem-vinda!  
  
Se quiser colaborar, abra uma [issue](https://github.com/brunosbardelatti/bsqa_code_review/issues) ou envie um pull request com sua proposta.

Para dúvidas, sugestões ou suporte direto, entre em contato pelos canais abaixo ou via e-mail.

---

## 🔒 Licença

Este projeto é mantido por:

**Bruno Sbardelatti**  
BSQA Qualidade de Software LTDA – BSQA Qualidade de Software  
CNPJ: 45.524.623/0001-00  

📧 E-mail pessoal: [bruno.sbardelatti@gmail.com](mailto:bruno.sbardelatti@gmail.com)  
📧 E-mail comercial: [contato.bsqa@gmail.com](mailto:contato.bsqa@gmail.com)  
🔗 LinkedIn: [linkedin.com/in/brunosbardelatti](https://www.linkedin.com/in/brunosbardelatti/)

Este repositório está licenciado sob a [Licença MIT](LICENSE).  
O uso do conteúdo aqui disponibilizado é livre para fins educacionais e profissionais, desde que respeitadas as condições da licença. A BSQA Qualidade de Software não se responsabiliza por eventuais danos decorrentes da má utilização dos workflows ou automações aqui presentes.