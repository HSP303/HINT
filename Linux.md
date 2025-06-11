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
- sudo systemctl restart networking # Reinicia o serviço de rede do Debian

## PARTITION

- sudo mkfs.vfat -F 32 /dev/sdx1 # Formata um pendrive
- sudo mount /dev/sdx1 /mnt/usb # Monta dispositivo USB/disco externo
- sudo mount -o loop arquivo.iso /mnt/iso
- sudo mount -t vfat /dev/sdx1 /mnt/flashdrive # Monta partição com fat32
- sudo unmount /mnt/usb ou sudo unmount /dev/sdx1 # Desmonta a partição
- lsblk # Mostra as partições
- sudo blkid /mnt/data # Descobre o tipo de sistema de arquivo da partição
- sudo mount -o exec /dev/sbx1 /mnt/data # Monta partição permitindo execução
- sudo umount /dev/sdx1; sudo eject /dev/sda % Ejetar pendrive

## Diagnóstico

- systemd-analyze # Mostra o tempo de inicialização do computador
- systemd-analyze # Mostra o que está consumindo tempo de inicialização

## Services
- brightnessctl set 100% # seta a luminozidade da tela
- sudo systemctl restart display-manager # logout
- sudo update-alternatives --config java # Mostra caminho onde a aplicação java está instalada
- ip link set wlan down; iw wlan set type managed; ip link set wlan up # Desabilitar o monitor mode da placa de rede
- systemctl --user restart pipewire pipewire-pulse # Reinicia o sistema de áudio

## IPtables

### Filtrar Por País
- 1. Criar ipset com IPs do Brasil (atualizado)
sudo ipset -N br-ips hash:net
sudo wget -O /tmp/br.zone http://www.ipdeny.com/ipblocks/data/countries/br.zone
while read ip; do sudo ipset add br-ips $ip; done < /tmp/br.zone

- 2. PERMITIR apenas IPs do Brasil na porta 80 (FILTER)
sudo iptables -A INPUT -p tcp --dport 80 -m set ! --match-set br-ips src -j DROP  # Bloqueia não-BR
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT  # Libera BR
