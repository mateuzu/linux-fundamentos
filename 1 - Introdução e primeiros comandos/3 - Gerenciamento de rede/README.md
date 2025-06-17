# Gerenciamento de rede

O gerenciamento de rede em uma máquina Linux é muito importante, pois ela pode atuar como um servidor, firewall, gateway, roteador, ou simplesmente como um cliente em uma rede corporativa ou doméstica. Ter controle sobre as interfaces de rede, rotas, portas e serviços permite ao administrador garantir conectividade, desempenho, segurança e estabilidade do sistema.

Em ambientes de servidores, especialmente em nuvem (como na Azure), o gerenciamento eficiente da rede é essencial para garantir o acesso remoto seguro, a distribuição de serviços, e a resiliência contra falhas e ataques.

>Para utilizar os comandos abaixo, vamos utilizar a biblioteca net-tools. Portanto, utilize o comando `sudo apt install net-tools` para instala-la.

Uma interface de rede no Linux é como a porta de entrada/saída que o sistema usa para se comunicar com outras máquinas, seja na internet ou em uma rede local.

### Exemplos de interfaces de rede
| Interface       | Tipo            | Descrição                                |
| --------------- | --------------- | ---------------------------------------- |
| `eth0`, `ens33` | Ethernet (cabo) | Placa de rede física ou virtual por cabo |
| `wlan0`         | Wi-Fi           | Conexão sem fio                          |
| `lo`            | Loopback        | A própria máquina (localhost, 127.0.0.1) |


