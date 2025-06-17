# Linux
O Linux é um software livre, de código aberto, gratuito e com diversas possibilidades de uso. O Linux é um exemplo de inovação e excelência dentro da tecnologia da informação.

>Linux é o nome dado à base de código de um sistema operacional. Foi desenvolvido na década de 90 por Linus Torvalds. Linus Torvalds foi o criador do Kernel, que é o núcleo do sistema operacional. Em torno dele, você tem as ferramentas que são utilizadas para compor o sistema operacional e transformá-lo em uma plataforma utilizável e gerenciável, chamadas de GNU.

Por se tratar de um sistema operacional open-source, ele acabou gerando várias linhagens. Por exemplo: Ubuntu, Debian, Red Hat, Mint, SUSE, etc. Todas elas são distribuições mais robustas porque exigem grupos econômicos por trás delas.

A principal característica dos sistemas Linux é a capacidade que você, com o conhecimento necessário, tem de acessar o código-fonte, redevelopar, reconstruir, modificar, alterar o sistema e, se achar necessário, construir seu próprio sistema com as suas especificações.

Além de ser o SO favorito da segurança da informação, o Linux é amplamante utilizado no ambiente de Ti de grandes empresas, como o Google, IBM e até mesmo o Facebook.

O Linux, ou suas distribuições, pode ser utilizado em projetos de segurança da informação, especialmente quando há questões relacionadas a vulnerabilidades que precisam ser exploradas, descoberta de serviços em rede, testes de penetração em redes/aplicações, etc.

>Existe um conceito sobre o sistema Linux: Ele é considerado, geralmente, um sistema mais seguro que os outros sistemas operacionais. Não existe um sistema operacional 100% seguro. Sempre existirão vulnerabilidades que podem ser exploradas.
- O Linux é um sistema que já foi muito testado e rapidamente corrigido quando problemas na sua estrutura acontecem. Por conta disso, ele pode ser visto como um sistema mais seguro.

Mas a segurança do Linux não está associada somente a esse aspecto, mas também aos usuários. Quando um usuário tem acesso administrativo através da sua conta, ele consegue realizar qualquer tarefa. Dessa forma, o Linux consegue lidar com essas questões separando arquivos de acesso do usuário e arquivos do sistema.

-----------------------------------------


#### Certificações
As certificações Linux são muito importantes na área de atuação de um profissional especialista em sistemas e também para especialistas em segurança. A certificação mais conhecida é o LPI (Linux Professional Institute). Ele possui 3 níveis: básico, intermediário e avançado.

-----------------------------------------
#### Mais sobre o Linux
O Linux se destaca por ser um sistema flexível, estável e robusto, o que o torna ideal tanto para servidores quanto para desktops. Ele é usado em servidores de alta performance, dispositivos embarcados, supercomputadores, e até mesmo em smartphones (Android, por exemplo).

Sua estrutura de código aberto (open-source) significa que qualquer pessoa pode modificar o código, fazer contribuições para o sistema, e até mesmo criar sua própria distribuição personalizada. Esse conceito de colaboração comunitária é um dos pilares do sucesso do Linux. Ao contrário de sistemas proprietários como o Windows ou macOS, o Linux não tem uma única empresa controlando seu desenvolvimento; ele é mantido por uma comunidade global de desenvolvedores.

O terminal do Linux (linha de comando) é uma ferramenta poderosa que permite gerenciar todo o sistema de forma rápida e eficiente. A seguir, veja um resumo de comandos úteis para administração de usuários e pacotes no Linux:

-----------------------------------------

#### Outras vantagens do Linux:
- Custo: O Linux é gratuito, ao contrário de outros sistemas operacionais que exigem licenças caras.
- Segurança: O Linux tem um forte foco em segurança, com a utilização de permissões de arquivos e a separação de funções entre usuários e administradores. Além disso, devido à sua natureza open-source, as vulnerabilidades são rapidamente identificadas e corrigidas.
- Performance: O Linux é muito eficiente, o que o torna ideal para servidores e máquinas de alto desempenho.
- Suporte comunitário: Existem vastos fóruns e documentações sobre o Linux, com respostas para quase todos os problemas que podem surgir.

-----------------------------------------
<br>

# Comandos Linux

Abaixo, uma lista de comandos úteis para o administrar sistemas Linux, subdividido em tópicos:

## Comandos essenciais

- `su`: Altera para o superuser (usuário root)
- `apt`: Gerenciador de pacotes para sistemas Debian e derivados
    - `install <pacote>`: instala pacotes
    - `update`: atualiza repositórios
    - `upgrade`: atualiza pacotes
- `sudo <comando>`: Executando comandos com permissões de superusuário
    - >Necessário instalar o pacote `apt install sudo`. Necessário utilizar o superuser

<br>

--------------------------------------------------------

## Administração de usuários

Comandos úteis para o administrar usuários no Linux:
- `passwd <usuario>`: Altera a senha de um usuário
- `useradd`: Cria um novo usuário no sistema
    - **Parâmetros úteis**:
    - `-m`: cria o diretório home do usuário
    - `-G <grupo`: adiciona o usuário a grupos
    - `-s <shell>`: define o shell padrão do usuário
