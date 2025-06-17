# Recursos Avançados

Conforme prometido em nosso último capítulo sobre os comandos Linux, 
vamos abordar agora a possibilidade de gerenciar volumes, agendar tarefas e filtrar 
pacotes (a instalação de firewall, incluindo port-knocking) no sistema operacional do 
Linux.

## Gerenciamento de volumes

O gerenciamento de volumes para armazenamento é um conhecimento 
importante para qualquer administrador Linux. Cada disco rígido, seja HDD ou SSD, 
pendrive, HD externo ou disco óptico (CD-ROMs, DVD-ROMs ou Blurays) são 
tratados e identificados pelo sistema como volumes e, diferente do que estamos 
acostumados em um sistema operacional Windows, do qual, cada um dos volumes é 
identificado com uma letra e possui sua própria raiz de arquivos e diretórios, em 
sistemas do tipo *nix, esta raiz é única e os diferentes volumes serão “pendurados” 
nesta árvore no ponto que acharmos mais conveniente. 

### Partições

Vamos falar um pouco de como estas unidades de armazenamento são 
divididas. Um único dispositivo físico como um disco rígido ou pendrive pode ser 
dividido em várias partes do ponto de vista lógico, ou seja, por software; as razões 
para isso podem ser diversas: maior organização, maior segurança (pois é possível 
estabelecer regras de acesso distintas para diferentes partições) ou mesmo 
performance (algumas operações realizadas pelos sistemas de arquivos são mais 
velozes em partições menores do que maiores). 

Outra boa razão para dividir um disco rígido em diferentes partições é a 
possibilidade de se instalar sistemas operacionais diferentes no mesmo hardware, 
ou seja, se temos Windows e Linux em um mesmo equipamento (e disco rígido) 
instalados diretamente no equipamento físico (não estamos falando de máquinas 
virtuais neste caso), é graças à possibilidade de particionar volumes de 
armazenamento.

### Sistena de arquivos

Uma vez criada(s) a(s) partição(ões) de um volume de armazenamento, faz
se necessário gerenciar como esta informação ficará armazenada. Por exemplo, em 
um disco rígido tradicional, formado por discos empilhados e agulhas de leitura, temos cilindros, setores e cabeças de leitura diferentes; 
dependendo de onde a informação está armazenada, e velocidade de acesso ou 
mesmo gravação da informação será diferente. Além disso, se desejo gravar um 
arquivo grande de, digamos, 5GB e não há 5GB continuamente livres no disco, este 
arquivo será quebrado em partes que ocuparão lugares diferentes neste disco. 

Quem organiza a estratégia de leitura e gravação em um disco, além de criar 
os conceitos de arquivos e diretórios para organizar as informações é um 
componente importante do sistema operacional chamado sistema de arquivos. O sistema de arquivos é parte de um sistema operacional e trata-se desta camada 
que é instalada nas partições, transformando arquivos e diretórios em blocos de 
informação e vice-versa. 

Existem dezenas de sistemas de arquivos diferentes. No caso do Windows, 
os mais comuns são o FAT32 (sigla para File AllocationTable ou Tabela de Alocação 
de Arquivos), seguido pelo sucessorexFAT (Extended File AllocationTable ou Tabela 
de Alocação de Arquivos Estendida ou ainda FAT64) e o NTFS (New Technology File System). O NTFS é o sistema de arquivos da Microsoft mais seguro entre as 
três opções e as versões mais atuais do Windows serão sempre instaladas em uma 
participação com NTFS. Embora possua uma série de desvantagens e restrições à 
segurança, o FAT32 é um sistema de arquivos fácil de ser implementado e 
embarcado, sendo reconhecido pelos mais diferentes periféricos como SmartTVs, 
sons automotivos e outros dispositivos que façam leitura de pendrives através de 
suas portas USB. 

No sistema operacional Linux ou ext4, é a quarta versão de sistema de 
arquivos e a mais utilizada pelo sistema. No entanto, temos outras possibilidades 
como o ReiserFS, Btrfs, XFS, ZFS entre dezenas de opções.


### fdisk - Gerenciando volums

Criamos um volume de armazenamento em nosso Oracle VirtualBox que 
simulará a instalação física de um novo disco rígido, vamos aprender a criar 
partições, este volume que se encontra virgem para gravação. 

No Linux, existe o comando `fdisk` que permite gerenciar os volumes. Com esse comando, podemos criar e destruir partições, ou seja, fazer esse gerenciamento do volume físico definindo como vai ser a divisão de partições no modelo lógico.

>O comando fdisk também existe no Windows

Mas antes de aprofundarmos nesse comando, vamos entender aonde os volumes são armazenados.

Como foi dito, qualquer periférico detectado pelo sistema operacional Linux se 
torna um pseudo arquivo no diretório `/dev`. Geralmente, discos rígidos são identificados como `/dev/sd*`, sendo `/dev/sda` o primeiro disco encontrado, `/dev/sdb` o segundo, assim por diante.

#### Verificando o novo disco
Podemos verificar o `/var/log/syslog` do Linux para identificar se o novo disco foi identificaod.

