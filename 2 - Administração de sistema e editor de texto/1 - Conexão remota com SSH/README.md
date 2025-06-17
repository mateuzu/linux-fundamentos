## Acesso a máquinas remotas com SSH

Não há necessidade mais atual do que o acesso remoto a uma máquina. A 
obrigação de estar fisicamente presente em um terminal, em frente ao equipamento 
que se precisa manipular é absolutamente incompatível como os dias de hoje, uma 
realidade totalmente Cloud Computing, ou seja, por vezes, sequer sabemos em qual 
gabinete (ou quais) localiza-se o sistema que estamos utilizando. 

Há décadas, temos servidores sem monitores, teclados e mouses para chamar de seus, sendo 
necessário acessá-los de outro equipamento (bem, se voltar alguns anos mais, o 
mouse sequer existia). Terminais sem processamento, apenas com o monitor e 
teclado, eram destinados para tal fim (os chamados “terminais burros”). 

Tão lendário quanto esse paradigma é o serviço que possibilitava isso no 
Unix, o telnet. Ele leva esse nome por possibilitar, ainda, um terminal via linha 
discada (sim, você pode ligar para seu servidor). Infelizmente, o serviço tornou-se 
obsoleto pela falta de segurança: os comandos trafegam abertamente em uma rede, 
sem criptografia, o que tornava a comunicação suscetível a um ataque conhecido 
como sessionhijacking (ou sequestro de sessão), no qual o atacante sequestra a sessão como servidor, ao 
derrubar o cliente e se passar por ele (afinal, ele ficou um bom tempo “escutando” e 
sabe exatamente como o cliente se comporta). 

O SSH (Secure Shell ou shell seguro) é a evolução do telnet, permitindo toda 
a praticidade de um acesso remoto com uma diferença: um túnel criptografado, 
garantindo a privacidade e evitando ataques.

### ssh – acesso outra máquina remotamente

O comando SSH é o cliente para acesso a terminais remotos e seu uso é 
muito simples: ele deve ser seguido por: 
- usuário que desejo usar para conectar
- “@” (arroba)
- o nome ou IP do host. 

Caso o serviço de SSH não esteja em sua porta padrão (a 22), o parâmetro “-p” pode ser adicionado para indicar o número da porta. 

**Exemplo de acesso remoto à uma VM Linux na Azure**
```bash
ssh -i C:\Cert\ssh-key-linux.pem vm-linux@000.000.00.00
```
- Nesse caso, estou realizando o acesso de uma máquina WIndows para uma VM linux utilizando uma chave privada

Ao acessar pela primeira vez o host remoto, este apresenta sua fingerprint 
que, na prática, é a apresentação de sua chave pública. O serviço de SSH utiliza 
uma criptografia assimétrica, também conhecida como chave público-privada, da 
qual cada host gera um par de chaves, distribuindo a chave pública e guardando a 
privada para si. As informações do terminal trafegam criptografadas, utilizando a 
chave privada do host de origem e a pública do remoto, e só podem ser 
descriptografadas pela combinação inversa (a chave pública do host de origem e a 
privada do remoto) tornando esse tipo de acesso prático e seguro. 

Absolutamente todos os comandos aprendidos aqui (desde que instalados no 
servidor remoto) são válidos como se estivéssemos sentados na frente do 
equipamento. Na prática, não faz a menor diferença. Ao terminar, podemos digitar 
“exit” e retornar ao host original.

### Definindo hosts no Linux

Você pode tornar o acesso mais prático, definindo os hosts no Linux usando o 
arquivo `/etc/hosts`; mesmo que o sistema esteja com um servidor DNS (Domain 
Name System) configurado e traduzindo hostnames em IPs, o sistema sempre olha 
para o `/etc/hosts` primeiro.

O exemplo da abaixo mostra a adição de um novo host no arquivo 
`/etc/hosts`, sem a necessidade de um processador de textos. 

- Para tal, faz-se necessário tornar-se o superusuário root, pois o arquivo é protegido.
- O comando echo serve para “ecoar” algo no terminal, ou seja, tudo o que é digitado entre aspas seria repetido no terminal. 
- Os símbolos “>>” (maior-maior) redirecionam a saída padrão do comando echo para dentro de um arquivo, incluindo o resultado do comando ao respectivo final. 
    - Tome cuidado apenas para não usar um sinal de maior só (“>”), pois o efeito é o mesmo, mas apaga o arquivo /etc/hosts inteiro no processo. 
