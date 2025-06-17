## Sobre o Linux

O Linux é um sistema operacional de código aberto, usado amplamente em servidores, computadores pessoais, dispositivos embarcados e muito mais. Ele é conhecido por sua estabilidade, segurança e flexibilidade. Aprender a usar o Linux é essencial para profissionais de TI, desenvolvedores e entusiastas da tecnologia, especialmente para quem trabalha com servidores, programação ou segurança da informação.

O terminal do Linux (linha de comando) é uma ferramenta poderosa que permite gerenciar todo o sistema de forma rápida e eficiente. A seguir, veja um resumo de comandos úteis para administração de usuários e pacotes no Linux.
Sem o conhecimento dos comandos abordados a seguir, o profissional fica 
extremamente limitado em relação às possibilidades de configuração deste sistema, além do conhecimento de como proteger seu ambiente computacional. 


## Por que aprender comandos ?

Em primeiro lugar, o terminal de comandos anteriormente conhecido como 
MS-DOS nunca foi abandonado pelo sistema Windows; na verdade, este sistema 
conta com uma nova versão deste terminal de comandos, o Power Shell. 

Existem situações nas quais o uso de um terminal de comandos se torna mais prático do que a interface gráfica. Por exemplo, as configurações do Microsoft Word.

Em um terminal de comandos, estas várias possibilidades de configuração do Microsoft Word seriam substituídas por um único comando com várias opções de parametrização, assim, desde que você saiba o parâmetro correto, poderia alterar a configuração rapidamente, enquanto na interface gráfica várias telas teriam que ser roladas até se localizar a configuração desejada. Talvez você indague, com uma certa razão, que decorar os parâmetros de um comando não é muito fácil. Sim, é verdade, mas você verá no decorrer deste capítulo que isso não é necessário, você aprenderá como procurar ajuda em caso de necessidade.

Também é fato que o sistema operacional Linux conta com belíssimos e eficazes softwares de gerenciamento de janelas (Windows Managers, vulgarmente conhecidos como interface gráfica), como é o caso do KDE, Gnome, XFCE, Window Maker e vários outros. No entanto, também é realidade que a maioria das implementações de servidor Linux **NÃO CONTA COM UMA INTERFACE GRÁFICA**. Na verdade, para muitos, a implementação de um servidor X e um gerenciador de janelas representa um imenso desperdício dos recursos do servidor.

O terminal de comandos será sempre a forma de interação com um kernel de sistema operacional mais rápida e leve que existe: não é necessária uma placa gráfica poderosa, grandes quantidades de memória RAM ou mesmo um monitor colorido. Na verdade, o terminal dispensa o uso de um monitor, possibilitando um acesso remoto através de SSH, cujo tráfego de dados será sempre infinitamente menor do que um TeamViewer, ou seja, nem sequer um bom link de dados é necessário para acessar o terminal de comandos de um servidor remoto.

## Comandos essenciais
Nesta seção, relacionamos os comandos básicos essenciais para se começar.

### man ou parâmetro --help

>Vale ressaltar que a maioria dos comandos linux podem ser parametrizados com a finalidade de modificar o seu comportamento. Os parâmetros são flags

O primeiro comando é o mais essencial de todos e uma lição para todos nós: **NÃO SABE, PEÇA AJUDA!** O comando `man` é a abreviação de manual, ou seja quando digito o comando `man` seguido do comando que desejo aprender, ele me mostra o manual completo dele: para que serve, quais são os parâmetros e, em alguns casos, até exemplos de uso!

