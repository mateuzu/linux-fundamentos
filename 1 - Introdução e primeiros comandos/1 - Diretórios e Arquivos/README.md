## Manipulação de arquivos e pastas

Navegar pelo sistema de arquivos de um sistema operacional organizando 
arquivos em diretórios (ou pastas) é uma das necessidades primárias em um 
sistema operacional, sendo, assim, vejamos alguns comandos úteis nesta tarefa.

### Raiz de diretórios

Antes de abordar os comandos, no entanto, precisamos falar um pouco sobre 
a raiz de diretórios do sistema operacional Linux. Como a maioria dos usuários 
conhece a árvore de um sistema Windows, tentarei explicar fazendo um paralelo 
entre eles, quando for possível.

```bash
azureuser@vm-linux-meus-estudos-east-us:~$ ls /
bin                dev   lib                lost+found  opt   run                 snap  tmp
bin.usr-is-merged  etc   lib.usr-is-merged  media       proc  sbin                srv   usr
boot               home  lib64              mnt         root  sbin.usr-is-merged  sys   var
```

**Vamos entender os principais diretórios**
- `bin`: Princiais executáveis de comando
- `boot`: Diretório de bootagem do sistema operacional, ou seja, os arquivos que o Linux utiliza para inicializar
- `dev`: Armazena pseudoarquivos. Todos os recursos físicos (webcam, pendrive, disco rigído, impressora) são transformado em arquivos. Quando é necessário acessar algum desses recursos, um arquivo que o representa fica armazenado no diretório dev. Essa abordagem traz diversas vantagens precisamos acessar algum recurso por linha de comando
- `etc`: Esse diretório faz a configuração dos principais serviços do sistema operacional, ou seja, os arquivos de configuração.
- `home`: Armazena os diretórios de usuários comuns, é o equivalemente ao diretório `C:User` do Windows
- `lib`: Armazena as bibliotecas do sistemas
- `lost+found`: Armazena arquivos corrompidos ou arquivos verificados, podem ser resgatados posteriormente
- `media` e mnt: Diretórios de montagem, o mnt é mais antigo. 
- `opt`: Diretporio opcionak de instalação 
- `proc`: Possui pseudoarquivos referentes ao hardware da máquina (processador, memória, disco, etc).
- `root`: Diretório do superuser
- `run`: Diretório que armazena os processos. Todo programa executada se torna um processo no Sistema Opereacional, no linux eles são transformados em arquivos e armazenados dentro desse diretório, contendo um identificador unico e propriedades
- `sbin`: Superbinario, comandos especificos para o superuser
- `srv`: Armazena os serviços do sistema
- `sys`: Guarda informações para dispositivos USB
- `tmp`: Diretório para arquivos temporários. Equivalente ao `C:\temp` do Windowas
- `usr`: Onde os programas ficam instalados no Linux. Equivalente ao Arquivos de Programas do Windows
- `var`: Armazena arqiovps de tamanhos variados, como log de sistema.

<br>

----------------------------------------------------

### ls - Listar e diretórios arquivos de forma personalizada

O comando `ls` permite exibir informações dos arquivos e subdiretórios de um determinado diretório

Em sua execução simples, exibirá apenas o nome dos arquivos e diretórios. 

Graças ao padrão de cores, podemos diferenciar o que é arquivo (em verde) do que é diretório (em azul). Não se baseie, no entanto, em extensões de arquivos: diferente do Windows, elas são completamente dispensáveis. A extensão “txt” nos três arquivos do exemplo é apenas para fácil identificação e para seu maior conforto. 

O comando `ls` é geralmente executado com dois parâmetros muito úteis: “`-l`”, que exibe uma lista longa ou detalhada de informações, e o “`-a`”, que vem de all, ou seja, traga todos os arquivos e diretórios, mesmo aqueles que são ocultos