- `groupadd <grupo>`: Cria um novo grupo no sistema
- `usermod`: Modifica informações de um usuário
    - **Parâmetros úteis**:
    - `-aG`: adiciona o usuário a um grupo, sem remover do grupo atual
    - `-s <shell>`: altera o shell padrão do usuário

<br>

--------------------------------------------------------

## Raiz de diretórios
Comandos úteis para o administrar arquivos e diretórios no Linux:

### Manipulação de arquivos/diretórios
- `ls`: Lista o conteúdo de um diretório
    - **Parâmetros úteis**:
    - `-l`: exibe detalhes (permissões, dono, grupo, tamanho)
    - `-a`: inclui arquivos ocultos
    - `-h`: exibe tamanhos de arquivos em formato legível
- `pwd`: Exibe o caminho absoluto do diretório atual
- `mkdir`: Cria um novo diretório
    - **Parâmetros úteis**:
    - `-p`: cria diretórios pai se necessário
    - `-v`: exibe os diretórios criados
- `cd <diretorio>`: Muda para um diretório específico
- `cp <origem> <destino>`: Copia arquivos ou diretórios
    - **Parâmetros úteis**:
    - `-r`: copia diretórios recursivamente
    - `-v`: exibe os arquivos copiados
- `mv <origem> <destino>`: Move ou renomeia arquivos e diretórios
    - Pode ser usado também para renomear: `mv antigo.txt novo.txt`
- `rm`: Remove arquivos ou diretórios
    - **Parâmetros úteis**:
    - `-f`: força a remoção, ignorando arquivos inexistentes e sem pedir confirmação
    - `-r`: remove diretórios de forma recursiva
    - `-v`: exibe os arquivos/diretórios sendo removidos

### Visualização de arquivos
- `cat`: Exibe todo o conteúdo de um arquivo
- `head`: Exibe as primeiras linhas de um arquivo
    - **Parâmetros úteis**:
    - `-n <número>`: número de linhas a exibir
- `tail`: Exibe as últimas linhas de um arquivo
    - **Parâmetros úteis**:
    - `-n <número>`: número de linhas a exibir
    - `-f`: acompanha o arquivo em tempo real
- `less`: Exibe o conteúdo de um arquivo com rolagem
    - **Parâmetros úteis**:
    - `-S`: desabilita a quebra de linha
    - `-N`: exibe os números das linhas

### Busca e localização
- `locate`: Localiza arquivos usando um banco de dados
    - **Parâmetros úteis**:
    - `-i`: ignora diferenças entre maiúsculas e minúsculas
    - `-r`: usa expressões regulares
- `grep`: Busca por padrões em arquivos
    - **Parâmetros úteis**:
    - `-i`: ignora diferenças entre maiúsculas e minúsculas
    - `-r`: busca recursivamente
    - `-v`: inverte a busca (exibe as linhas que não contém o padrão)

### Compressão e arquivos

>Necessário instalar os pacotes `sudo apt install unzip gzip bzip2`

- `tar`: Empacota ou extrai arquivos em formato .tar
    - **Parâmetros úteis**:
    - `-c:` cria um arquivo
    - `-x:` extrai o conteúdo
    - `-v:` exibe os arquivos processados
    - `-f <arquivo>`: especifica o nome do arquivo
- `unzip`: Extrai arquivos de um arquivo compactado .zip
    - `-l`: lista o conteúdo do arquivo
    - `-d <diretório>`: define o diretório de destino

**Exemplos - tar**
- `tar -czvf arquivo-comprimido.tar.gz *.txt`: Compressão utilizando o algoritmo `gzip`. Compressão de todos os arquivos `.txt` de um diretório específico.
- `tar -xzvf arquivo-comprimido.tar.gz`: Descompressão utilizando o algoritmo `gzip`


### Baixar arquivos da internet
>Necessário instalar o pacote `sudo apt install wget`

- `wget`: Baixa arquivos da web
    - **Parâmetros úteis**:
    - `-r`: baixa recursivamente
    - `-N`: não baixa arquivos não modificados
    - `-O <arquivo>`: define o nome do arquivo de saída

>Necessário instalar o pacote `sudo apt install wget`

<br>

--------------------------------------------------------

## Configurações de rede
Comandos úteis para o configuração de rede no Linux:

>Necessário instalar o pacote `sudo apt install net-tools nmap`

- `sudo ifconfig`: Exibe e configura interfaces de rede
    - **Parâmetros úteis**:
    - `up`: ativa a interface
    - `down`: desativa a interface
- `sudo route`: Exibe ou modifica a tabela de roteamento
    - **Parâmetros úteis**:
    - `-n`: mostra IPs em formato numérico
    - `-add`: adiciona uma rota
    - `-del`: remove uma rota
    - `-add gw <ip> <interface-rede>`: adiciona um gateway
- `sudo nmap <interface-rede>`: Ferramenta de varredura de rede
    - **Parâmetros úteis**:
    - `-sP`: verifica hosts ativos
    - `-p <portas>`: define portas a escanear
    - `-v`: modo verboso
    - `-sV`: varredura com identificação de serviços