### ifconfig - Visualizar interfaces de rede
Para visualizar a interface de rede, podemos utilizar o comando `sudo ifconfig`.
- Esse comando retorna informações das interfaces de redes que estão sendo utilizadas pela máquina, como: ip, mascara de sub rede, ipv6, estatisticas de pacotes, etc.
- É possível especificar um nome para que o retorno seja somente da interface específica
- A interface `lo` é uma pseudointerface da qual o sistema utiliza para se referir a si mesmo.
```bash
:~$ sudo ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.0.4  netmask 255.255.255.0  broadcast 10.0.0.255
        inet6 fe80::20d:3aff:fe8b:8364  prefixlen 64  scopeid 0x20<link>
        ether 00:0d:3a:8b:83:64  txqueuelen 1000  (Ethernet)
        RX packets 107884  bytes 20714645 (20.7 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 110439  bytes 24825766 (24.8 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 185  bytes 24699 (24.6 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 185  bytes 24699 (24.6 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

#### Derrubar/Subir uma interface de rede
Ainda com o comando `ifconfig`, é possível realizar operações de desligar e ligar as interfaces de rede. O comando `down`, junto com o nome da interface de rede, irá desligar aquela conexão, enquanto o `up` faz o contrário.
- `sudo ifconfig eth0 down`: Desliga a interface de rede
- `sudo ifconfig eth0 up`: Liga a interface de rede


#### Trocar o IP da interface de rede
Ainda com o comando `inconfig`, é possível realizar operações para modificação do IP. Após o nome da interface, especificamos o novo IP. Para a mascara, utilizamos a palavra `netmask` para determinar uma nova
- `sudo ifconfig eth0 10.0.2.10 netmask 255.255.255.0`

### route - Estabelecendo rotas de rede

Uma rota de rede define para onde o sistema deve enviar os pacotes de dados, dependendo do endereço de destino. Ela funciona como uma instrução de “por onde ir” para alcançar outro computador ou rede.

> 💡 Analogia: Imagine que o Linux é um carteiro, e a tabela de rotas é o mapa que diz qual estrada (interface de rede) usar para entregar cada carta (pacote IP).

O sistema operacional tem algumas rotas padronizadas, definidas pelo próprio sistema. Para visualiza-las, podemos utilizar o comando `sudo route`.

> A forma mais moderna de ver a tabela de rotas é com o comando `ip route`

- O comando abaixo vai listar todas as rotas definidas no sistema. 
```bash
:~$ sudo route -- ou ip route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         _gateway        0.0.0.0         UG    100    0        0 eth0
10.0.0.0        0.0.0.0         255.255.255.0   U     100    0        0 eth0
_gateway        0.0.0.0         255.255.255.255 UH    100    0        0 eth0
000.000.00.00   _gateway        255.255.255.255 UGH   100    0        0 eth0
000.000.00.00 _gateway        255.255.255.255 UGH   100    0        0 eth0
```
- Destination: O destino da rota (pode ser uma rede ou default)
- Gateway: Para onde enviar os pacotes
- Genmask: Máscara de rede usada para determinar o intervalo de IPs
- Flags:
    - U: Interface está ativa (up)
    - G: Usa um gateway
    - H: É um host específico
- Iface: Interface usada (por exemplo, eth0)

#### **Criar rotas**
Para criar rotas, podemos utilizar o parâmetro `-add net` e especificar algumas propriedades. Por exemplo, nesse comando: `sudo route -add net 000.000.00.00 dev eth0`, temos que:
- `sudo rout -add net`: Adiciona uma rota específica
- `000.000.00.00`: Especifica o IP
- `dev`: Especifica outro device/dispositivo
- `eth0`: A interface de rede.

Quando você cria uma rota:
- O Linux adiciona uma nova entrada na tabela de roteamento.
- Quando um pacote precisa ser enviado, o sistema verifica essa tabela para saber qual interface usar e se precisa passar por um gateway.

>Basicamente, estamos dizendo “Sempre que quiser falar com alguém da rede 000.000.00.x, envie os pacotes para o gateway 10.0.0.1 pela interface eth0.”

>`sudo ip route add 000.000.00.00/24 dev eth0` é uma versão mais moderna para criar rotas.

#### **Criar gateway**
Um gateway (ou roteador padrão) é o "caminho para fora" da sua rede local. É o dispositivo que recebe os pacotes destinados a redes que o Linux não conhece diretamente. Quando você quer acessar um site, seu computador envia o tráfego para o gateway, que cuida de encaminhar esse tráfego para a internet.

Para criar gateway, o comadno segue a estrutura: `sudo route add gw 10.0.2.1 eth0`.
- `sudo route add gw`: Adiciona um gateway
- `10.0.2.1`: Determina o IP do Gateway, nesse caso, esse é o IP padrão.
- `eth0`: A interface de rede que será utilizada


### nmap - Escaneamento de portas

>Nos exemplos abaixo, iremos usar a biblioteca `nmap`, portanto, utilize o comando: `sudo apt install nmap` para instala-la

O nmap é um port scanner, ou seja, um escaneador de portas de serviço. Além disso, é uma ferramenta poderosa para segurança, administração de redes e auditoria. Ele pode identificar serviços, sistemas operacionais, versões de software e até detectar vulnerabilidades conhecidas com os parâmetros corretos.

Em um servidor, temos vários serviços sendo disponibilizados e esses serviços ficam em portas específicas. Cada uma das portas abertas significa que um serviço está escutando com seu listener, mas também significa uma certa vulnerabilidade a invasões. Portanto, usar esse serviço é uma boa prática para verificar as portas que estão sendo executados e eventualmente fecha-las, se necessario.

Cada serviço, como SSH, HTTP ou FTP, escuta em portas específicas (por padrão: 22, 80, 21...). Manter apenas as portas essenciais abertas reduz a superfície de ataque do sistema, aumentando a segurança.

O comando `sudo nmap ip` irá verificar as portas abertas.

- No exemplo abaixo, estamos utilizando o ip do looback (lo), ou seja, ele vai escanear as portas da própria máquina.
```bash
:~$ sudo nmap 127.0.0.1
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-06-09 21:42 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.0000040s latency).
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE
22/tcp   open  ssh
631/tcp  open  ipp
3389/tcp open  ms-wbt-server