Primeiro, vamos buscar por `sda`, que é o 1° disco rigido (onde o Linux esta instalado):
- Vemos que o disco foi detectado no sistema
- Além disso, vemos que as partições também foram identificadas (destaquei no exemplo abaixo)
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ sudo cat /var/log/syslog | grep sda
2025-06-04T18:22:27.211750+00:00 vm-linux-meus-estudos-east-us (udev-worker)[237]: sda: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/sda' failed with exit code 1.
2025-06-04T18:22:27.211759+00:00 vm-linux-meus-estudos-east-us (udev-worker)[226]: sda14: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/sda14' failed with exit code 1.
2025-06-04T18:22:27.211763+00:00 vm-linux-meus-estudos-east-us (udev-worker)[222]: sda16: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/sda16' failed with exit code 1.
2025-06-04T18:22:27.211772+00:00 vm-linux-meus-estudos-east-us (udev-worker)[224]: sda15: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/sda15' failed with exit code 1.
2025-06-04T18:22:27.211776+00:00 vm-linux-meus-estudos-east-us (udev-worker)[237]: sda1: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/sda1' failed with exit code 1.
2025-06-04T18:22:27.211825+00:00 vm-linux-meus-estudos-east-us systemd-fsck[415]: /dev/sda15: 12 files, 12499/213663 clusters
2025-06-04T18:22:27.212621+00:00 vm-linux-meus-estudos-east-us kernel: sd 0:0:0:0: [sda] 62916608 512-byte logical blocks: (32.2 GB/30.0 GiB)
2025-06-04T18:22:27.212622+00:00 vm-linux-meus-estudos-east-us kernel: sd 0:0:0:0: [sda] 4096-byte physical blocks
2025-06-04T18:22:27.212628+00:00 vm-linux-meus-estudos-east-us kernel: sd 0:0:0:0: [sda] Write Protect is off
2025-06-04T18:22:27.212629+00:00 vm-linux-meus-estudos-east-us kernel: sd 0:0:0:0: [sda] Mode Sense: 0f 00 10 00
2025-06-04T18:22:27.212635+00:00 vm-linux-meus-estudos-east-us kernel: sd 0:0:0:0: [sda] Write cache: enabled, read cache: enabled, supports DPO and FUA
## ESSA LINHA ABAIXO MOSTRA AS PARTIÇÕES
2025-06-04T18:22:27.212657+00:00 vm-linux-meus-estudos-east-us kernel:  sda: sda1 sda14 sda15 sda16
```
Ao buscar por `sdc` no `/var/log/syslog` ou no diretório `/dev`, tenho os seguintes resultados:
- Vemos que o disco foi identificado, mas ainda não possui nenhuma partição
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ sudo cat /var/log/syslog | grep sdc
2025-06-11T18:30:49.127181+00:00 vm-linux-meus-estudos-east-us kernel: sd 0:0:0:1: [sdc] 16777216 512-byte logical blocks: (8.59 GB/8.00 GiB)
2025-06-11T18:30:49.127181+00:00 vm-linux-meus-estudos-east-us kernel: sd 0:0:0:1: [sdc] 4096-byte physical blocks
2025-06-11T18:30:49.127186+00:00 vm-linux-meus-estudos-east-us kernel: sd 0:0:0:1: [sdc] Write Protect is off
2025-06-11T18:30:49.127187+00:00 vm-linux-meus-estudos-east-us kernel: sd 0:0:0:1: [sdc] Mode Sense: 0f 00 10 00
2025-06-11T18:30:49.127193+00:00 vm-linux-meus-estudos-east-us kernel: sd 0:0:0:1: [sdc] Write cache: disabled, read cache: enabled, supports DPO and FUA
2025-06-11T18:30:49.127214+00:00 vm-linux-meus-estudos-east-us kernel:  sdc: sdc1
2025-06-11T18:30:49.127221+00:00 vm-linux-meus-estudos-east-us kernel: sd 0:0:0:1: [sdc] Attached SCSI disk
2025-06-11T18:30:49.127493+00:00 vm-linux-meus-estudos-east-us kernel: EXT4-fs (sdc1): mounted filesystem 01072ba3-a010-4cc3-81ec-a8ed111450ad r/w with ordered data mode. Quota mode: none.
2025-06-11T18:30:49.130272+00:00 vm-linux-meus-estudos-east-us (udev-worker)[239]: sdc: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/sdc' failed with exit code 1.
2025-06-11T18:30:49.130284+00:00 vm-linux-meus-estudos-east-us (udev-worker)[227]: sdc1: Process '/usr/bin/unshare -m /usr/bin/snap auto-import --mount=/dev/sdc1' failed with exit code 1.
2025-06-11T18:30:49.130358+00:00 vm-linux-meus-estudos-east-us systemd-fsck[428]: sdc1: fsck.ntfs doesn't exist, not checking file system.
2025-06-11T18:30:49.130934+00:00 vm-linux-meus-estudos-east-us ntfs-3g[706]: Mounted /dev/sdc1 (Read-Only, label "Temporary Storage", NTFS 3.1)
2025-06-11T18:30:49.130942+00:00 vm-linux-meus-estudos-east-us ntfs-3g[706]: Mount options: ro,allow_other,nonempty,relatime,fsname=/dev/sdc1,blkdev,blksize=4096
2025-06-11T18:30:49.130954+00:00 vm-linux-meus-estudos-east-us ntfs-3g[706]: Unmounting /dev/sdc1 (Temporary Storage)
2025-06-11T18:30:54.634168+00:00 vm-linux-meus-estudos-east-us python3[1208]: 2025-06-11T18:30:54.633953Z INFO EnvHandler ExtHandler Set block dev timeout: sdc with timeout: 300
```

