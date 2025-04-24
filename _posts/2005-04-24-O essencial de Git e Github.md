# Entendendo o essencial de Git e GitHub

## Introdução

Se você trabalha com desenvolvimento de software, é essencial entender **Git** e **GitHub**. Eles são ferramentas poderosas para controle de versão e colaboração em projetos. Neste post, vou abordar os principais conceitos e comandos que você precisa conhecer para começar com Git e GitHub.

---

## O que é esse tal de Git?

**Git** é um sistema de controle de versão distribuído. Ele permite que você registre mudanças no seu código, volte a versões anteriores e trabalhe em diferentes funcionalidades de forma paralela usando _branches_ (ramificações).

### Conceitos Básicos

- **Repositório (repo)**: Diretório onde o Git armazena todos os arquivos e o histórico de mudanças. Pense nisso como uma pasta sua na internet, no github.
- **Commit**: Registro de uma alteração no projeto. Útil para rastrear mudanças e reverter caso necessário.
- **Branch**: Uma linha de desenvolvimento paralela. Você pode criar branches para trabalhar do seu computador, sem afetar a branch main, a principal do projeto.
- **Merge**: Junta alterações de uma branch a outra. Permite mescla a sua branch com a branch main.
- **Clone**: Cópia local de um repositório remoto. 
- **Stage (index)**: Área onde você prepara as mudanças antes de fazer o commit.

---

## Principais comandos do Git:

**Inicializa um repositório Git**
git init

**Clona um repositório remoto**
git clone https://github.com/usuario/repositorio.git

**Verifica o status das alterações**
git status

**Adiciona arquivos ao stage**
git add teste.txt        # adiciona um arquivo
git add .                  # adiciona todos os arquivos

**Faz o commit das alterações**
git commit -m "mensagem do commit"

**Ver histórico de commits**
git log

**Cria uma nova branch e muda para ela**
git checkout -b nova-feature

**Muda para outra branch**
git checkout main

**Junta alterações da branch 'nova-feature' com a 'main'. Execute-o sempre a partir da branch que irá receber as mudanças. Neste exemplo estamos na branch main mergeando, isto é, recebendo as mudanças da branch nova-feature. Após o merge, você pode excluir a branch criada para tal função.**
git merge nova-feature

**Atualiza o repositório local com as mudanças do remoto**
git pull

**Envia suas alterações para o GitHub**
git push origin minha-branch

**Ver diferenças entre arquivos**
git diff

**Reverte um commit**
git revert <id-do-commit>

---

## .gitignore

Arquivo responsável por indicar quais pastas ou arquivos não devem ser incluídos no repositório. Por exemplo:

- node_modules/
- .env
- *.log

## Boas práticas:

- Faça commits frequentes e com mensagens claras.
- Use branches para trabalhar em funcionalidades específicas.
- Sempre atualize sua branch com git pull antes de começar a trabalhar.
- Revise Pull Requests com atenção antes de fazer merge.
- Use tags para marcar versões importantes.

## E o Github, hein?

GitHub é, entre outras coisas, uma plataforma de hospedagem de repositórios Git na nuvem. Ele permite colaboração entre desenvolvedores, revisão de código, integração contínua (CI), e muito mais. Ele possibilita:

- Armazenar repositórios na nuvem
- Trabalhar com outras pessoas em um mesmo projeto
- Abrir issues e pull requests
- Usar GitHub Actions para automações

### Workflow básico de git e github:

1. Clone o repositório remoto com git clone
2. Crie uma branch com git checkout -b minha-branch
3. Faça alterações no código
4. Adicione e faça commit das alterações (git add + git commit)
5. Envie para o repositório remoto (git push origin minha-branch)
6. Crie um Pull Request no GitHub
7. Alguém revisa e faz o merge