Se eu desejar, por exemplo, saber como funciona o clássico comando “ls”, 
basta digitar: `man ls`
```bash
NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List information about the FILEs (the current directory by default).  Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

       Mandatory arguments to long options are mandatory for short options too.

       -a, --all
              do not ignore entries starting with .

       -A, --almost-all
              do not list implied . and ..

       --author
              with -l, print the author of each file

       -b, --escape
              print C-style escapes for nongraphic characters

       --block-size=SIZE
              with -l, scale sizes by SIZE when printing them; e.g., '--block-size=M'; see SIZE format below

       -B, --ignore-backups
              do not list implied entries ending with ~

       -c     with -lt: sort by, and show, ctime (time of last change of file status information); with -l: show ctime and sort by name; otherwise: sort by ctime, newest first

       -C     list entries by columns

       --color[=WHEN]
              color the output WHEN; more info below

       -d, --directory
              list directories themselves, not their contents

       -D, --dired
              generate output designed for Emacs' dired mode

       -f     list all entries in directory order

       -F, --classify[=WHEN]
              append indicator (one of */=>@|) to entries WHEN

       --file-type
              likewise, except do not append '*'

       --format=WORD
              across -x, commas -m, horizontal -x, long -l, single-column -1, verbose -l, vertical -C
```

- su
    - Esse usuário tem mais privilégios

### Gerenciamento de pacotes
`apt install sudo`
- Descrição: Instala o pacote sudo.
- Por que usar? Necessário para permitir que usuários não-root executem comandos como superusuário.

``apt remove sudo``
- Descrição: Remove o pacote sudo.
- Cuidado: Sem sudo, usuários comuns perdem a capacidade de executar comandos administrativos facilmente.

`apt update`
- Descrição: Atualiza a lista de pacotes disponíveis nos repositórios.
- Por que usar? Mantém o sistema informado sobre as versões mais recentes de software.

`apt upgrade`
- Descrição: Atualiza todos os pacotes instalados para suas versões mais recentes disponíveis.
- Importância: Garante que o sistema esteja atualizado e seguro.

### Superuser

Antes de avançarmos para alguns comandos importantes do Linux, vamos a uma breve explicação sobre um conceito imporatnte do Linux, o superuser.

No Linux, o superuser (ou usuário root) é o usuário com permissão total no sistema. Ele pode fazer qualquer coisa, como:
- Instalar ou remover programas
- Criar, alterar ou excluir qualquer arquivo
- Gerenciar usuários e permissões
- Configurar o sistema inteiro

É como ser o administrador principal do sistema.

#### Por que isso importa?
Por segurança, você não deve usar o superuser o tempo todo, porque um erro (como deletar arquivos do sistema) pode quebrar o Linux. Por isso, usamos o comando sudo, que permite executar apenas um comando como superuser, de forma mais segura.

`su`
- Descrição: Muda para o usuário root (administrador do sistema).
- Por que usar? Permite executar comandos com permissões administrativas.
Obs: Requer a senha do root.


### sudo - Rodando comandos como se fosse o superuser
Tornar-se o usuário root e voltar a ser seu próprio usuário o tempo todo pode ser cansativo e, por descuido, o usuário pode ficar como root tempo demais. O comando sudo pode ser útil nestes casos. 

A primeira coisa é instalar o sudo com o comando # apt install sudo como o usuário root.

A sintaxe é `$ sudo [comando que desejo rodar com privilégios]`; o terminal solicitará a senha do seu próprio usuário e, caso ele seja membro do grupo sudo, o comando será executado com os privilégios de superusuário.

>O procedimento é mais seguro do que tornar-se root o tempo todo com o 
comando su, neste caso, os privilégios são usados pontualmente, conforme a 
necessidade.

<br>


## Gerenciamento de usuários

Os comandos a seguir permitem realizar algumas operações como trocar a 
senha do usuário, criar um usuário ou grupo e modificar usuários existentes. 

### passwd - Modificar a senha do usuário

Para modificar a senha do usuário, o comando passwd (abreviação da 
palavra inglesa password ou senha) pode ser utilizado. Ao digitar passwd no 
terminal, este, por sua vez, solicita a senha atual e, posteriormente, a nova senha e a confirmação da nova senha