#### Examinando os discos

>O fdisk tem um prompt esclusivo com comando particulares. Ao utilizar o comando, podemos acessar o seu manual pressionando `m`
```bash
Welcome to fdisk (util-linux 2.39.3).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS (MBR) disklabel with disk identifier 0x672a8c6f.

Command (m for help): m

Help:

  DOS (MBR)
   a   toggle a bootable flag
   b   edit nested BSD disklabel
   c   toggle the dos compatibility flag

  Generic
   d   delete a partition
   F   list free unpartitioned space
   l   list known partition types
   n   add a new partition
   p   print the partition table
   t   change a partition type
   v   verify the partition table
   i   print information about a partition

  Misc
   m   print this menu
   u   change display/entry units
   x   extra functionality (experts only)

  Script
   I   load disk layout from sfdisk script file
   O   dump disk layout to sfdisk script file

  Save & Exit
   w   write table to disk and exit
   q   quit without saving changes

  Create a new label
   g   create a new empty GPT partition table
   G   create a new empty SGI (IRIX) partition table
   o   create a new empty MBR (DOS) partition table
   s   create a new empty Sun partition table

Command (m for help):
```

Vamos visualizar o disco primário através do comando `sudo fdisk /dev/sda`
- Para visualizar as partições, utilize o comando `p`
- Para sair do comando `fdisk`, utilize o comando `q`

Podemos visualizar que esse disco já possui partições criadas, vamos explicar um pouco:
- `sda1`:
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ sudo fdisk /dev/sda

Welcome to fdisk (util-linux 2.39.3).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

This disk is currently in use - repartitioning is probably a bad idea.
It's recommended to umount all file systems, and swapoff all swap
partitions on this disk.


Command (m for help): p

Disk /dev/sda: 30 GiB, 32213303296 bytes, 62916608 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: 42384FD8-FCBB-4218-866C-F11E4647EE49

Device       Start      End  Sectors  Size Type
/dev/sda1  2099200 62916574 60817375   29G Linux filesystem
/dev/sda14    2048    10239     8192    4M BIOS boot
/dev/sda15   10240   227327   217088  106M EFI System
/dev/sda16  227328  2097152  1869825  913M Linux extended boot

Partition table entries are not in disk order.

Command (m for help):
```

Podemos reparar nas colunas três e quatro (respectivamente “Início” e “Fim”) o bloco inicial e final de cada partição, e destacamos que:
- A primeira partição não começa no primeiro setor do disco; ela inicia sempre em 2048. Isso ocorre porque os setores iniciais são reservados para a tabela de partições (onde estão os dados sobre todas as partições presentes no disco). Em discos com GPT, essa tabela ocupa os primeiros setores e não o MBR (Master Boot Record), que era usado em sistemas mais antigos. Em sistemas com MBR, esses setores iniciais poderiam conter informações sobre múltiplos sistemas operacionais e permitir a escolha de qual iniciar, mas no caso de GPT, as partições podem ser iniciadas diretamente após o setor 2048. 


- As partições `/dev/sda14` e `/dev/sda15` são do tipo EFI System e BIOS Boot, com tamanhos e setores bastante distintos. A partição `/dev/sda14` é do tipo BIOS Boot (necessária para inicialização em sistemas que não utilizam UEFI), enquanto a partição /dev/sda15 é do tipo EFI System, crucial para inicialização em sistemas UEFI.


- A partição `/dev/sda1` é a partição principal onde o sistema operacional Linux está instalado. Ela ocupa o maior espaço no disco, com 29 GB, e é onde o sistema de arquivos está montado, contendo arquivos do sistema e dados do usuário. Já a `/dev/sda16` é uma partição Linux extended boot, usada para armazenar dados de inicialização do sistema operacional. Não vemos aqui uma partição de swap explícita, o que pode significar que o sistema não usa uma partição swap ou a utiliza em formato de arquivo swap (em vez de uma partição dedicada).

Agora, visualizando as partições de um disco novo, dentro do comando `fdisk /dev/sdc`
- Podemos visualizar que nesse disco possui partição
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ sudo fdisk /dev/sdc

Welcome to fdisk (util-linux 2.39.3).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS (MBR) disklabel with disk identifier 0x484409b2.

Command (m for help): p
Disk /dev/sdc: 4 GiB, 4294967296 bytes, 8388608 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: dos
Disk identifier: 0x484409b2

Command (m for help):
```