>Vale ressaltar que existem diversos parâmetros que podem ser utilizados para personalizar o retorno do comando. Para visualizar todos os possíveis parâmetros, utilize o comando `man ls` ou `ls --help`

```bash
azureuser@vm-linux-meus-estudos-east-us:~$ ls -la /
total 92
drwxr-xr-x  22 root root  4096 Jun  5 15:09 .
drwxr-xr-x  22 root root  4096 Jun  5 15:09 ..
lrwxrwxrwx   1 root root     7 Apr 22  2024 bin -> usr/bin
drwxr-xr-x   2 root root  4096 Feb 26  2024 bin.usr-is-merged
drwxr-xr-x   5 root root  4096 May 21 08:40 boot
drwxr-xr-x  18 root root  4060 Jun  5 15:10 dev
drwxr-xr-x 144 root root 12288 Jun  4 19:21 etc
drwxr-xr-x   3 root root  4096 Jun  4 18:22 home
lrwxrwxrwx   1 root root     7 Apr 22  2024 lib -> usr/lib
drwxr-xr-x   2 root root  4096 Apr  8  2024 lib.usr-is-merged
lrwxrwxrwx   1 root root     9 Apr 22  2024 lib64 -> usr/lib64
drwx------   2 root root 16384 May 21 08:38 lost+found
drwxr-xr-x   2 root root  4096 May 21 08:35 media
drwxr-xr-x   3 root root  4096 Jun  5 15:10 mnt
drwxr-xr-x   2 root root  4096 May 21 08:35 opt
dr-xr-xr-x 217 root root     0 Jun  5 15:09 proc
drwx------   4 root root  4096 Jun  4 19:15 root
drwxr-xr-x  34 root root  1020 Jun  5 15:11 run
lrwxrwxrwx   1 root root     8 Apr 22  2024 sbin -> usr/sbin
drwxr-xr-x   2 root root  4096 Mar 31  2024 sbin.usr-is-merged
drwxr-xr-x   2 root root  4096 Jun  4 18:22 snap
drwxr-xr-x   2 root root  4096 May 21 08:35 srv
dr-xr-xr-x  12 root root     0 Jun  5 15:09 sys
drwxrwxrwt  14 root root  4096 Jun  5 15:10 tmp
drwxr-xr-x  12 root root  4096 May 21 08:35 usr
drwxr-xr-x  14 root root  4096 Jun  4 18:33 var
```
---------------------------------------------
### Entendendo as nomenclaturas

#### **Primeira letra**
A primeira letra determina o tipo do arquivo: 
- Quando inicia com `d`, indica o que estamos visualizando é um diretório
- Quando inicia com `l`, indica o que estamos visualizando é um link, ou seja, um arquivo que aponta para outro.
- Quando inicia com `-`, indica o que estamos visualizando é um arquivo comum
- Quando inicia com `c` ou `b` (normalmente visualizando quando listamos o diretório `/dev`), indica o que estamos visualizando são pseudoarquivos que representam periféricos do sistema
    - O c transmite em formato caracter
    - O b transmite em formato bits

#### **Permissões**
As permissões controlam quem pode acessar, modificar ou executar um arquivo. Após a primeira letra, você verá uma sequência de 9 caracteres, divididos em 3 blocos de 3 caracteres cada:
- d`rwxr-xr-x`. Isso em destaque é a permissão

Sâo 3 permissões divididas para cada tipo de usuário/acesso, podendo ser para o dono, grupo ou qualquer usuário. 
- O primeiro bloco refere-se ao dono do arquivo.
- O segundo bloco refere-se ao grupo.
- O terceiro bloco refere-se a outros usuários (qualquer um que não seja dono ou parte do grupo).

Cada bloco pode conter os seguintes caracteres:
- `r`: Permissão de leitura (read).
- `w`: Permissão de escrita (write).
- `x`: Permissão de execução (execute).
- `-`: Permissão ausente.

