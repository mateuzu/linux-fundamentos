
## Políticas de firewall

O firewall mais utilizado do Linux, o iptables, é capaz de filtrar qualquer pacote de dados que entre ou saia por qualquer uma das interfaces de rede do equipamento.

Temos as seguintes correntes de regras como padrão: 

| **Nome da Corrente** | **Descrição** | **Regras a serem aplicadas** |
|----------------------|---------------|------------------------------|
| **INPUT**            | Regras para pacotes que **chegam ao equipamento** (destino é o sistema local). | Regras a serem aplicadas para qualquer pacote que tenha como **destino** o sistema em que estamos. |
| **OUTPUT**           | Regras para pacotes que **partem do equipamento** (origem é o sistema local). | Regras a serem aplicadas para qualquer pacote que tenha como **origem** o sistema em que estamos. |
| **FORWARD**          | Regras para pacotes que **passam pelo sistema** (intermediário entre interfaces). | Regras a serem aplicadas para qualquer pacote que passe pelo sistema em que estamos. Chegam por uma interface e saem por outra. |
| **PREROUTING**       | Disponível para casos específicos quando o equipamento faz **NAT** (Network Address Translation). | Usado para **SNAT** (Source NAT, quando se muda o endereço de origem). |
| **POSTROUTING**      | Disponível para casos específicos quando o equipamento faz **NAT** (Network Address Translation). | Usado para **DNAT** (Destination NAT, quando se muda o endereço de destino). |


Quanto às ações que podem ser tomadas, temos: 

| **Nome da Ação** | **Descrição** |
|-------------------|---------------|
| **ACCEPT**        | Aceita o pacote, liberando-o para entrada, saída ou passagem pelo equipamento. |
| **REJECT**        | Rejeita o pacote devolvendo uma resposta ao remetente. Para UDP, ocorre um ICMP do tipo 3, e para TCP um TCP reset. |
| **DROP**          | Descarte o pacote silenciosamente, sem retornar qualquer tipo de resposta ao remetente. |


**Notas:**
- **PREROUTING** é usado antes do roteamento, para modificar pacotes quando eles chegam ao dispositivo.
- **POSTROUTING** é usado após o roteamento, para modificar pacotes antes de saírem do dispositivo, muito comum em **gateways de rede**.


### iptables - Filtragem de pacotes

Para fazer as regras de firewall, iremos utilizar um script shell.
>É muito comum que o script shell fique armazenado na pasta `.boot` do Linux. Com isso, toda vez que o sistema é inicializado, o script é executado e a configuração de firewall é aplicada.

O comando que será utilizado para a filtragem de pacotes é o iptables. No 
quadro “Principais parâmetros do comando iptables”, esclarecemos o que fazem os 
principais parâmetros disponíveis:

| **Nome da Ação** | **Descrição** |
|-------------------|---------------|
| **-A**            | Adiciona uma nova regra ao final da corrente. |
| **-D**            | Remove uma ou mais regras de uma corrente. |
| **--dport**       | Permite informar a porta específica em que a regra se aplica. |
| **-F**            | Remove todas as regras do firewall ou de uma corrente em específico. |
| **-I**            | Permite definir a qual interface se aplica a regra. |
| **-j**            | Permite definir a ação que será aplicada pela regra (ACCEPT | REJECT | DROP). |
| **-A (top)**      | Adiciona uma regra no topo da corrente. |
| **-L**            | Lista as regras do firewall ou de uma corrente em específico. |
| **-P**            | Estabelece a política padrão de uma determinada corrente. |
| **-t**            | Permite informar o tipo de pacote (tcp | udp). |
| **-s**            | Permite informar qual o IP de origem do pacote. |
| **-X**            | Remove todas as regras de uma corrente. |

>Vale ressaltar que existem outros parâmetros referentes ao firewall, mas esses são os mais importantes.

Para que a demonstração a seguir seja realmente relevante, utilizei o host 
que possuo na Digital Ocean, assim sendo, estou estabelecendo regras de firewall 
para um equipamento que está exposto à Internet e pode se beneficiar muito destas 
regras. Pode usar quaisquer duas máquinas para testar as regras, podendo ser até 
mesmo a relação entre o Windows de uma máquina real e o Linux virtualizado com o 
Oracle VirtualBox, tudo depende do tipo de rede que foi estabelecido entre as 
máquinas e as regras que deseja testar.   

Para começar nossa demonstração, criaremos um arquivo no host da Digital 
Ocean chamado /etc/firewall.sh com o comando `sudo vi /etc/firewall.sh` e 
digitaremos inicialmente as seguintes linhas:
```bash
#!/bin/bash 

# Limpa as regras de todas as correntes  
iptables -F  
iptables -X  

# Estabelece as políticas padrão das correntes  
iptables -P INPUT ACCEPT  
iptables -P FORWARD ACCEPT  
iptables -P OUTPUT ACCEPT  

# Rejeita quaisquer conexões na porta 80  
iptables -A INPUT -i eth0 -p tcp --dport 80 -j REJECT 
```

Com estas poucas linhas, rejeitaremos todos as requisições que chegam na 
porta 80, ou seja, a porta do serviço HTTP da qual se encontra o Apache. 

Ao salvar 
o arquivo, digitarei o comando `sudo chmod 700 /etc/firewall.sh` para que o 
comando seja lido, modificado e executado pelo dono do arquivo, o root (4+2+1 = 7), e não possa ser acessado por mais ninguém “0”