>Caso queira saber qual sistema de arquivos o Linux está usando, o arquivo `/etc/mtab` mostra a tabela de todos os volumes de armazenamento ativos neste momento, juntamente com seus sistemas de arquivos e regras

Logo depois do dispositivo (/dev/sda1) temos o que é chamado de ponto de montagem, ou seja, onde aquele volume está ativo na raiz de arquivos; a resposta para /dev/sda1 é “/”, ou seja, é a raiz de nossa árvore e, por esta razão, é o que deve ser montado primeiro; todos os outros dispositivos serão “pendurados” ou montados na árvore iniciada por /dev/sda1. 

#### Criando uma partição

Agora que sabemos mais sobre partições, vamos usar o comando fdisk para criar duas partições em /dev/sdb. 

Para tal, comece com o comando `sudo fdisk /dev/sdc` e no menu do comando, digite `n` (de new, para criar uma partição) e seguir as instruções:
 - Ao pressionar a tecla `n`, ele pergunta qual o tipo de partição será criada (nesse caso, iremos criar partições primárias.):
```bash
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
```

- Em seguida, precisamos informar o n° da partição. Como é a primeira partição, vamos usar o `1` (ou simplesmente pressione `enter` para aceitar o padrão):
```bash
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
```

- Agora, ele solicita a numeração do *primeiro setor*. Nesse caso, podemos dar `enter` e deixar como está
```bash
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-8388607, default 2048):
```

- Por fim, ele solicita a numeração do *ultimo setor*. Aqui, podemos utilizar o setor ou o  tamanho como referência
  - Nesse caso, vamos especificar que essa partição terá 2G (o disco tem um total de 4G, portanto a partição vai ocupar metade). Podemos fazer isso digitando `+2G` 
```bash
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-8388607, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-8388607, default 8388607): +2G

Created a new partition 1 of type 'Linux' and of size 2 GiB.
```

Podemos utilizar o comando `p` novamente para verificar que a partição foi criada corretamente:
```bash
Command (m for help): p
Disk /dev/sdc: 4 GiB, 4294967296 bytes, 8388608 sectors
Disk model: Virtual Disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: dos
Disk identifier: 0x484409b2

Device     Boot Start     End Sectors Size Id Type
/dev/sdc1        2048 4196351 4194304   2G 83 Linux
```

>Para fins de teste, criamos a partição 2, de 2G para aproveitar o restante do disco.
```bash
Device     Boot   Start     End Sectors Size Id Type
/dev/sdc1          2048 4196351 4194304   2G 83 Linux
/dev/sdc2       4196352 8388607 4192256   2G 83 Linux
```

Por padrão, as duas partições foram criadas como tipo “Linux” assim, espera
se que se use sistemas de arquivo como o ext4 ou ReiserFS. Caso você queira criar 
partições para outros fins ou sistemas operacionais, a letra “t” seguida da letra “L” 
permite mudar o tipo facilmente.

>Com o comando `l` podemos listar os diversos tipos possíveis. Abaixo, uma lista com os tipos de partição:
```bash
Command (m for help): l

00 Empty            27 Hidden NTFS Win  82 Linux swap / So  c1 DRDOS/sec (FAT-
01 FAT12            39 Plan 9           83 Linux            c4 DRDOS/sec (FAT-
02 XENIX root       3c PartitionMagic   84 OS/2 hidden or   c6 DRDOS/sec (FAT-
03 XENIX usr        40 Venix 80286      85 Linux extended   c7 Syrinx
04 FAT16 <32M       41 PPC PReP Boot    86 NTFS volume set  da Non-FS data
05 Extended         42 SFS              87 NTFS volume set  db CP/M / CTOS / .
06 FAT16            4d QNX4.x           88 Linux plaintext  de Dell Utility
07 HPFS/NTFS/exFAT  4e QNX4.x 2nd part  8e Linux LVM        df BootIt
08 AIX              4f QNX4.x 3rd part  93 Amoeba           e1 DOS access
09 AIX bootable     50 OnTrack DM       94 Amoeba BBT       e3 DOS R/O
0a OS/2 Boot Manag  51 OnTrack DM6 Aux  9f BSD/OS           e4 SpeedStor
0b W95 FAT32        52 CP/M             a0 IBM Thinkpad hi  ea Linux extended
0c W95 FAT32 (LBA)  53 OnTrack DM6 Aux  a5 FreeBSD          eb BeOS fs
0e W95 FAT16 (LBA)  54 OnTrackDM6       a6 OpenBSD          ee GPT
0f W95 Ext'd (LBA)  55 EZ-Drive         a7 NeXTSTEP         ef EFI (FAT-12/16/
10 OPUS             56 Golden Bow       a8 Darwin UFS       f0 Linux/PA-RISC b
11 Hidden FAT12     5c Priam Edisk      a9 NetBSD           f1 SpeedStor
12 Compaq diagnost  61 SpeedStor        ab Darwin boot      f4 SpeedStor
14 Hidden FAT16 <3  63 GNU HURD or Sys  af HFS / HFS+       f2 DOS secondary
16 Hidden FAT16     64 Novell Netware   b7 BSDI fs          f8 EBBR protective
17 Hidden HPFS/NTF  65 Novell Netware   b8 BSDI swap        fb VMware VMFS
18 AST SmartSleep   70 DiskSecure Mult  bb Boot Wizard hid  fc VMware VMKCORE
1b Hidden W95 FAT3  75 PC/IX            bc Acronis FAT32 L  fd Linux raid auto
1c Hidden W95 FAT3  80 Old Minix        be Solaris boot     fe LANstep
1e Hidden W95 FAT1  81 Minix / old Lin  bf Solaris          ff BBT
24 NEC DOS

Aliases:
   linux          - 83
   swap           - 82
   extended       - 05
   uefi           - EF
   raid           - FD
   lvm            - 8E
   linuxex        - 85

Command (m for help):
```