Para ficar mais fácil entender, vamos "quebrar" as permissões. <br>
- Cada nivel de permissão tem 3 letras, podendo ser: `r`, `w` e `x` (read, write e execute). Ou seja, o dono, grupo ou qualquer usuário, tem 3 caracteres que representam a sua permissão para aquele arquivo 
- Quando temos o `-`, significa ausência de uma permissão específica.
- Ao separar as permissõesm, temos o seguinte: `1° rwx     2° r-x      3° r-x`  

Vamos a explicação:
- `rwx`: Essa primeira permissão é referente ao dono daquele arquivo (o dono pode ser visto numa coluna mais a frente). Represdenta `read`, `write` e `execute`, ou seja, indica que pode ler, escerver e executar.
- `r-x`: A segunda permissão é referente ao grupo (ou seja, para todos os integrantes do grupo). Nesse caso, representa `read` e `exeucte`, ou seja, pode ser executado e lido. Repare que aqui ele não possui a permissão de escrita `w`, então é substituida pelo `-`
- `r-x`: A terceira permissão é referente a todos os outros usuários (que não sejam dono ou parte do grupo). Nesse caso, representa `read` e `exeucte`, ou seja, pode ser executado e lido executado por qualquer um. Aqui também ele não tem a permissão de escrita,

**Exemplo**<br>
`rwxr-xr-x` Significa:
- O dono tem permissões `rwx` (pode ler, escrever e executar).
- O grupo tem permissões `r-x` (pode ler e executar, mas não escrever).
- Outros usuários também têm permissões `r-x` (podem ler e executar, mas não escrever).

>Quando um usuario/grupo não possui permissão de escrita (`w`), significa, entre outras coisas, que ele não pode ser apagado.

#### **Demais informações**
>Ao utilizar o parâmetro `h` em `-lah`, podemos visualizar as informações de modo mais organizado.
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ ls -lah /
total 92K
drwxr-xr-x  22 root root 4.0K Jun  5 15:09 .
drwxr-xr-x  22 root root 4.0K Jun  5 15:09 ..
lrwxrwxrwx   1 root root    7 Apr 22  2024 bin -> usr/bin
```
Vamos visualizar o primeiro arquivo para destrinchar: `drwxr-xr-x  22 root root 4.0K Jun  5 15:09 .`
- `d`: A primeira letra representa o tipo do arquivo. O `d` indica que é um diretório
- `rwxr-xr-x`: Refere-se as permissões que aquele arquivo tem para cada tipo de acesso, sendo que:
       - `rwx`: Permissões do dono do arquivo, ou seja, o dono tem permissões de leitura, escrita e execução (`rwx`).
       - `r-x`: Permissões do grupo, ou seja, o grupo pode ler e executar, mas não escrever (`r-x`).
       - `r-x`: Permissoes para outros usuários, ou seja, outros usuários podem ler e executar, mas não escrever (`r-x`).
- `22`: Indica quantos arquivos existem dentro daquele diretório. Quando o número é 1, normalmente indica que é um arquivo comum.
- `root`: Representa o dono do arquivo
- `root`: Representa o grupo do arquivo. Nesse caso, é o grupo de root onde só o superuser tem acesso
- `4.0K`: Indica o tamanho do arquivo. Nesse caso, 4 kbytes.
- `Jun  5 15:09`: Representa a data da última modificação daquele arquivo/diretório
- `.`: Representa o nome do arquivo ou diretório. Quando inicia com `.`, indica que esse arquivo é oculto (se executarmos o comando de listagem sem o parâmetro `a`, esse arquivo não aparece).

### pwd - Mostrar o diretório ou pasta atual

Exibe o diretório atual (pwd – print work directory):

```bash
azureuser@vm-linux-meus-estudos-east-us:~$ pwd
/home/azureuser
```

>É um comando pertinente especialmente em situações em que o prompt de 
comandos não exibe o diretório atual. No exemplo em questão, possui um prompt 
que mostra o diretório, mas de maneira resumida: “~” será sempre o diretório-padrão 
ou home do usuário que está sendo utilizado. 


### cd - mudar de diretório ou pasta

O comando é um acrônimo de change directory e permite mudar o diretório 
atual. O diretório pode ser mencionado tanto com seu caminho completo ou absoluto (`/home/azureuser`) quanto o caminho relativo, que recebe este nome porque é relativo ao diretório que estamos naquele momento. Ou seja, cd hpoyatos (sem a barra antes do azureuser), estando no diretório `/home`, possibilitará acessar o subdiretório `azureuser`, `/home/azureuser`. 

- O comando `cd..` indica o acesso ao diretório superior ou pai (simbolizado 
pelo ponto-ponto). 
- A chamada do comando `cd` sem nenhuma informação adicional resulta na troca para o diretório-padrão do usuário, ou seja, equivale ao comando cd `~` ou, no meu caso, `cd /home/azureuser`. 

### mkdir - Criar um diretório

O comando `mkdir` é uma abreviação da frase inglesa “make a directory”, que 
significa literalmente “criar um diretório”, veja um exemplo de seu funcionamento.

Repare que caracteres especiais (como o espaço “ “ ou a própria barra invertida) são permitidos, desde que sejam precedidos por uma barra invertida. Relembro que o Linux é case-sensitive, sendo, assim, a pasta criada no exemplo foi “Nova Pasta” e não “nova pasta”

Outra consideração é o fato de que o comando não me congratulou por ter 
escrito o comando corretamente; não acontece uma mensagem “Diretório criado com sucesso”. O sistema operacional só apresenta uma mensagem em caso de 
fracasso 
- Um parâmetro muito útil para acompanhar o mkdir é o “`-p`”, especialmente em situações que eu esteja criando subdiretórios
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ mkdir -p novapasta/subpasta/subpasta
```
>Sem o parâmetro “-p”, o comando apresentaria um erro, pois ele tentaria localizar a “Nova Pasta2” e não encontraria, ou seja, mkdir Nova\ Pasta2/subpasta/subsubpasta (sem o “-p”) só criaria o diretório “subsubpasta”