Nmap done: 1 IP address (1 host up) scanned in 0.09 seconds
```

#### nmap -sV - Escanear as portas de forma detalhada

É possível fazer um scan mais profundo nas portas, de modo que não mostre somente quais portas estão abertas, mas também quais serviços estão sendo executados. Para isso, utilize os parâmetros `-sV` junto com o comando `nmap`

- 🚨 Esse comando é recomendado somente quando você é o dono do servidor, pois ele já é considerado um ataque de segurança.
```bash
:~$ sudo nmap 127.0.0.1 -sV
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-06-09 22:04 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.0000040s latency).
Not shown: 996 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
22/tcp   open  ssh           OpenSSH 9.6p1 Ubuntu 3ubuntu13.11 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http          Apache httpd 2.4.58 ((Ubuntu))
631/tcp  open  ipp           CUPS 2.4
3389/tcp open  ms-wbt-server xrdp
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.38 seconds
```
- 22/tcp- SSH: O SSH (Secure Shell) é um protocolo de rede seguro para acesso remoto a terminais.
- 80/tcp - HTTP: A porta 80 é padrão para o protocolo HTTP (acesso web).
- 631/tcp - CUPS (ipp): IPP significa Internet Printing Protocol. O serviço CUPS (Common Unix Printing System) é usado para gerenciar impressoras no Linux, ele permite configurar filas de impressão, drivers, impressoras de rede, etc.
- 3389/tcp - RDP(xrdp): Porta 3389 é padrão para RDP (Remote Desktop Protocol). xrdp é um servidor RDP para sistemas Linux, permitindo acesso remoto com interface gráfica (como se fosse um “Área de Trabalho Remota”).

------------------------------------------

<br>

## Bônus - Criando um servidor web

>Para criar um servidor web de teste, vamos utilizar o pacote do apache. Portanto, utilize o comando `sudo apt install apache2`

#### Sobre o apache

O Apache é um dos principais servidor web da internet, muito utilizado em sistemas linux. Ele é muito utilizado em hospedagens de sites, sistemas de intranet e ambientes de desenvolvimento local. 

O Apache é um servidor HTTP que atende requisições feitas por navegadores ou ferramentas HTTP. Ele serve páginas HTML, scripts PHP, entre outros.

Com o apache, conseguimos criar um serviço web.

### Subindo o serviço

Feita a instalação do Apache, precisamos inicar o serviço. Normalmente, o diretório `/etc/init.d/apache2` armazena o daemon do apache, que nada mais é que um serviço de inicialização.

Para inicializa-lo, utilizamos o comando `sudo /etc/init.d/apache2 start`

>Para derruba-lo, utilize o mesmo comando porém com o `stop`, exemplo: `sudo /etc/init.d/apache2 stop`. Além disso, podemos visualizar o status com o `status`

```bash
:~$ sudo /etc/init.d/apache2 start
Starting apache2 (via systemctl): apache2.service. --Indica que o serviço foi inicializado
```

Como não temos uma interface gráfica, vamos testar usando o `links` para usar o navegador pelo terminal.
- Usamos o ip `127.0.0.1` que é da própra máquina (semelhante ao localhost do Windows)
```bash
:~$ links 127.0.0.1

Ubunto Logo
Apache2 Default Page
It Works!

This is the default welcome page used to test the correct operation of the Apache2 server after installation on Ubuntu systems. It is based on the equivalent page on Debian, from which the Ubuntu Apache packaging is derived. If you can read this page, it means the Apache HTTP server installed at this site is working properly. You should replace this file (located at /var/www/html/index.html) before continuing to operate your HTTP server.

//Continua
```

> Esse arquivo HTML pode ser editado para criar sua própria página web. Basta alterar o conteúdo de: `/var/www/html/index.html`

Verificando as portas novamente, veremos que a porta 80 está aberta (utilizadas em serviços web)
```bash
:~$ sudo nmap 127.0.0.1
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-06-09 22:02 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.0000040s latency).
Not shown: 996 closed tcp ports (reset)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
631/tcp  open  ipp
3389/tcp open  ms-wbt-server

Nmap done: 1 IP address (1 host up) scanned in 0.09 seconds
```

### Dica final: Segurança

Cada porta aberta representa uma porta de entrada no seu sistema. Use esses critérios:
| Porta | Necessária?        | Ação recomendada                       |
| ----- | ------------------ | -------------------------------------- |
| 22    | Sim (para SSH)     | Deixe aberta, mas use chave e firewall |
| 80    | Sim (para web)     | OK se estiver usando Apache            |
| 631   | Provavelmente não  | Pode desativar o serviço `cups`        |
| 3389  | Depende (usa GUI?) | Desative `xrdp` se não usar desktop    |

>OBS: Você pode parar/reiniciar/iniciar/verificar qualquer serviço com a sintaxe `serivce nome_serviço start/stop/restart/status`. Por exemplo, para desativar o cups, utilize: `service cups stop` (para esse comando, é necessário realizar a autenticação)