**Salvando as partições**<br>
Para salvar as alterações e partições criadas, pressione `w`:
```bash
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

## mkfs - Criando sistema de arquivos

Embora o volume `sdc` possua duas partições de 2GB do tipo Linux, estas não 
possuem nenhum sistema de arquivos que nos permita criar arquivos ou diretórios. 
Para tal, podemos usar o comando `mkfs` (abreviação de makefilesystem ou criar 
sistema de arquivo). Além disso, podemos seguir essa estrutura:
- `sudo mkfs -t <tipo_sistema_arquivo> /<particao>`

No nosso caso, vamos criar um sistema de arquivos do tipo `ext4` na partição `sdc1`, localizada em `/dev/sdc1`. Sendo assim, o comadno inteiro fica
- `sudo mkfs -t ext4 /dev/sdc1`

```bash
azureuser@vm-linux-meus-estudos-east-us:~$ sudo mkfs -t ext4 /dev/sdc1
mke2fs 1.47.0 (5-Feb-2023)
Discarding device blocks: done
Creating filesystem with 524288 4k blocks and 131072 inodes
Filesystem UUID: 08dee615-8525-451f-a5e7-a86e3e7f266d
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done
```

>Ao digitar `sudo mkfs.` e a tecla `TAB` podemos ver as opções de sistemas de arquivo disponíveis no Linux que estamos. Podemos utilizar `mkfs.<tipo>` para especificar o tipo do sistema de arquivos, sem a necessidade de utilizar o parâmetro `-t`:
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ sudo mkfs.
mkfs.bfs     mkfs.cramfs  mkfs.ext3    mkfs.fat     mkfs.msdos   mkfs.vfat
mkfs.btrfs   mkfs.ext2    mkfs.ext4    mkfs.minix   mkfs.ntfs    mkfs.xfs

##Criando sistema de arquivos sem o parametro -t
azureuser@vm-linux-meus-estudos-east-us:~$ sudo mkfs.ext4 /dev/sdc2
mke2fs 1.47.0 (5-Feb-2023)
Discarding device blocks: done
Creating filesystem with 524032 4k blocks and 131072 inodes
Filesystem UUID: bc377c01-d95e-4779-9a8f-970c8fca01e5
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912

Allocating group tables: done
Writing inode tables: done
Creating journal (8192 blocks): done
Writing superblocks and filesystem accounting information: done
```

>Vale ressaltar que o comando `mkfs` pode ser utilizado para formatar partições pré-existentes.

### Montar, Desmontar e Montagem Automática de volumes

Diferente do Windows, onde cada volume é uma própria raiz de diretórios, no Linux temos uma única árvore de diretórios da qual os outros volumes ficarão "pendurados" nela.

#### Mount - Montar

- O comando `mount` é utilizado para montar as partições na raiz de diretórios. 


Para tal, basta indicar a `partição` e o ponto de montagem, ou seja, a partir de qual 
diretório queremos que o volume atue.
- Não é recomendado realizar a montagem na raiz `/`, pois é onde o sda1 atua.
- No Linux, existem alguns diretórios padrão para realizar montagens, como: `/media` e `/mnt`. Neles, podemos criar subdiretórios para armazenar a montagem
  - Criamos um subdiretório em `/media/novaParticao`