>Caso o usuário não saiba a própria senha, é possível passar o nome de 
usuário logo após o comando passwd e trocar a senha de outro usuário; se você for o usuário root, a senha atual não será solicitada. Sendo assim, `$ sudo passwd root` pode ser usado para trocar a senha do próprio superusuário

### useradd - Adicionar um usuário

Quando for necessário criar um usuário no sistema Linux, o comando useradd deve ser utilizado. Utilize os parâmetros “`-d`” para informar o diretório 
pessoal deste usuário, e “`-p`” para informar a senha (embora você possa rodar o comando `$ sudo passwd [usuario]` logo na sequência).

>Embora não fique explícito, por padrão o comando useradd cria um grupo de 
mesmo nome que o usuário e o coloca como membro. Procure outras informações sobre o comando com `man useradd`, pois existem parâmetros para criar usuários temporários, adicioná-lo previamente em outros grupos

### groupadd - Adicionar um grupo

O comando `groupadd` permite criar um grupo de usuários. Para tal, basta 
digitar o comando groupadd acompanhado do nome deste novo grupo (e sudo, pois criar grupos precisa de privilégio especial).


### usermod - Modificando um usuário
Embora seja possível alterar os arquivos /etc/passwd (configurações do 
usuário), /etc/shadow (as senhas dos usuários) e /etc/group (configurações de 
grupo) com os privilégios de um root, a prática não é recomendada, pois qualquer 
falha na alteração destes arquivos pode comprometer o acesso dos usuários ao 
sistema

Sendo assim, o comando `usermod` (de user modification ou modificação de 
usuário) e os parâmetros “`-aG`” são geralmente usados em conjunto e permitem 
adicionar o usuário em novos grupos sem, no entanto, retirá-lo dos grupos dos quais 
já faz parte. 

O parâmetro “`-d`” permite estabelecer um novo diretório pessoal para o 
usuário, “`-L`” (de lock) bloqueia um usuário, enquanto “`-U`” (de unlock) pode 
desbloqueá-lo facilmente. O uso de “`-e`” permite estabelecer uma data de expiração 
para a conta do usuário, que é automaticamente bloqueada após esta data.


```bash
azureuser@vm-linux-meus-estudos-east-us:~$ usermod --help
Usage: usermod [options] LOGIN

Options:
  -a, --append                  append the user to the supplemental GROUPS
                                mentioned by the -G option without removing
                                the user from other groups
  -b, --badname                 allow bad names
  -c, --comment COMMENT         new value of the GECOS field
  -d, --home HOME_DIR           new home directory for the user account
  -e, --expiredate EXPIRE_DATE  set account expiration date to EXPIRE_DATE
  -f, --inactive INACTIVE       set password inactive after expiration
                                to INACTIVE
  -g, --gid GROUP               force use GROUP as new primary group
  -G, --groups GROUPS           new list of supplementary GROUPS
  -h, --help                    display this help message and exit
  -l, --login NEW_LOGIN         new value of the login name
  -L, --lock                    lock the user account
  -m, --move-home               move contents of the home directory to the
                                new location (use only with -d)
  -o, --non-unique              allow using duplicate (non-unique) UID
  -p, --password PASSWORD       use encrypted password for the new password
  -P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files
  -r, --remove                  remove the user from only the supplemental GROUPS
                                mentioned by the -G option without removing
                                the user from other groups
  -R, --root CHROOT_DIR         directory to chroot into
  -s, --shell SHELL             new login shell for the user account
  -u, --uid UID                 new UID for the user account
  -U, --unlock                  unlock the user account
  -v, --add-subuids FIRST-LAST  add range of subordinate uids
  -V, --del-subuids FIRST-LAST  remove range of subordinate uids
  -w, --add-subgids FIRST-LAST  add range of subordinate gids
  -W, --del-subgids FIRST-LAST  remove range of subordinate gids
  -Z, --selinux-user SEUSER     new SELinux user mapping for the user account

```

<br>

--------------------------------------