<br>

--------------------------------------------------------

## Administração de sistema
Comandos úteis para o administrar o sistema, processos e serviços no Linux:

### Listar e monitorar processos
- `ps` Lista os processos
    - **Parâmetros úteis**:
    - `-a`: mostra os processos de todos os usuários (permissão de root para tal)
    - `-u`: exibe uma lista detalhada contendo o usuário dono do processo
    - `-x`: exibe processos que não estejam associados ao terminal de comandos (pois cada processo tem uma origem, ao usar o parâmetro, podemos ver processos da interface gráfica, por exemplo, que é considerada outro terminal)
- `top` Monitor de processos

### Encerrar processos
- `kill <PID>` ou `kill -SIGKILL <PID>`: Encerra um processo pelo seu pid
- `kill -SIGSTOP <PID>`: Pausa um processo pelo seu pid
- `kill -SIGCOUNT <PID>`: Descongela um processo pelo seu pid 
- `killall <nome_processo>`: Encerra todos os processos ativos pelo seu nome

### Terminais virtuais
- `screen -S <nome_terminal>`: Cria um terminal virtual
- `screen -ls`: Lista os terminais disponíveis
- `screen -r <nome_terminal>`: Troca de terminal

### Gerenciar serviços
- `service <nome_servico> start`: Inicializa um serviço
    - Também é possível utilizar com as ações: `stop` para parar, `status` para verificar o status e `restart` para reiniciar

<br>

-------------------------------------------------

## Acesso remoto com SSH
Comandos úteis para realizar conexão e transferência ssh no Linux:

>Necessário instalar o pacote `sudo apt install ssh`

### Conexão com SSH
- `ssh -i C:\Chaves\minha-chave.pem user@ip`: Realiza o acesso remoto para outra máquina
    - `user`: usuário que desejo usar para conectar
    - `ip`: o nome ou IP do host
    - `-i`: para utilizar chaves privadas, utilize o parâmetro `-i` seguido pelo caminho relativo da chave

- `echo "ip user" >> /etc/hosts`: Adiciona um host no arquivo `/etc/hosts` do Linux.
    - É necessário executar como superuser

### Transferência de arquivos
- `scp [origem] [destino]`: Uma vez conectado a outra maquina com o SSH, esse comando realiza a transferência de arquivo
    - `origem`: Diretório/caminho do arquivo que será enviado
    - `destino`: Diretório/caminho onde o arquivo será armazenado na outra máquina

<br>

-------------------------------------------------

## VI

Comandos úteis para o utilizar o vi ou vim no Linux:

### Entrando no vi
- `vi arquivo-vim.txt`: vi arquivo.txt: Abre um arquivo com o editor vi. Se o arquivo não existir, será criado automaticamente ao salvar.

### Modos do vi
- `i`: Entra no modo de inserção na posição atual do cursor
- `a`: Entra no modo de inserção após o cursor
- `A`: Entra no modo de inserção no final da linha
- `o`: Cria uma nova linha abaixo e entra no modo de inserção
- `Esc`: Sai do modo de inserção e retorna ao modo de comando

### Comandos para salvar e sair
- `:w`: Salva o arquivo
- `:w outro-arquivo.txt`: Salva o conteúdo em um novo arquivo
- `:3,10w outro-arquivo.txt`: Salva apenas as linhas de 3 a 10 em outro arquivo
- `:wq ou :x`: Salva e sai do vi
- `:q`: Sai do vi (somente se não houver alterações)
- `:q!`: Sai do vi sem salvar as alterações

### Edição de texto
- `yy`: Copia uma linha inteira
- `10yy`: Copia um n° especifico de linhas, nesse caso dez
- `p`: Cola a linha copiada
- `dd`: Apaga a linha inteira
- `10dd`: Apaga um n° especifico de linhas, nesse caso dez

### Navegação e visualização
- `:set nu`: Exibe a numeração das linhas
- `:n`: Vai diretamente para a linha de número n (ex: :25 vai para a linha 25)
  
<br>

-------------------------------------------------

## Recursos avançados

Comandos úteis para o administrar partições no Linux:

### Partições, sistema de arquivos, montagem e desmontagem

- `fdisk`: Gerencia partições de disco (criação, exclusão e modificação)
    - `m`: abre o manual do comando
    - `p`: cria uma nova partição
    - `q`: sai do comando `fdisk`
- `mkfs`: Cria sistemas de arquivos em partições
    - `sudo mkfs.ext4 /dev/sdX1`: formata a partição em ext4
    - `sudo mkfs.vfat /dev/sdX1`: formata a partição em FAT32
- `sudo mount <caminho/particao>`: Monta uma partição ou sistema de arquivos
- `sudo umount <caminho/particao>`: Desmonta uma partição montada
- `fsck`: Verifica e repara erros em sistemas de arquivos
    - `-A`: verifica todos os sistemas de arquivos listados em `/etc/fstab`
    - `-r`: modo interativo (pede confirmação)

### Firewall
- `sudo iptables -L`: Verificar regras de firewall aplicadas a maquina