**Manual do comando**
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ mkdir --help
Usage: mkdir [OPTION]... DIRECTORY...
Create the DIRECTORY(ies), if they do not already exist.

Mandatory arguments to long options are mandatory for short options too.
  -m, --mode=MODE   set file mode (as in chmod), not a=rwx - umask
  -p, --parents     no error if existing, make parent directories as needed,
                    with their file modes unaffected by any -m option.
  -v, --verbose     print a message for each created directory
  -Z                   set SELinux security context of each created directory
                         to the default type
      --context[=CTX]  like -Z, or if CTX is specified then set the SELinux
                         or SMACK security context to CTX
      --help        display this help and exit
      --version     output version information and exit

GNU coreutils online help: <https://www.gnu.org/software/coreutils/>
Report any translation bugs to <https://translationproject.org/team/>
Full documentation <https://www.gnu.org/software/coreutils/mkdir>
or available locally via: info '(coreutils) mkdir invocation'
```

### Copiando, movedo e removendo arquivos

Para os comamdos dessa aula, iremos utilizar esse diretório como base:
```bash
Arquivo.txt  Documents  Pictures   Videos               novoDiretorio2
Arquivos     Downloads  Public     copiaNovoDiretorio3  novoDiretorio3
Desktop      Music      Templates  novoDiretorio        thinclient_drives
```

Tão comum quanto a necessidade de criar e me mover entre diretórios, é a necessidade de copiar informações através de arquivos. Para isso, utilizamos o comando `cp`.

- cp origem destino: Copiar arquivos ou diretórios inteiros.
       - Origem: Arquivo/diretorio que será copiado
       - Destino: Arquivo/diretorio onde o arquivo será enviado.

**Copiando arquivos**<br>
Ao copiar arquivos, podemos especificar um novo nome para ele:
- O comando abaixo, irá copiar o `Arquivo.txt` para o diretório *Arquivos*, porém modificando seu nome para `NomeAtualizado2`
```bash
~$ cp Arquivo.txt Arquivos/NomeAtualizado2.txt
~$ cd Arquivos --Acessando o diretório para verificar o conteúdo
~/Arquivos$ ls
NomeAtualizado2.txt
```

**Copiando diretórios**<br>
O comando cp também pode ser utilizado para copiar diretórios, mas é recomendado especificar alguns parâmetros.
- -r: Copia o diretório e todo o seu conteúdo interno
- -v: Mostra no terminal quais arquivos/diretórios serão copiados
- -p: Preserva as permissões dos arquivos/diretórios originais.

```bash
$ cp -rvp novoDiretorio3 copiaNovoDiretorio3
'novoDiretorio3' -> 'copiaNovoDiretorio3'
'novoDiretorio3/sub' -> 'copiaNovoDiretorio3/sub'
'novoDiretorio3/sub/subsub' -> 'copiaNovoDiretorio3/sub/subsub'
```

#### **Renomear/mover arquivos**<br>
Muitas vezes, não queremos copiar um arquivo, mas sim visualizar/renomear o conteúdo de um arquivo. O comando `mv` é muito semelhante a estrutura do `cp`, pois tamém pede a origem e destino.
- O comando abaixo irá modificar o nome `Arquivos.txt` para `NomeAtualizado.txt`
```bash
~$ mv Arquivo.txt NomeAtualizado.txt
~$ ls
Arquivos   Downloads           Pictures   Videos               novoDiretorio2
Desktop    Music               Public     copiaNovoDiretorio3  novoDiretorio3
Documents  NomeAtualizado.txt  Templates  novoDiretorio        thinclient_drives
```


#### Apagar diretorios/arquivos

Para apagar um arquivo, podemos utilizar o comando `rm`, seguido do nome do arquivo que será deletado:
- No exemplo abaixo, vamos copiar o `Arquivo.txt` e, em seguida, deletar a cópia
```bash
~$ cp NomeAtualizado.txt CopiaArquivo.txt --Copuando um arquivo para deletar
~$ ls
Arquivos          Documents  NomeAtualizado.txt  Templates      novoDiretorio2
CopiaArquivo.txt  Downloads  Pictures            Videos         novoDiretorio3
Desktop           Music      Public              novoDiretorio  thinclient_drives