O comando `sudo /etc/firewall.sh` é o suficiente para rodar as regras, 
pois o arquivo é executável ao root graças ao chmod e o interpretador de comandos 
foi definido na primeira linha do arquivo (#!/bin/bash), ou seja, criamos nosso primeiro *bash script*. 

Com o comando `sudo iptables -L`, conseguimos ver as três correntes 
(INPUT, FORWARD e OUTPUT) e as políticas aplicadas a cada uma delas. Além 
disso, nossa primeira regra de rejeição está listada na corrente INPUT e, portanto, 
ativa. 

Neste momento, é impossível acessar o Apache do servidor através do 
navegador web, pois o firewall está bloqueando a entrada do pacote. O serviço está 
rodando normalmente e “escutando” a porta 80, mas os pacotes simplesmente não 
chegam.

>A ordem que as regras são definidas é importante.

-----------------------------------------

<br>

## Segurança com port-hnocking
As portas de serviço de um equipamento não precisam ficar 100% do tempo disponíveis e vulneráveis a ataques. Um recurso que pode aumentar muito a segurança é implementar o port-knocking (traduzido para “batidas na porta”) que remete àqueles filmes antigos em que a porta misteriosa seria aberta por alguém dentro do estabelecimento, se o personagem batesse na porta na sequência esperada

É exatamente isso que acontece, mas a porta aberta é uma porta de serviço liberada por uma regra de firewall disparada, se alguns pacotes de rede específicos atinjam algumas portas na sequência certa.

Para facilitar, vamos instalar o serviço chamado knockd usando o comando:
- `sudo apt install knockd`

Vamos realizar algumas mudanças de regras de firewall no arquivo 
`/etc/firewall.sh`. Repare a presença de uma regra baseada no parâmetro `–ctstate` que permite que conexões entrantes pré-existentes continuem aceitas e não sejam derrubadas. Encerramos as regras com uma que rejeita todas as conexões entrantes além das estabelecidas
```bash
#!/bin/bash 

# Limpa as regras de todas as correntes 
iptables -F 
iptables -X 

# Estabelece as políticas padrão das correntes 
iptables -P INPUT ACCEPT 
iptables -P FORWARD ACCEPT 
iptables -P OUTPUT ACCEPT 

# Rejeita quaisquer conexões na porta 80 
iptables -A INPUT -i lo -j ACCEPT 

# Aceita e mantém conexões já estabelecidas 
iptables -A INPUT -m conntrack --ctstateRELATED,ESTABLISHED 
j ACCEPT 

# Aceita conexões na porta 80 livremente 
iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT 

# Rejeita outras conexões entrantes 
iptables -A INPUT -j DROP 
```

Ao confirmar as regras com o comando `sudo /etc/firewall.sh`, a 
conexão ssh atual não é derrubada, graças à regra de aceite mencionada 
anteriormente.

O arquivo `/etc/knockd.conf` nos é entregue praticamente pronto, exigindo 
apenas uma alteração: a regra para abrir a porta 22 para meu IP deve ser criada com `iptables -I INPUT -s %IP% -p tcp --dport 22 -j ACCEPT e não 
iptables -A INPUT -s %IP% -p tcp --dport 22 -j ACCEPT`,  a nova 
regra de firewall com o parâmetro “-A” ficaria após a regra de DROP e jamais seria 
alcançada. Sua correção como parâmetro "-I” para que a regra seja executada ANTES do DROP, resolvendo o problema:
```bash
[options]  
UseSyslog 

[openSSH]  
sequence    = 7000,8000,9000  
seq_timeout = 5  
command     
= /sbin/iptables -IINPUT -s %IP% -p tcp --dport 22 -j ACCEPT  
tcpflags    = syn

[closeSSH]  
sequence    = 9000,8000,7000  
seq_timeout = 5  
command     
= /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT  
tcpflags    = syn 
```

Cabe uma última alteração no arquivo `/etc/default/knockd`, trocando o 
parâmetro START_KNOCKD de "0" para "1"
```bash
# control if we start knockd at init or not 
# 1 = start 
# anything else = don't start 
# PLEASE EDIT /etc/knockd.conf BEFORE ENABLING 
START_KNOCKD=1 
# command line options 
#KNOCKD_OPTS="-i eth1" 
```

Basta agora subir o serviço knockd com o comando `sudo /etc/init.d/knockd start` e o port-knocking está devidamente implementado. 

Cabe testar a segurança, inicialmente realizando um escaneamento na porta 
22 que indica uma filtragem ali; a tentativa de conexão direta na porta resulta em um timeout

Conforme visto em Código-fonte “O arquivo /etc/knockd.conf com uma ligeira 
alteração”, o serviço espera por pacotes nas portas 7000, 8000 e 9000 
respectivamente, e o próprio comando nmap pode ser usado para isso. 

Abaixo, temos um pequeno shell script que dispara os pacotes nas portas 7000, 8000 e 9000, realizando o port-knocking e 
permitindo a conexão ssh na sequência. Caso queira realizar, troque o ip para o corresponde a maquina:
```bash
$ for x in 7000 8000 9000; do nmap -Pn --host_timeout 201 --max-retries 0 -p $x 67.205.133.93; done 
```

Conforme pode ser visto, o serviço knockd pode fechar a porta se os pacotes forem disparados na sequência contrária (portas 9000, 8000 e 7000), cabendo uma ligeira modificação.

Para aumentar a segurança, altere todas as portas do exemplo mostrado 
aqui. Comece alterando o arquivo `/etc/ssh/sshd_config` para que o serviço 
ssh suba em uma porta diferente da 23. 
Depois, altere `/etc/knockd.conf` para 
que a regra do firewall libere a mesma porta que o ssh subirá. 

Além disso, modifique as portas que abrirão e fecharão o ssh, o port-knocking deve ocorrer em portas fora do padrão, pois as primeiras que um atacante tentará bater são justamente 7000, 8000 e 9000, pois são o padrão predefinido por knockd. Suba os serviços de sshd e knockd e sinta-se mais seguro do que antes.