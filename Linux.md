# SISTEMA OPERACIONAL

## Arquivos

- tar -xvzf nome_arquivo.tar.gz # Download de arquivo tar.gz
- gdb arquivo # Engenharia reversa
- grep -r "name" # procura todos os arquivos com a string "name"
- grep recursivo: grep -rnw STRING /local/inicio

## WIFI-BLUETOOTH

- nmcli dev wifi connect Nome_rede password senha123 # Conectar em uma rede wifi
- iptables -t nat PREROUTING -i eth1 -j REDIRECT # Redirecionamento no Servidor
- route ADD 0.0.0.0 MASK 255.255.255.255 10.0.0.1 # Redirecionamento para o servidor Windows

## PARTITION

- sudo mkfs.vfat -F 32 /dev/sdx1 # Formata um pendrive

## Diagnóstico

- systemd-analyze # Mostra o tempo de inicialização do computador
- systemd-analyze # Mostra o que está consumindo tempo de inicialização