~$ rm CopiaArquivo.txt --Deletando o arquivo copiado
~$ ls
Arquivos  Documents  Music               Pictures  Templates  novoDiretorio   novoDiretorio3
Desktop   Downloads  NomeAtualizado.txt  Public    Videos     novoDiretorio2  thinclient_drives
```

> Além do `rm`, existe um comando utilizado para remover diretórios vazios, o `rmdir`. Porém, se houver algum conteúdo dentro, esse comando não vai conseguir deletar.

**Apagar diretórios inteiros**<br>
É recomendado utilizar alguns parâmetros ao apagar diretórios inteiros:
- r: Apaga o diretório e tudo que estiver dentro dele
- f: Ao apagar o diretório com o parâmrtro `r`, o terminal vai solicitar a permissão de exclusao para cada arquivo/diretorio. O parâmetro `f` vai forçar a remoção de todos
```bash
//ERRO POIS O DIRETORIO POSSUI CONTEUDO
~$ rmdir copiaNovoDiretorio3
rmdir: failed to remove 'copiaNovoDiretorio3': Directory not empty

~$ rm -rf copiaNovoDiretorio3/
~$ ls
Arquivos  Documents  Music               Pictures  Templates  novoDiretorio   novoDiretorio3
Desktop   Downloads  NomeAtualizado.txt  Public    Videos     novoDiretorio2  thinclient_drives
```

> 🚨Cuidado ao utilizar comandos de remoção: O comando `sudo rm -rf /` apaga o sistema operacional inteiro. Nunca utilize

## Visualizando o conteúdo de arquivos

>Para os exemplos dessa aula, iremos utilizar os arquivos de log do linux

Alguns comandos úteis para visualizar o conteúdo de arquivos

### cat - Exibe o contepudo de um arquivo (visualizar o conteudo todo)
O comando `cat` permite visualizar o conteúdo inteiro do arquivo especificado.
```bash
/$ cat /var/log/syslog