- A execução do comando cat mostra o antes e depois do arquivo:
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ cat /etc/hosts
127.0.0.1 localhost
# The following lines are desirable for IPv6 capable hosts
::1 ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
ff02::3 ip6-allhosts

azureuser@vm-linux-meus-estudos-east-us:~$ su
Password:

root@vm-linux-meus-estudos-east-us:/home/azureuser# echo "000.000.00.00 vm-linux" >> /etc/hosts

root@vm-linux-meus-estudos-east-us:/home/azureuser# exit

azureuser@vm-linux-meus-estudos-east-us:~$ cat /etc/hosts
127.0.0.1 localhost
# The following lines are desirable for IPv6 capable hosts
::1 ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
ff02::3 ip6-allhosts
000.000.00.00 vm-linux
```

A partir de agora, o equipamento remoto também pode ser acessado pelo hostname definido em `/etc/hosts`
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ ssh pi@vm-linux
```

### Definindo hosts no Windows

Para adicionar um novo host numa máquina Windows, basta modificar o arquivo de hosts que normalmente fica no caminho: `C:\Windows\System32\drivers\etc\hosts`. Vale ressaltar que o arquivo precisa ser editado como administrador, ou você não conseguirá salvar as alterações.

> Atenção ao arquivo hosts: Esse arquivo tem prioridade sobre o DNS, então ele pode ser usado para falsificar domínios.

**Para se proteger:**
- Só edite o hosts com um editor confiável (como o Bloco de Notas), executado como administrador.
- Verifique o conteúdo do arquivo se algo suspeito acontecer (como redirecionamentos estranhos).
- Mantenha seu sistema e antivírus atualizados — alguns malwares usam o hosts para ataques.



### scp - Transferindo arquivos de forma segura

O serviço de SSH não possibilita apenas o controle total de um servidor 
remoto, ele permite, ainda, a transferência de arquivos utilizando esse mesmo canal 
seguro. O comando `SCP` (SecureCopy ou cópia segura) pode ser utilizado para 
copiar arquivos inteiros de um servidor a outro, sem a necessidade de apelar para o 
antigo e ultrapassado FTP. 

A sintaxe do comando é `scp [origem] [destino]`, referindo-se ao arquivo local 
da mesma forma que o comando `cp` (copy) funciona, e o local remoto da forma que o SSH 
funciona.

>OBS: Vale ressaltar que estou em uma máquina Windows conectado à uma VM linux.

Vamos a um exemplo, para simplificar. Estando no diretório 
pessoal do usuário windows (C:\Users\user), aponto para o arquivo `DocumentoWindows.txt` 
dentro do subdiretório `Cert`, copiando-o para o diretório `/Documents` da vm-linux:
```bash
PS C:\Users\user> scp -i C:\Cert\ssh-key-linux.pem C:\Cert\DocumentoWIndows.txt azureuser@000.000.00.00:/home/azureuser/Documents

DocumentoWIndows.txt                                                                  100%    0     0.0KB/s   00:00
```
Vamos destrinchar para entender melhor:
- `scp`: Comando utilizado para trasnferencia de arquivo
    - `-i`: Flag para especificar o caminho da chave privada
- `C:\Cert\ssh-key-linux.pem`: Caminho da chave privada utilizada para autenticar a conexão
- `C:\Cert\DocumentoWIndows.txt`: Arquivo que será enviado à vm Linux
- `azureuser@000.000.00.00:/home/azureuser/Documents`: Diretório da vm Linux onde o arquivo será armazenado


Repare que podemos realizar o inverso: peço pelo arquivo DocumentoLinux.txt, que sei estar no diretório `/Documents` da `vm-linux`, e o copio para meu diretório do Windows atual (representando pelo símbolo “.”, o ponto):
- Vale ressaltar que eu poderia especificar qualquer diretório local
```bash
PS C:\Users\user> scp -i C:\Cert\ssh-key-linux.pem azureuser@000.000.00.00:/home/azureuser/Documents/DocumentoLinux.txt .
``` 

### Subindo um serviço SSH no Linux

Falamos, até então, em como acessar um serviço SSH disponível, mas como configurar um serviço como esse na máquina em que estamos? 
- Instale o serviço com `sudo apt install ssh`