Com isso, podemos executar o comando `mount`
- `sudo mount /dev/sdc1 /media/novaParticao`
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ sudo mount /dev/sdc1 /media/novaParticao
```



>O comando mount conta com um parâmetro “-o” da qual é possível parametrizar a montagem do dispositivo

**Visualizar os volumes**<br>

- O comando `df` que, com seu parâmetro `-h`, permite visualizar o tamanho dos volumes de uma maneira mais amigável.
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ sudo df -h
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           2.0G     0  2.0G   0% /dev/shm
tmpfs           784M  1.1M  783M   1% /run
tmpfs           5.0M     0  5.0M   0% /run/lock
efivarfs        128M   26K  128M   1% /sys/firmware/efi/efivars
/dev/sda16      881M   60M  760M   8% /boot
/dev/sda15      105M  6.2M   99M   6% /boot/efi
/dev/sdb1       7.8G   28K  7.4G   1% /mnt
tmpfs           392M  100K  392M   1% /run/user/112
tmpfs           392M   96K  392M   1% /run/user/1000
/dev/sdc1       2.0G   24K  1.8G   1% /media/novaParticao
``` 
Podemos observar também que, embora a partição `/dev/sdc1` de 2GB nunca tenha sido usada, esta começa com alguma ocupação (16M ou dezesseis megabytes) e nos permite a utilização de 1,8 gigabytes. Isso acontece, pois, o próprio sistema de arquivos precisa de um percentual desta partição para funcionar adequadamente, e por esta razão que nunca conseguiu usar todo o espaço que a embalagem que seu pendrive diz que tem à disposição. 

> Vamos copiar o `syslog` para essa volume para verificar o aumento no número de bytes utilizados:
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ sudo cp /var/log/syslog /media/novaParticao/

azureuser@vm-linux-meus-estudos-east-us:~$ sudo df -h
sudo: unable to resolve host vm-linux-meus-estudos-east-us: Temporary failure in name resolution
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           2.0G     0  2.0G   0% /dev/shm
tmpfs           784M  1.1M  783M   1% /run
tmpfs           5.0M     0  5.0M   0% /run/lock
efivarfs        128M   26K  128M   1% /sys/firmware/efi/efivars
/dev/sda16      881M   60M  760M   8% /boot
/dev/sda15      105M  6.2M   99M   6% /boot/efi
/dev/sdb1       7.8G   28K  7.4G   1% /mnt
tmpfs           392M  100K  392M   1% /run/user/112
tmpfs           392M   96K  392M   1% /run/user/1000
/dev/sdc1       2.0G   92K  1.8G   1% /media/novaParticao
azureuser@vm-linux-meus-estudos-east-us:~$
```

#### Ummount - Desmontar
O comando `umount` (que vem da palavra unmount ou desmontagem) permite desmontar um volume da raiz de diretórios. Para tal, basta informar a partição ou ponto de montagem (diretório onde a montagem foi realizada), como pode ser visto abaixo:
- `sudo umount /media/novaParticao` - Pelo ponto de montagem
- `sudo umount /dev/sdc1` - Pela partição

No nosso exemplo, vamos utilizar a partir da partição:
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ sudo umount /dev/sdc1
```

Podemos utilizar o comando `df -h` para visualizar os volumes novamente:
- Repare que agora o volume `/dev/sdc1` não está presente
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ sudo df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        29G  3.5G   25G  13% /
tmpfs           2.0G     0  2.0G   0% /dev/shm
tmpfs           784M  1.1M  783M   1% /run
tmpfs           5.0M     0  5.0M   0% /run/lock
efivarfs        128M   26K  128M   1% /sys/firmware/efi/efivars
/dev/sda16      881M   60M  760M   8% /boot
/dev/sda15      105M  6.2M   99M   6% /boot/efi
/dev/sdb1       7.8G   28K  7.4G   1% /mnt
tmpfs           392M  100K  392M   1% /run/user/112
tmpfs           392M   96K  392M   1% /run/user/1000
```

#### Montagem automática
Embora o comando mount nos permita a montagem de partições com facilidade, na maioria dos casos precisamos que esta montagem aconteça automaticamente.

Quando queremos que certas partições sejam montadas no boot do sistema operacional, basta informá-la no arquivo `/etc/fstab`. Vamos visualizar o arquivo com o comando `cat`:
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ cat /etc/fstab
# CLOUD_IMG: This file was created/modified by the Cloud Image build process
UUID=295c2e8c-18f3-4bce-992a-c9ec672e2297       /        ext4   discard,commit=30,errors=remount-ro  0 1
LABEL=BOOT      /boot   ext4    defaults,discard        0 2
UUID=1182-4527  /boot/efi       vfat    umask=0077      0 1
/dev/disk/cloud/azure_resource-part1    /mnt    auto    defaults,nofail,x-systemd.after=cloud-init.service,_netdev,comment=cloudconfig        0       2
``` 

Sendo: 
- **Primeira coluna – partição a ser montada**: a primeira informação é a  partição que será montada. Em nosso caso, pode ser /dev/sdb1; repare  que os dois exemplos no arquivo não estão usando a partição como sendo  /dev/sda1, e sim seu UUID. É possível usar também o rótulo (LABEL) do  volume. A razão pela qual o Linux está usando o UUID é que antes de  /dev/sda1 ser montado, não existe raiz de diretórios e, por coerência,  também não existe um diretório /dev. Indicar a partição como ela aparece  na raiz seria impossível. 
- **Segunda coluna – ponto de montagem**: assim como no comando mount, indicamos onde queremos que o volume atue. 
- **Terceira coluna – tipo**: aqui informamos qual sistema de arquivos estamos utilizando; em nosso caso, ext4. 
- **Quarta coluna – opções**: conforme dito, a partição pode ser montada de várias formas diferentes
- Quinta coluna – dump: esta coluna pode ser usada para indicar ao comando dump quais partições  deve fazer backup; o comando dump faz backup em um nível de bloco. A opção padrão é “0” que significa “não faça o dump”. 

