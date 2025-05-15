# GIT

## Inicialização de Repositório

- git init # Transforma o diretório em um repositório do **GIT**
- git add . # Adiciona os arquivos para o commit
- git commit -m "Mensagem de preferência" # Prepara as informações para serem lançadas no diretório remoto
- git branch -M main # Vai para a branch de preferência
- git remote add origin git@github.com:HSP303/3386-git-github-projeto_inicial.git # Sincroniza o local repo com o remote repo **(É feito somente uma vez)**
- git push -u origin main # Aplica as alterações no reposítorio remoto

## Remote

- git remote add apelido url # estabelece a conexão de um repositório local com um remoto
- git remote -v # Lista os repositórios remotos com suas respectivas URLs
- git remote remove origin # Esse comando remove um repositório remoto
- git remote set-url origin https://github.com/seu-usuario/seu-repositorio.git # Modifica a url de um repositório remoto
- git remote rename origin novo-origin

## Comandos de Manutenção

- git clone url
- git status # Mostra mudanças nos arquivos do projeto e quais ações foram feitas com eles
- git log # Mostra o histórico de commits
- git pull origin main # Pegas os commits do repositório remoto e baixa no local

## Comandos de Manutenção Específicos

- git reset --keep HEAD@{1} # Reverte um pull, quardando os commits realizados anteriormente