2025-06-05T15:09:57.793186+00:00 vm-linux-meus-estudos-east-us systemd[1]: Started networkd-dispatcher.service - Dispatcher daemon for systemd-networkd.
2025-06-05T15:09:57.797534+00:00 vm-linux-meus-estudos-east-us systemd[1]: Cannot find unit for notify message of PID 864, ignoring.

//ARQUIVO CONTINUA - MUITO GRANDE
```

### head - visualizar o inicio do arquivo
Em arquivos grandes como esse, é dificil visualizar uma informação específica. Para ajustar isso, podemos utilizar outros comandos

O comando `head` permite visualizar o cabeçalho do arquivo, ou seja, apenas algumas linhas do início. Além disso, podemos especificar o n° de linhas que desejamos visualizar através do parâmetro `n`
- O comando abaixo vai visualizar apenas as 5 primeiras linhas do arquivo
```bash
/$ head -n 5 /var/log/syslog

2025-06-04T18:22:27.211403+00:00 vm-linux-meus-estudos-east-us systemd-fsck[160]: cloudimg-rootfs: clean, 78071/327680 files, 436149/655099 blocks
2025-06-04T18:22:27.211504+00:00 vm-linux-meus-estudos-east-us systemd-modules-load[158]: Inserted module 'msr'
2025-06-04T18:22:27.211512+00:00 vm-linux-meus-estudos-east-us systemd-modules-load[158]: Inserted module 'dm_multipath'
2025-06-04T18:22:27.211517+00:00 vm-linux-meus-estudos-east-us systemd[1]: Mounted dev-hugepages.mount - Huge Pages File System.
2025-06-04T18:22:27.211520+00:00 vm-linux-meus-estudos-east-us systemd[1]: Mounted dev-mqueue.mount - POSIX Message Queue File System.
```

### tail - visualizar o final do arquivo
Quando se trata de um arquivo de log, é mais útil visualizar as última informações do que as primeiras. Podemos fazer isso com outro comando

A estrutura é exatamente a mesma do comando `head` (incluindo o n° de linhas que deseja visualizar). A unica diferença é que esse comando verifica o final do arquivo
- O comando abaixo vai visualizar apenas as 5 ultimas linhas do arquivo
```bash
/$ tail -n 5 /var/log/syslog

2025-06-09T16:00:15.702926+00:00 vm-linux-meus-estudos-east-us systemd[1]: Finished sysstat-collect.service - system activity accounting tool.
2025-06-09T16:05:01.749762+00:00 vm-linux-meus-estudos-east-us CRON[2124]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
2025-06-09T16:10:15.697623+00:00 vm-linux-meus-estudos-east-us systemd[1]: Starting sysstat-collect.service - system activity accounting tool...
2025-06-09T16:10:15.705434+00:00 vm-linux-meus-estudos-east-us systemd[1]: sysstat-collect.service: Deactivated successfully.
2025-06-09T16:10:15.705504+00:00 vm-linux-meus-estudos-east-us systemd[1]: Finished sysstat-collect.service - system activity accounting tool.
```

### less - navegar dentro de um arquivo grande
Em alguns casos, precisamos navegar por todo o arquivo, portanto, visualizar as primeiras/ultimas linhas do arquivo não serão de grande utilidade.

Para isso, podemos utilizar o comando `less`. Esse comando vai "bloquear" o terminal para que você possa navegar pelo conteúdo. Com isso, você pode navegar utilizando as teclas cima e baixo do teclado
- O comando abaixo vai navegar por todo o arquivo de log. Para sair do modo de nevageção, basta apertar a tecla `q`
```bash
/$ less /var/log/syslog

