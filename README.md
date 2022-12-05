# Servidor DHCP
Instalando e configurando o Servidor DHCP no Ubuntu Server. 


## Atualizando pacotes
Primeiro, deve-se baixar os pacotes do Ubuntu Server.

`sudo apt update`

E logo em seguida, instalar e atualizar os pacotes.

`sudo apt upgrade`


## Instalando o servidor DHCP
Para instalar o pacote do servidor DHCP, execute o comando

`sudo apt-get install isc-dhcp-server`


## Configurando o IP estático da placa de rede
Para que o servidor não pegue o IP da rede que estamos usando, devemos especificar o IP da placa de rede do servidor. Da seguinte maneira:

* Acessar o arquivo de configurações da placa com: `sudo nano /etc/netplan/00-installer-config.yaml`
* Desativar o dhcp4 `dhcp4: false`
* Definir um IP `addresses: [endereço IP]`

Após definir um IP para o servidor, aplique as configurações com o comando `sudo netplan apply`

Depois verifique se a placa conseguiu receber as configurações com o comando `ifconfig`


## Definindo a interface para distribuir os IPs
Para alterar a interface de distribuição de IPs do servidor, execute o comando

`sudo nano /etc/default/isc-dhcp-server`

E altere INTERFACESv4="" para INTERFACESv4="enp0s3".

Assim, o servidor usará a porta ether.


## Criando backup do arquivo dhcpd.conf
Antes de mexer no arquivo de configurações do servidor DHCP, é aconselhável fazer um backup antes.

Para fazer o backup do arquivo `dhcpd.conf`, execute o comando: 

`sudo cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.bkp`


## Configurando o arquivo dhcpd.conf
No arquivo `dhcpd.conf`, é possível definir algumas configurações do servidor DHCP.

Para abrir o arquivo `dhcpd.conf`, execute o comando: 

`sudo nano /etc/dhcp/dhcpd.conf`

##

Dentro do arquivo, você irá encontrar algumas opções como:

`ddns-update-style none`; Significa que não temos nenhum servidor dns especificado;

`authoritative`; Significa que esse servidor com o serviço dhcp prevalecerá sobre os demais;

`subnet 192.168.10.0 netmask 255.255.255.0`; Está especificando o endereço da rede e da máscara
    
`range 192.168.10.15 192.168.10.254`; Define a faixa de ips que serão distribuídos;
    
`option-domain-name-servers 8.8.8.8`; 8.8.4.4;  Contém os servidores DNS que serão usados pelas estações. 
    
`option routers 192.168.10.2`; Aqui se encontra o endereço default gateway, neste caso o endereço do servidor está compartilhando a conexão na rede.  
    
`option broadcast-address 192.168.10.255`; Especifica o endereço broadcast da rede;
    
`default-lease-time 600`; Especifica o tempo de concessão para 600 segundos (10 min)
    
`max-lease-time 7200`; Especifica o tempo máximo de concessão para 7200 segundos(2h)

## Verificando se o servidor possui algum erro
Para garantir que não há nenhum erro com o serviço, verifique com o seguinte comando: 

`dhcpd -d`

## Iniciar o serviço
Para iniciar o serviço, execute: 

`sudo service isc-dhcp-server restart`

##