Agora, vamos realizar o preenchimento desse arquivo para realizar a montagem automática. Para isso:
- Vamos acessar o su e utilizar o vi para editar o arquivo
- Adicionamos as propriedades refenretes a partição `sdc1`...
- Com isso, na próxima vez que o Linux iniciar, a partição `sdc1` seria montada automatiacmente
```bash
# CLOUD_IMG: This file was created/modified by the Cloud Image build process
UUID=295c2e8c-18f3-4bce-992a-c9ec672e2297       /        ext4   discard,commit=30,errors=remount-ro     0 1
LABEL=BOOT      /boot   ext4    defaults,discard        0 2
UUID=1182-4527  /boot/efi       vfat    umask=0077      0 1
/dev/disk/cloud/azure_resource-part1    /mnt    auto    defaults,nofail,x-systemd.after=cloud-init.service,_netdev,comment=cloudconfig  0       2
/dev/sdc1       /media/novaParticao     ext4    defaults        0       0  
```

Para casos como discos ópticos e pendrives, o Linux conta com um poderosíssimo gerenciador dinâmico de dispositivos do Linux conhecido como udev, que teve como antecessores DEVFS e hotplug que foram combinados para formar este novo gerenciador. 

O `udev` é o componente que permite o plug and play no Linux, e no caso de dispositivos de armazenamento, algumas regras podem ser definidas em /etc/udev/rules.d para automatizar seu uso. Geralmente, regras básicas vêm instaladas em qualquer distribuição Linux moderna, como montar o pendrive automaticamente em `/media/<label do pendrive>`. No entanto, se queremos que um pendrive em específico monte em outro lugar e/ou execute algum aplicativo automaticamente, é com estas regras que isso será feito. 

VIsualizando esse arquivo no cat
```bash
root@vm-linux-meus-estudos-east-us:/etc/udev/rules.d# cat /etc/udev/udev.conf
# see udev.conf(5) for details
#
# udevd is also started in the initrd.  When this file is modified you might
# also want to rebuild the initrd, so that it will include the modified configuration.

#udev_log=info
#children_max=
#exec_delay=
#event_timeout=180
#timeout_signal=SIGKILL
#resolve_names=early
root@vm-linux-meus-estudos-east-us:/etc/udev/rules.d#
``` 

## fsck - Verificando um sistema de arquivos

Quando um sistema de arquivos está em funcionamento, existem diversos comportamentos que o sistmea resolve rapidamente e automaticamente. Eles são feitos com inteligência para resolver a grande maioria dos problemas. No entanto, há possibilidades do sistema de arquivos ser corrompido de alguma maneira e parar de montar automaticamnete (ou manualmente). Nesses casos, podemos usar o comando fsck.

>No Windows, esse comando é chamado de scan disk

O comando fsck (abreviação de filesystemcheck ou verificação de sistema de arquivos) permite uma verificação “na saúde” do sistema de arquivos presente na partição. Este deve ser usado sempre que o sistema de arquivos esteja corrompido por alguma razão, e a partição não pode ser montada e, portanto, utilizada.

A execução de fsck em uma partição com sistema de arquivos corrompido resolverá o problema e permitirá a montagem. Este também é executado automaticamente no boot do sistema operacional Linux para verificar as partições de forma preventiva. 

Para utiliza-lo, basta digitar o comando `sudo fsck <local-particao>`
- Nesse caso, vemos que a partição foi montada corretamente
- Se a partição estivesse corrompida, esse comando iria demorar mais para ser executado. Isso porquê, ele iria tentar recuperar a partição a fim de resutara-la 
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ sudo fsck /dev/sdc1
fsck from util-linux 2.39.3
e2fsck 1.47.0 (5-Feb-2023)
/dev/sdc1 is mounted.
e2fsck: Cannot continue, aborting.
```

-------------------------------------------------------------------

<br>

## Agendamento de tarefas

É uma necessidade comum agendar tarefas em um sistema operacional, que precisem ser executadas no futuro ou regularmente de tempos em tempos. Quando isso acontecer, pode utilizar o `cron`, criado no Unix e portado para o Linux exatamente para este fim. 

Para fins de exemplo, imagine que precisemos realizar backups diários do diretório `/etc` (responsável pelas configurações gerais do sistema) em um arquivo do tipo `tar.gz` ou .tgz. Para tal, precisaremos do comando `tar`, `gzip` e um lugar para armazenar estes arquivos e, para este caso, usaremos o `/var/backups` que existe na árvore de diretórios do Ubuntu.

Outro pré-requisito de nosso exemplo é que desejo acumular estes backups; não basta ter um backup do que o diretório /etc tinha ontem, se quisermos arquivos de uma semana, três semanas ou 23 dias atrás exatamente, será possível. Para tal, o jeito mais fácil e organizado é colocar a data do backup no nome do arquivo, só que isso precisa ser feito de forma dinâmica.


No bash, temos um comando `date` que retorna a data atual. A sintaxe é: `date +%Y-%m-%d`, sendo que: `Y` = ano, `m` = mês e `d` = dia. 
- Vale ressaltar que o formato da data pode ser personalizado, basta alterar a ordem dos parâmetros.
  - Para mostrar a data no formato brasileiro, por exemplo: dia/mês/ano, podemos usar a sinatexe: `date +%d-%m-%Y` 
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ date +%d-%m-%Y
13-06-2025
```