2025-06-04T18:22:27.211579+00:00 vm-linux-meus-estudos-east-us systemd[1]: modprobe@loop.service: Deactivated successfully.
2025-06-04T18:22:27.211583+00:00 vm-linux-meus-estudos-east-us systemd[1]: Finished modprobe@loop.service - Load Kernel Module loop.
2025-06-04T18:22:27.211586+00:00 vm-linux-meus-estudos-east-us systemd[1]: modprobe@nvme_fabrics.service: Deactivated successfully.
: 
```

## Localizar a baixar arquivos
Alguns comandos que permitem localizar arquivos/diretórios e até mesmo baixar arquivos da internet.

>Vamos utilizar o comando `locate` que por padrão não está presente no Linux. Portanto, utilize o comando `sudo apt install locate` para instalar essa biblioteca.

> Antes de mais nada, utilize o comando `sudo updatedb` para que ele atualize as refenrências de arquivos e diretórios que estão presentes no sistema.

### locate - localizar um arquivo>
Para localizar um arquivo, podemos utilizar o comnado `locate` seguido do nome do arquivo. Ele vai retonar o caminho absoluto do arquivo, se encontra-lo
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ locate NomeAtualizado.txt
/home/azureuser/NomeAtualizado.txt
```

### grep - localizar arquivos pelo conteudo
O comando `grep` permite localizar um arquivo a partir de seu conteúdo.
- O comando abaixo vai procurar arquivos que contenham a ocorerndo da palavra `pulseaudio`. Esses arquivos estãoo presentes no subdiretório `syslog`, mas nao precisamos especificar isso pois o comando consegue localiza-los
- Ele retorna a identificação do arquivo do qual a palavra-chave foi identificado, bem como o trecho.
```bash
/$ grep pulseaudio /var/log -r

/var/log/syslog:2025-06-05T15:10:06.568963+00:00 vm-linux-meus-estudos-east-us systemd[1256]: Listening on pulseaudio.socket - Sound System.
/var/log/syslog:2025-06-05T15:10:06.598059+00:00 vm-linux-meus-estudos-east-us systemd[1256]: Starting pulseaudio.service - Sound Service...
```

>Vale ressaltar que delimitamos a busca no diretório `var/log` (sem especificar o syslog que é onde os arquivos estão de fato). É possível fazer uma busca mais abragente sem delimitar nenhum diretório, por exemplo: `/$ grep pulseaudio / -r`. Porém, essa abordagem traz um resultado muito "sujo" por conta dos pseudoarquivos.

### wget - baixar um arquivo da internet
O comando `wget` permite fazer o download de qualquer arquivo através do link.
```bash
/$ wget https://git.kernel.org/torvalds/t/linux-4.16-rc2.tar.gz
```
- Ele tem uma particularidade interessante: Caso o download seja interrompido por qualquer motivo, é possível retoma-lo de onde parou. Para isso, utilizamos o parâmetro `-c`
```bash
/$ wget -c https://git.kernel.org/torvalds/t/linux-4.16-rc2.tar.gz
```

### links - encontrar links
Se estiver usando somenteo o terminal Linux, você nao tera uma interface grafica para buscar os links de arquivos desejados. Mas é possivel fazer isso pelo terminal.

>Para que isso funcione, é necessário instalar a biblioteca links com o comando: `sudo apt install links`

O comando `links` permite acessar sites pelo terminal, como se fosse um navegador sem interface gráfica.
- No caso do exemplo abaixo, vamos acessar o Google. A biblioteca links renderiza o site em texto para que podemos interagir pelo terminal.
- Para sair do site acessado, basta apertar a tecla `q`
```bash
/$ links www.google.com

--------------------SITE NO TERMINAL---------------------------
```