Uma vez que o serviço foi instalado, podemos subir utilizando o comanado:
- `sudo /etc/init.d/ssh start` ou `service ssh start`

Feito isso, a porta SSH estará aberta no Linux. Portanto, qualquer usuário que tenha permissão de entrada poderá acessar esse serviço.

#### Tornando o SSH ainda mais seguro

Da maneira atual, o serviço está rodando da maneira padrão. Como o SSH é muito poderoso, é recomendado alguns cuidados.

Embora seja um serviço que conte com criptografia e possua uma 
configuração inicial razoavelmente segura, o acesso remoto provido pelo SSH é 
muito visado por eventuais invasores, justamente pelo seu potencial de manipulação 
remota (embora não seja, nem de longe, a única maneira de invadir uma máquina). 

Com o intuito de proteger ainda mais o serviço, seguem algumas dicas rápidas de como tornar o SSH mais seguro do que já é: 
- **Use senhas seguras em seus usuários**: o ataque mais banal que se procura fazer é um ataque de força bruta (ou brute force attack), no qual se conecta ao serviço e tenta-se adivinhar a senha, testando todas as combinações possíveis. Sem muita dificuldade, o atacante pode automatizar o processo, usando todas as palavras do vocabulário de um idioma, conhecido como ataque de dicionário, ou seja, senhas muito simples são facilmente descobertas e garantem um acesso simples e rápido. Não deixe isso acontecer.
- **Configure o serviço**: por falar em força bruta, o arquivo `/etc/ssh/sshd_config` permite uma série de parametrizações interessantes, como: 
    - Determinar o número de tentativas máximas para senhas. 
    - Garantir que o usuário root não possa se autenticar e restrinja o serviço para as interfaces de rede em que ele precise realmente ficar disponível. 
    - Restringir o acesso apenas para hosts conhecidos (aqueles dos quais você possui as chaves públicas). Não será prático adicionar novos hosts, mas torna-se mais seguro. Aprenderemos, na sequência, sobre o editor de textos vi e como alterar arquivos com ele.
- **Restrinja os usuários que podem usar SSH**: nem todo mundo precisa ter acesso remoto. Para fazê-lo, adicione, no arquivo /etc/ssh/sshd_config, as diretivas allowuser, para especificar os usuários permitidos (separe-os por vírgula), ou allowgroup, para liberar grupos. As diretivas denyuser e denygroup permitem definir quais usuários e grupos estão proibidos de usá-lo. 
- **Troque a porta de serviço**: no mesmo arquivo `/etc/ssh/sshd_config`, você pode trocar o número da porta de serviço por outro número que não seja o óbvio 22.
- **Apenas IPs permitidos**: aprenderemos sobre configuração de firewall em um capítulo de uma etapa seguinte; quando acontecer, você pode usar o comando iptables para definir quais IPs podem acessar a porta de serviço na qual o SSH está.
- **Identifique-se primeiro**: outro truque muito interessante, de que trataremos em outro capítulo, é a possibilidade de implementar um Port Knocking, que em tradução livre significa “batidas na porta”; configuramos o firewall para liberar a porta de serviço apenas quando pacotes de dados são enviados para portas específicas em uma determinada sequência; podemos até mesmo configurar a sequência para fechar a porta ao sair! 


Vamos visualizar o arquivo `/etc/ssh/sshd_config`.
- Utilizando o comando `less /etc/ssh/sshd_config` podemos visualizar o que tem dentro dele:
```bash
# This is the sshd server system-wide configuration file.  See
# sshd_config(5) for more information.

# This sshd was compiled with PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games

# The strategy used for options in the default sshd_config shipped with
# OpenSSH is to specify options with their default value where
# possible, but leave them commented.  Uncommented options override the
# default value.

Include /etc/ssh/sshd_config.d/*.conf

# When systemd socket activation is used (the default), the socket
# configuration must be re-generated after changing Port, AddressFamily, or
# ListenAddress.
#
# For changes to take effect, run:
#
#   systemctl daemon-reload
#   systemctl restart ssh.socket
#
#Port 22
#AddressFamily any
#ListenAddress 0.0.0.0
#ListenAddress ::

#HostKey /etc/ssh/ssh_host_rsa_key
#HostKey /etc/ssh/ssh_host_ecdsa_key
#HostKey /etc/ssh/ssh_host_ed25519_key
:

//CONTINUA
```