Na exemplo abaixo, testamos esta necessidade e o “truque” é executar o comando `date` ANTES e DENTRO do comando tar, bem no ponto que queremos a data. Vamos explica-lo ponto a ponto:
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ sudo tar cfz /var/backups/etc-backup-$(date +%d-%m-%Y).tar.
gz /etc
tar: Removing leading `/' from member names
```

Podemos visualizar se o arquivo foi criado utilizando o comando `ls` para o caminho da data atual:
- Você precisa ajustar a data para o dia que está vendo isso ou você pode usar o autocompletar com o `tab`
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ ls /var/backups/etc-backup-13-06-2025.tar.gz
/var/backups/etc-backup-13-06-2025.tar.gz
```

#### crontab
Agora que conseguimos fazer manualmente, basta agendarmos o comando de forma com que rode todos os dias.  A primeira possibilidade é usar o comando: 
- `sudo crontab -u root -e` : Esse comando vai criar uma tabela de agendamentos `cron` com a permissão do superusuário (root). O comando abrirá um arquivo com o vim ou nano e nele digitaremos o que está no exemplo abaixo
  - `-u`: Esse parametro permite definir um usuário para acessar a tabela de tarefas.
  - `-e`: Permite fazer edições na tabela de tarefas

```bash
# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# m h  dom mon dow   command
30 2 * * * tar cfz /var/backups/etc-backup-$(date +%d-%m-%Y).tar.gz /etc
```

Cada uma das colunas de tabela cron tem um significado, sendo: 
- **Primeira coluna – minutos**: Informe arquivo o qual minuto o comando 
rodará. Números como “0” ou “00” farão o comando rodar em horas 
cheias, enquanto “30” fará rodar no minuto trinta e assim por diante. Se 
quisermos que o comando rode a cada 5 minutos, usamos “*/5”, por 
exemplo. 
- **Segunda coluna – hora**: Informe a hora da qual o comando deve 
acontecer no formato 0-23. Ou seja, “00 5” fará com que o comando rode 
às cinco horas da manhã, enquanto “45 21” fará com que rode às nove e 
quarenta e cinco da noite. O uso do asterisco faz com que rode em todas 
as horas, ou seja, “10 * * * *” faria o comando rodar em todas as horas de 
minuto 10, enquanto “*/20 * * * *” faria o comando rodar a cada vinte 
minutos. 
- **Terceira coluna – dia do mês**: Permite informar o dia do mês em que o 
comando deverá rodar. Por exemplo, “30 23 10 * *” faria o comando rodar 
às 23h30 todo dia 10 de cada mês. 
- **Quarta coluna – mês**: Permite informar o mês em que o comando deverá 
rodar, sendo assim, “00 2 8 8 *” faria o comando rodar todo dia oito de 
agosto às 2 da manhã. 
- **Quinta coluna – dia da semana**: Permite informar o dia da semana em 
que o comando deverá rodar, sendo “0” para domingo e “6” para sábado 
sendo assim, “15 4 * * 3” faria o comando rodar toda quarta-feira às quatro 
e quinze da manhã. 
- **Sexta coluna – comando**: Na sequência, deve ser informado o comando 
propriamente dito. 

>O comando `crontab` permite manipular a tabela de agendamento de tarefas.

Assim sendo, o backup do diretório /etc foi marcado para todos os dias às duas e meia da manhã, bastando sair do vi com o comando “:x”.

### Armazenando scripts vinculados ao cron

Outra possibilidade mais simples é a criação de scripts que podem ser armazenados ou vinculados aos diretórios `/etc/cron.hourly`, `/etc/cron.daily`, `/etc/cron.weeklye` `/etc/cron.monthly`, permitindo rodar o comando de hora em hora, diariamente, semanalmente e mensalmente, respectivamente. Os horários de execução podem ser vistos ou modificados no arquivo `/etc/crontab`, como pode ser visto no exemplo abaixo:

Atente-se apenas ao fato que em `/etc/crontab` deve ser informado o usuário da qual o comando rodará com os privilégios. 

```bash
azureuser@vm-linux-meus-estudos-east-us:~$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
# You can also override PATH, but by default, newer versions inherit it from the environment
#PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || { cd / && run-parts --report /etc/cron.daily; }
47 6    * * 7   root    test -x /usr/sbin/anacron || { cd / && run-parts --report /etc/cron.weekly; }
52 6    1 * *   root    test -x /usr/sbin/anacron || { cd / && run-parts --report /etc/cron.monthly; }
#
```