## Comandos
### docker run
- docker run # O comando docker run é responsável por executar um container em nosso host.
- docker run -d # Roda em background
- docker run -P # mapeia uma porta aleatório
- docker run -p # mesma coisa que o -P porém você especifica a porta do container e a do host
- docker run -it -v /home/meu_dir:/dir_docker ubuntu bash # comando para persistir tudo o que está no dir_docker no meu_dir
- docker run -it --mount type=bind,source=/home/meu_dir,target/dir_docker ubuntu bash # mesma coisa que o acima, porém mais semântico

- docker build -t nome/image:1.0 # criar uma imagem a partir do diretório no qual o comando foi executado

- docker ps # lista os container em execução
- docker container ls # mesma coisa que o ps, porém mais semântico
- docker ps -a ou docker container ls -a # mostra os containers que estão e não estão em execução
- docker stop idnome do container # para o processo de um container
- docker rm # Remove o container

