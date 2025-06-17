# Administração de sistema e editor de texto
Retomaremos nosso aprendizado dos comandos essenciais e abordaremos, 
neste capítulo, o gerenciamento de processos, a possibilidade de acesso remoto 
com SSH e o processador de texto vi. 

## Gerenciando processos

Um programa de computador é algo inerte, adormecido em um disco rígido de um equipamento. Quando chamamos um comando ou clicamos em um lançador de programa em uma interface gráfica, ele se torna um processo. Ou seja, um processo é um programa em execução. 

No entanto, em alguns momentos, processos podem travar ou consumir 
recursos demais de uma máquina. Portanto, entender como gerenciar é de suma importância para qualquer administrador de sistemas.

>O gerenciamento de processos permite gerenciar os programas que estão sendo executados em um sistema operacional.

Os comandos a seguir permitem a um 
administrador monitorar os processos em andamento, verificando o consumo de 
recursos e podendo, até mesmo, destruir processos.

### ps listar processos

- O comando `ps` é utilizado para listar processos.

O comando “ps” permite listar os processos atuais de um sistema e é, 
geralmente, acompanhado pelos parâmetros: 
- `-a`: mostra os processos de todos os usuários (permissão de root para tal)
- `-u`: exibe uma lista detalhada contendo o usuário dono do processo
- `-x`: exibe processos que não estejam associados ao terminal de comandos (pois cada processo tem uma origem, ao usar o parâmetro, podemos ver processos da interface gráfica, por exemplo, que é considerada outro terminal)

O comando abaixo vai listar todos os processos que estão executando no sistema, utilizando o parâmetro `-aux` para uma verificação completa.
- O retorno traz informações dos processos
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ ps -aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.3  0.3  22900 14128 ?        Ss   14:21   0:04 /sbin/init
root           2  0.0  0.0      0     0 ?        S    14:21   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        S    14:21   0:00 [pool_workqueue_release]
root           4  0.0  0.0      0     0 ?        I<   14:21   0:00 [kworker/R-rcu_gp]
root           5  0.0  0.0      0     0 ?        I<   14:21   0:00 [kworker/R-sync_wq]
root           6  0.0  0.0      0     0 ?        I<   14:21   0:00 [kworker/R-slub_flushwq]
root           7  0.0  0.0      0     0 ?        I<   14:21   0:00 [kworker/R-netns]
root           9  0.0  0.0      0     0 ?        I<   14:21   0:00 [kworker/0:0H-events_highpri]

//CONTINUA
```

### top - monitor de processos
- O comando `top` permite monitorar os processos do sistema.

Enquanto o ps mostra uma "chapa" daquele momento, ou seja, os processos que estão rodando que o comando `ps` é digitado, o top é mais intermitente, ou seja, ele vai ficar rodando até que você decida destruir esse monitor de processo.

Vale ressaltar que o comando top não é apenas um monitor de processos, ele permite obter 
informações importantes, como o total de processos em andamento, seus estados 
no momento (executando, em espera, parados e zombies), uso de CPU, memória 
RAM e memória de swap.

Encabeçando a lista, estão sempre os processos que consomem mais 
recursos de sistema e como podemos ver na figura “Exemplo de execução do top”, 
os processos associados à interface gráfica (gnome-shell, Xorg etc.) estão sempre 
no topo: 

```bash
top - 14:54:10 up 32 min,  3 users,  load average: 0.01, 0.13, 0.07
Tasks: 214 total,   1 running, 213 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.2 sy,  0.0 ni, 99.8 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   3917.7 total,   2318.8 free,   1012.3 used,    842.2 buff/cache
MiB Swap:      0.0 total,      0.0 free,      0.0 used.   2905.3 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
      1 root      20   0   23028  14424   9816 S   0.0   0.4   0:06.72 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kthreadd
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_workqueue_release
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-rcu_gp
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-sync_wq
```

### Matando processos com kill

- Os comandos `kill` e `killall` são usados para encerrar processos.

Agora que sabemos como listar os processos e como monitorá-los, uma necessidade muito importante é sabermos destrui-los (bem como para-los e inicia-los) sempre que for necessário. Eventualmente, um administrador de sistema precisa interromper processos 
em andamento de maneira abrupta, seja por estarem consumindo recursos 
indesejados, seja por representarem algum risco ao sistema. 

Para demonstrar seu funcionamento, aproveitei a interface gráfica e abri o 
aplicativo `gnome-chess`, que faz parte do pacote padrão para desktop do Ubuntu. 

>Para que o terminal não fique travado, podemos executar o comando utilizando o `&`. Essa propriedade faz com que o processo seja executado em background, deixando o terminal livre. O terminal retorna o PID, que é o identificador do processo:
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ gnome-chess &
[1] 9118
```

#### kill - matando um processo

Ao executar `$ ps`, o processo é exibido:
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ ps
    PID TTY          TIME CMD
   9207 pts/0    00:00:00 bash
   9118 pts/0    00:00:00 gnome-chess
   9120 pts/0    00:00:00 hoichess
   9270 pts/0    00:00:00 ps
```

Com isso, podemos encerrá-lo utilizando o comando `kill` com o seu identificador:
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ kill 9118
```
Além do fato do tabuleiro de xadrez ser fechado abruptamente, ao repetir o comando de listagem (ps) à procura de gnome-chess, o processo não existe mais.

**Combinando comandos**<br>
Trata-se de um bom momento para demonstrar a possibilidade de combinar comandos: a execução do comando `ps`, especialmente com os parâmetros `aux`, pode ficar longa e confusa. O símbolo “|” (ou pipe) permite combinar um comando a outro. 

Por exemplo, podemos combinar o comando psaux com o comando `grep`, que permite busca por palavras-chave: `ps -aux | grep gnome-chess`
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ ps -aux | grep gnome-chess
azureus+    9300  0.0  0.0   7080  2264 pts/0    S+   15:20   0:00 grep --color=auto gnome-chess
```

>Não é necessário usar o `sudo` para encerrar um processo do proprio usuário. Porém, se o processo pertence à um usuário distinto do atual, é necessário utilizar o `sudo`

#### SIGSTOP e SIGCONT - pausando e despausando um processo

Embora o comando se chame kill, ele pode ser utilizado para mandar sinais 
diferentes ao processo. Por exemplo, o uso do parâmetro `-SIGSTOP` no comando 
kill não destruiria o processo, apenas o pausaria ou congelaria.

```bash
azureuser@vm-linux-meus-estudos-east-us:~$ kill -SIGSTOP 9118
```

>Sendo assim, um processo com a compressão de um arquivo (comando tar) que demorasse muito tempo não precisaria ser necessariamente interrompido, caso precisasse liberar recursos de sistema

O parâmetro `-SIGCONT` descongela o processo, que retoma seu processamento exatamente de onde parou.
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ kill -SIGCONT 9118
```

>Quando utilizamos o comando `kill` sem nenhum parâmetro, o comando manda um sinal de `SIGKILL` que, na prática, significa a destruição do processo. Ou seja, `kill <PID>` e `kill -SIGKILL <PID>` fazem a mesma coisa 

#### killall - matando varios processos

Há uma variação do comando kill, conhecida como `killall` (ou “matar todos”), que deve ser acompanhado pelo nome do processo (e não o PID). Isso acontece pois, como o nome sugere, o comando pode destruir todos os processos que possuam o mesmo nome, isso é muito útil. 

Para a demonstração, usei o servidor web mais utilizado do mundo, o Apache Web Server; caso precise de um servidor web, não deixe de instalá-lo com o comando `$ sudo apt install apache2`. Para iniciá-lo, podemos usar o comando: `$ sudo /etc/init.d/apache2 start`

O apache é útil neste exemplo, pois como todo serviço escalonável, ele cria subprocessos conforme a necessidade de atender a muitas requisições de rede. Logo “de cara”, seu processo principal gera outros dois processos, assim, temos três processos chamados apache2.
- Podemos conferir isso utilizando o comando `ps -aux` combinado com o comando `grep`, como visto anteriormente. Repare que possuo três processos, sendo um principal e dois  subprocessos (embora não saiba dizer qual é qual)

```bash
azureuser@vm-linux-meus-estudos-east-us:~$ ps -aux | grep apache2
root        9353  0.0  0.1   6904  5028 ?        Ss   15:33   0:00 /usr/sbin/apache2 -k start
www-data    9355  0.0  0.1 1212988 5428 ?        Sl   15:33   0:00 /usr/sbin/apache2 -k start
www-data    9356  0.0  0.1 1212988 5556 ?        Sl   15:33   0:00 /usr/sbin/apache2 -k start
azureus+    9415  0.0  0.0   7076  2264 pts/0    S+   15:34   0:00 grep --color=auto apache2
```

Agora, para encerrar todos os processos do `apache2`, podemos utilizar o `sudo killall apache2`, 

>O comando killall apache2 é acompanhado pelo sudo, pois o serviço pertence ao usuário `www-data` e, como podemos ver, ao listar os processos novamente, o comando faz o serviço sem deixar vestígios.  

```bash
azureuser@vm-linux-meus-estudos-east-us:~$ sudo killall apache2

azureuser@vm-linux-meus-estudos-east-us:~$ ps -aux | grep apache2
azureus+    9473  0.0  0.0   7076  2268 pts/0    S+   15:40   0:00 grep --color=auto apache2
```

>Atenção ao sudo! Se utilizarmos o comando sem o `sudo`, dará erro de permissão
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ killall apache2
apache2(9353): Operation not permitted
apache2(9355): Operation not permitted
apache2(9356): Operation not permitted
apache2: no process found
```

>Outra forma de interromper processos é fechar o terminal no qual eles rodam. Processos não possuem apenas um usuário dono, mas um terminal (seja ele gráfico ou não) associado a eles.

-------------------------------------------------------------------------------------------

## Processos em background e screen

Todo comando que executa em um terminal, ao seu final, devolve o prompt 
de comando para a execução de novos comandos, mas até o seu encerramento, ele 
“tratava” o terminal, não permitindo novas interações do usuário. Embora seja um 
fenômeno comum em comandos de modo texto, esse comportamento torna-se 
evidente em aplicativos gráficos, como o `gnome-chess`

Isso acontece porque o processo está rodando em foreground ou primeiro 
plano. 

### & - rodar comando sem travar terminal
Podemos solicitar a execução de um comando em segundo plano ou 
background, desde que sua linha de comando seja acompanhada por um “&” (ê 
comercial) em seu final: 
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ gnome-chess &
```

No entanto, o fato do processo rodar em segundo plano não significa que este está livre do terminal; o processo continua atrelado ao terminal que, se for fechado, leva o tabuleiro de 
xadrez com ele.

### screen - Criando terminais virtuais

Processos sempre estarão associados a terminais, isto é um fato. No entanto, 
há formas de criar terminais virtuais que podem ser abandonados e retomados em 
um segundo momento

Isso é particularmente interessante,nos casos em que 
precisamos rodar comandos muito longos, como a realização de um backup (como 
tar ou outro comando) ou o download de um arquivo muito grande (com o bom e 
velho wget)

Imagine, ainda, que você queira iniciar o comando fisicamente no 
servidor da empresa e deseja verificar a execução horas depois, remotamente, de 
sua casa? O comando screen pode ajudar nesse caso.

Para instala-lo, utilize o comando: `sudo apt install screen`

O comando `screen -S [nome do terminal]` cria um terminal virtual que pode ser abandonado a qualquer momento, utilizando [CTRL+A], seguida da tecla [D]:
- Quando se executa esse comando, outro terminal identico ao anterior sobe em cima do terminal original. Pode parecer que nada mudou, pois essa troca é feita de maneira sutil, porém você ja está em um terminal virtual.
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ screen -S terminal2
[detached from 9584.terminal2]
```

### -ls - visualizar e trocar de terminais
- Ao executar `screen -ls`, podemos observar os terminais disponíveis:
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ screen -ls
There is a screen on:
        9584.terminal2  (06/10/25 16:04:20)     (Detached)
1 Socket in /run/screen/S-azureuser.
```

- Para trocar de terminal, utilize o comando: ` screen -r <nome_terminal>`
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ screen -r terminal2
[detached from 9584.terminal2]
```

- Digitar `exit` no outro terminal, encerra sua execução.
```bash
azureuser@vm-linux-meus-estudos-east-us:~$ screen -r terminal2
[screen is terminating]

azureuser@vm-linux-meus-estudos-east-us:~$ screen -ls
No Sockets found in /run/screen/S-azureuser.
```

-------------------------------------------------------------------------------------------
<br>

## Introdução ao VI

Fica absolutamente impossível de admoinistrar um servidor Linux sem conhecer um editor de textos que possa rodar em modo terminal. Isso porque, a grande maioria dos arquivos presentes no Linux/Unix utilizam arquivos de texto para parametrizar os serviços, configurar elemetnos de grupo, entre outros. Portanto, através dos editores de texto podemos manipular várias configurações. 

Um dos editores de texto mais utilizados no Linux é o vi (lê-se "vi ai") ou vim - VI IMproved (lê-se "vi ai am"), que é uma espécie de vi "melhorado". Com o vi, podemos acessar qualquer arquivo e criar arquivos.

Embora existam outros editores de texto, como o emacs ou o nano (que é bem fácil de usar), o vi possui uma infinidade de recursos interessantes e poderosos. <br>
Praticamente todas as letras minúsculas e maiúsculas disparam comandos no 
editor; portanto, para que este tutorial não fique muito complexo (e chato), vou me 
restringir às funcionalidades mais comuns e úteis.

Normalmente, os sistemas Linux já possuem esse pacote instalado. Mas caso não tenha e precise instalar, utilize o comando `sudo apt install vim`

### Comandos VI
Abaixo, uma lista de comandos interessantes para se usar no vi

>Vale ressaltar que o vi é case sensitive, ou seja, ele diferencia letras minusculas e maisculas. `a` tem um comportamente diferente de `A`, portanto, dobre a atenção ao utilizar os comandos. 

#### Entrando no VI
Para entrar no vi, basta digitar, no terminal, `$ vi`. 

Normalmente, o comando é acompanhado com o nome do arquivo que se quer criar ou editar, conforme podemos ver no exemplo a seguir:
- Esse comando também é utilizado para abrir um arquivo existente.
- Ao utilizar esse comando, uma tela com o conteúdo do arquivo é exibida. Como esse arquivo é novo, não tem nenhum conteúdo nele.
- Cada `~` representa uma linha do arquivo. A primeira não possui o `~` porque podemos escrever nela
```bash
azureuser@vm-linux-meus-estudos-east-us:~$/~Arquivos vi arquivo-vim.txt

~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
"arquivo-vim.txt" [New]   
``` 

#### Saindo do VI
Para sair do editor no modo de comando, você deve digitar: `:q` (dois pontos e a letra q minúscula, 
de quit). Sugiro acostumar-se com a combinação das teclas `ESC`, `:`, `q` e 
`ENTER`, pois você pode estar em modo de edição de texto, e a tecla `ESC` fará 
voltar ao modo de comandos. 
```bash
~
~
~
~
~
~
~
~
~
~
~
:q -- Saindo do vi
```

#### Sair do arquivo sem salvar alterações
Se você abriu um arquivo, realizou alterações e não deseja salvar, podemos utilizar o comando `:q!`
```bash
Fiz alterações e não quero salvar
~
~
~
~
~
~
~
~
~
~
~
:q! -- Saindo do vi sem salvar alterações
```

### Modo de comando e modo de edição
Uma das coisas que causam estranheza no primeiro contato com o VI é que ele tem 2 modos diferentes: 
- comandos: as teclas que você digitar dispararão comandos no editor
- edição: tudo aquilo que você digitar fará, literalmente, parte do documento. 

Fazer essa transição entre o modo de comando e edição é a orimeira coisa que temos que nos atentar.

Uma vez que você abre um arquivo (ou cria um novo), você entra no modo de comandos:
- Podemos perceber que é o modo de comandos, pois qualquer coisa que digitarmos será exibido no rodaré da tela (onde está escrito `"arquivo-vim.txt" [New]`)
- Nesse modo, aquilo que você escreve não é necesseriamente transcrito para o arquivo.
```bash
azureuser@vm-linux-meus-estudos-east-us:~$/~Arquivos vi arquivo-vim.txt

~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
"arquivo-vim.txt" [New]  
```

Existem várias maneiras de migrar do modo de comando para o modo de edição de texto, vou me restringir às quatro mais interessantes.

#### 1. Tecla i
A mais simples é a tecla `i`, através da qual entramos em modo de edição de texto no ponto em que o cursor estiver no momento. Uma vez que essa tecla foi pressionada, o que for escrito será inserido no arquivo.
- Para voltar ao modo de comandos, tecle `ESC`. 
```bash
Apos apertar a tecla i, podemos digitar o conteudo do arquivo
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
-- INSERT --                                                                        1,62           All
```

#### 2. Tecla a
Outra forma é a tecla `a`, através da qual entramos em modo de edição de texto uma posição após o ponto em que o cursor estiver no momento. Dessa maneira, podemos continuar a frase exatamente de onde havia parado
- Para voltar ao modo de comandos, tecle `ESC`. 
```bash
Apos apertar a tecla i, podemos digitar o conteudo do arquivo  Apertei a tecla a para continuar de onde parei
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
-- INSERT --                                                                        1,108           All
```

#### 3. Tecla A
O terceiro modo é editar com [A], e entramos em modo de edição no final da frase em que estamos. Ao digitar [A], somos enviados imediatamente ao final da frase, independente de onde o cursor estiver posicionado.
- Para voltar ao modo de comandos, tecle `ESC`. 

> Alguns atalhos interessantes em modo de comando:
- `G`: Redireciona para o fim do arquivo, independente do tamanho
- `$`: Desloca imediatamente para o fim da linha atual
- `^`: Desloca imediatamente para o início da frase

```bash
Apos apertar a tecla i, podemos digitar o conteudo do arquivo  Apertei a tecla a para continuar de onde parei Apertei A para ir para o final da linha
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
-- INSERT --                                                                        1,148           All
```

#### 4. Tecla o
Por último, podemos usar `o` e entrar no modo de edição na linha seguinte àquela em que estamos. É como se fosse uma quebra de linha
- Para voltar ao modo de comandos, tecle `ESC`. 
```bash
Apos apertar a tecla i, podemos digitar o conteudo do arquivo  Apertei a tecla a para continuar de onde parei Apertei A para ir para o final da linha
Apertei o e fui para a linha debaixo
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
-- INSERT --                                                                        2,36           All
```

>Vale ressaltar que existem ainda outras maneiras de fazer essa transição, mas focamos em apenas 4.

### Salvando o arquivo

Para salvar o arquivo, você deve estar em modo de comandos, e pode utilizar o comando: 
- `:w`, em seguida pressionar `ENTER`

Para salvar e sair do arquivo, podemos utilizar dois comandos:
- `:wq` e `ENTER`
- `:x` e `ENTER`

E para sair sem salvar, podemos utilizar o comando:
- `:q!` e `ENTER`

**Exemplo - Salvar e sair do arquivo**
```bash
Apos apertar a tecla i, podemos digitar o conteudo do arquivo  Apertei a tecla a para continuar de onde parei Apertei A para ir para o final da linha
Apertei o e fui para a linha debaixo
~
~
~
~
~
~
:x -- SALVEI E FECHEI O ARQUIVO

##VISUALIZANDO O ARQUIVO COM cat
azureuser@vm-linux-meus-estudos-east-us:~/Arquivos$ cat arquivo-vim.txt
Apos apertar a tecla i, podemos digitar o conteudo do arquivo Apertei a tecla a para continuar de onde parei Apertei A para ir para o final da linha
Apertei o e fui para a linha debaixo
```

Para salvar em um arquivo diferente:
- `:w nome-do-outro-arquivo` Esse comando vai salvar o conteudo em outro arquivo

Para salvar um intervalo em outro arquivo: É possível especificar um intervalo que será salvo. Por exemplo, queremos salvar da linha 3 à linha 5
- `:3,5w nome-do-outro-arquivo`

**Exemplo - Salvando um intervalo em outro arquivo**
- Nesse exemplo, vamos salvar as instruções de salvamento em outro arquivo `instrucoes-salvar.txt`
```bash
  1 Apos apertar a tecla i, podemos digitar o conteudo do arquivo Apertei a tecla a para continuar de     onde parei Apertei A para ir para o final da linha
  2 Apertei o e fui para a linha debaixo
  3 Agora estou editando livremente
  4 A ideia é adicionar mais linha nesse arquivo
  5
  6 TODOS OS COMANDOS ABAIXO DEVEM SER FEITOS COM O VIM NO MODO DE COMANDO
  7 PRESSIONE A TECLA ENTER PARA EXECUTA-LOS
  8                                                                                                     9 Para salvar um arquivo - :w                                                                        10 Para salvar e sair de um arquivo - :wq ou :x                                                       11 Para salvar o conteudo em outro arquivo - :w <nome_outro_arquivo>
 12 Para salvar um intervalo de linhas em outro arquivo - :2,5 <nome_outro_arquivo>   
 :6,12w instrucoes-salvar.txt
 "instrucoes-salvar.txt" [New] 7L, 333B written                                      12,1          All

## VISUALINDO O ARQUIVO COM CAT
azureuser@vm-linux-meus-estudos-east-us:~/Arquivos$ cat instrucoes-salvar.txt
TODOS OS COMANDOS ABAIXO DEVEM SER FEITOS COM O VIM NO MODO DE COMANDO
PRESSIONE A TECLA ENTER PARA EXECUTA-LOS

Para salvar um arquivo - :w
Para salvar e sair de um arquivo - :wq ou :x
Para salvar o conteudo em outro arquivo - :w <nome_outro_arquivo>
Para salvar um intervalo de linhas em outro arquivo - :2,5 <nome_outro_arquivo>
azureuser@vm-linux-meus-estudos-east-us:~/Arquivos$
``` 

### Numeração de linhas e navegação
Um recurso muito útil para utilizar no vi, é a questão de habilitar o numero de linhas de um arquivo. Isso é util porque, em alguns casos, podemos estar manipulando um código-fonte e o depurador em questão mencione "Erro na linha 39", eu preciso me dirigir rapidamente a linha 39 sem que eu precise ficar contando o número de linhas

>Em modo de comando, podemos navegar pelo arquivo utilizando as teclas de direção do teclado. 

Por exemplo, vamos abrir o arquivo `syslog` que é um arquivo bem grande
- Repare que embaixo, temos um indicador de qual linha o cursor está (1,1). Nesse caso, ele está na primeira letra da primeira linha.
- Porém, mesmo com esse indicador, podemos nos confundir no momento de analisar em qual linha estamos.
```bash
azureuser@vm-linux-meus-estudos-east-us:~/Arquivos$ sudo vi /var/log/syslog
2025-06-04T18:22:27.211403+00:00 vm-linux-meus-estudos-east-us systemd-fsck[160]: cloudimg-rootf      s: clean, 78071/327680 files, 436149/655099 blocks
2025-06-04T18:22:27.211504+00:00 vm-linux-meus-estudos-east-us systemd-modules-load[158]: Insert      ed module 'msr'
2025-06-04T18:22:27.211512+00:00 vm-linux-meus-estudos-east-us systemd-modules-load[158]: Insert      ed module 'dm_multipath'
2025-06-04T18:22:27.211517+00:00 vm-linux-meus-estudos-east-us systemd[1]: Mounted dev-hugepages      .mount - Huge Pages File System.
2025-06-04T18:22:27.211520+00:00 vm-linux-meus-estudos-east-us systemd[1]: Mounted dev-mqueue.mo      unt - POSIX Message Queue File System.
2025-06-04T18:22:27.211523+00:00 vm-linux-meus-estudos-east-us systemd[1]: Mounted sys-kernel-de      bug.mount - Kernel Debug File System.
2025-06-04T18:22:27.211527+00:00 vm-linux-meus-estudos-east-us systemd[1]: Mounted sys-kernel-tr      acing.mount - Kernel Trace File System.
2025-06-04T18:22:27.211530+00:00 vm-linux-meus-estudos-east-us systemd[1]: Finished keyboard-set      up.service - Set the console keyboard layout.
2025-06-04T18:22:27.211536+00:00 vm-linux-meus-estudos-east-us systemd[1]: Finished kmod-static-      nodes.service - Create List of Static Device Nodes.
@@@                                                                                          1,1           Top
```

Para ajustar isso, vamos habilitar o número de linhas através do comando:
- `:set nu`

Feito isso, agora temos um indicador a esquerda que informa o número da linha
```bash
    1 2025-06-04T18:22:27.211403+00:00 vm-linux-meus-estudos-east-us systemd-fsck[160]: cloudimg-rootf      s: clean, 78071/327680 files, 436149/655099 blocks
    2 2025-06-04T18:22:27.211504+00:00 vm-linux-meus-estudos-east-us systemd-modules-load[158]: Insert      ed module 'msr'
    3 2025-06-04T18:22:27.211512+00:00 vm-linux-meus-estudos-east-us systemd-modules-load[158]: Insert      ed module 'dm_multipath'
    4 2025-06-04T18:22:27.211517+00:00 vm-linux-meus-estudos-east-us systemd[1]: Mounted dev-hugepages      .mount - Huge Pages File System.
    5 2025-06-04T18:22:27.211520+00:00 vm-linux-meus-estudos-east-us systemd[1]: Mounted dev-mqueue.mo      unt - POSIX Message Queue File System.
    6 2025-06-04T18:22:27.211523+00:00 vm-linux-meus-estudos-east-us systemd[1]: Mounted sys-kernel-de      bug.mount - Kernel Debug File System.
    7 2025-06-04T18:22:27.211527+00:00 vm-linux-meus-estudos-east-us systemd[1]: Mounted sys-kernel-tr      acing.mount - Kernel Trace File System.
    8 2025-06-04T18:22:27.211530+00:00 vm-linux-meus-estudos-east-us systemd[1]: Finished keyboard-set      up.service - Set the console keyboard layout.
    9 2025-06-04T18:22:27.211536+00:00 vm-linux-meus-estudos-east-us systemd[1]: Finished kmod-static-      nodes.service - Create List of Static Device Nodes.
   10 2025-06-04T18:22:27.211539+00:00 vm-linux-meus-estudos-east-us systemd[1]: Finished lvm2-monitor      .service - Monitoring of LVM2 mirrors, snapshots etc. using dmeventd or progress polling.
   11 2025-06-04T18:22:27.211542+00:00 vm-linux-meus-estudos-east-us systemd[1]: modprobe@configfs.ser      vice: Deactivated successfully.
```

E para de deslocar entre as linhas? Por exemplo, imagine que precisamos acessar a linha 500, vamos visualiza-la mais facil graças a numeração, mas mesmo assim seria trabalhoso se deslocar até la manualmente. Podemos nos deslocar rapidamente apenas passando o número da linha desejeda no modo de comando. 
- Por exemplo, vamos navegar para a linha 500
```bash
  494 2025-06-04T18:22:27.212489+00:00 vm-linux-meus-estudos-east-us kernel: PCI: System does not supp      ort PCI
  495 2025-06-04T18:22:27.212491+00:00 vm-linux-meus-estudos-east-us kernel: vgaarb: loaded             496 2025-06-04T18:22:27.212492+00:00 vm-linux-meus-estudos-east-us kernel: clocksource: Switched to       clocksource hyperv_clocksource_tsc_page                                                           497 2025-06-04T18:22:27.212492+00:00 vm-linux-meus-estudos-east-us kernel: VFS: Disk quotas dquot_6.      6.0                                                                                               498 2025-06-04T18:22:27.212493+00:00 vm-linux-meus-estudos-east-us kernel: VFS: Dquot-cache hash tab      le entries: 512 (order 0, 4096 bytes)                                                             499 2025-06-04T18:22:27.212496+00:00 vm-linux-meus-estudos-east-us kernel: AppArmor: AppArmor Filesy      stem Enabled                                                                                      500 2025-06-04T18:22:27.212498+00:00 vm-linux-meus-estudos-east-us kernel: pnp: PnP ACPI init
  501 2025-06-04T18:22:27.212508+00:00 vm-linux-meus-estudos-east-us kernel: pnp: PnP ACPI: found 3 de      vices
  502 2025-06-04T18:22:27.212509+00:00 vm-linux-meus-estudos-east-us kernel: clocksource: acpi_pm: mas      k: 0xffffff max_cycles: 0xffffff, max_idle_ns: 2085701024 ns
  503 2025-06-04T18:22:27.212509+00:00 vm-linux-meus-estudos-east-us kernel: NET: Registered PF_INET p      rotocol family
  504 2025-06-04T18:22:27.212509+00:00 vm-linux-meus-estudos-east-us kernel: IP idents hash table entr      ies: 65536 (order: 7, 524288 bytes, linear)
  505 2025-06-04T18:22:27.212513+00:00 vm-linux-meus-estudos-east-us kernel: tcp_listen_portaddr_hash       hash table entries: 2048 (order: 3, 32768 bytes, linear)
@@@
:500                                                                                500,1          3%
```

### Copiar, colar e apagar textos

Não seríamos ninguém sem a possibilidade de copiar e colar textos. Você não 
usaria o vi se isso não fosse possível (bem, nem eu), e, talvez, muitos falem mal do 
vi justamente por não saberem como fazer. 

#### Copiando uma linha inteira
Vamos começar copiando uma linha inteira. Vá para a primeira linha (:1 em 
modo de comandos) e digite `yy` (de yank ou “arrancar”) 
no modo de comando. Embora não tenha avisado, ele copiou a primeira 
linha. 

>um único [y] também copiaria, mas ele copia um parágrafo (ou melhor, até achar uma linha em branco)

Podemos avançar para a última linha do arquivo (experiente `G` em modo de 
comando) e digite `p` (de paste ou colar):
```
```

#### Copiando varias linhas
Para copiar um número específico de linhas, basta colocar a quantidade antes 
do comando, ou seja, digitar `10yy` em modo de comando fará o vi copiar dez linhas, 
sendo a linha em que você está e as nove seguintes. 
- Copiando varias linhas, o vi retorna uma mensagem informando quantas linhas foi copiada

```bash
Apos apertar a tecla i, podemos digitar o conteudo do arquivo Apertei a tecla a para continuar de onde parei Apertei A para ir para o final da linha
Apertei o e fui para a linha debaixo
Agora estou editando livremente
A ideia é adicionar mais linha nesse arquivo

TODOS OS COMANDOS ABAIXO DEVEM SER FEITOS COM O VIM NO MODO DE COMANDO
PRESSIONE A TECLA ENTER PARA EXECUTA-LOS

Para salvar um arquivo - :w
Para salvar e sair de um arquivo - :wq ou :x
Para salvar o conteudo em outro arquivo - :w <nome_outro_arquivo>
Para salvar um intervalo de linhas em outro arquivo - :2,5 <nome_outro_arquivo>
                                                                                             7 lines yanked                                                                      1,1           All
```

#### Copiando trechos
Você pode copiar trechos de linha também. As teclas `yw` (de word ou 
palavra) copiarão uma única palavra (ou seja, até o vi achar um caracter de espaço), 
`y$` copiará do ponto em que o cursor esteja até o final da linha, enquanto `y^` faz o contrário, copiará de onde estivermos até o início da linha.
- `yw`:
- `y$`:
- `y^`:

#### Apagar linhas

Para apagar textos, os procedimentos são os mesmos usados para copiar, 
mas você substitui o “y” por “d” (de delete ou apagar); ou seja, 
- digitar `dd` em modo comando apaga uma única linha, enquanto `10dd` apaga a linha em que você está e mais nove
- `dw` apaga a palavra em que você está
- `d$` do ponto em que está até o final da linha, enquanto `d^` apaga de onde está até o início.

> Apagou algo errado? Digite `u` e `ENTER` (de undo ou desfazer); com `u` você pode desfazer o último comando, e APENAS O ÚLTIMO; Se for o penúltimo, danou-se. 

>Quer recortar? Use os comandos `d` também. Talvez você se pergunte: “ué, dd não é para apagar uma linha?”. Na verdade, ele recorta a linha. Se você usar o `p` (de paste) na sequência, ele realiza um recortar-e-colar

### Loalizar e substituir palavras
É indispensável saber fazer uma busca por palavra-chave em um documento. 
Para fazer isso no vi, digite `/` (em modo de comando), a palavra que você deseja e `ENTER`, como você pode ver no exemplo.
- Digite `n` para pular para a próxima ocorrência.
- Digite `N` para voltar para a anterior.

```bash
 8364 2025-06-09T21:54:40.239460+00:00 vm-linux-meus-estudos-east-us systemd[1]: Finished fwupd-refres      h.service - Refresh fwupd metadata and update motd.
 8365 2025-06-09T21:54:40.389684+00:00 vm-linux-meus-estudos-east-us systemd[1]: Reloading requested f      rom client PID 4799 ('systemctl') (unit session-52.scope)...
 8366 2025-06-09T21:54:40.389768+00:00 vm-linux-meus-estudos-east-us systemd[1]: Reloading...          8367 2025-06-09T21:54:40.676750+00:00 vm-linux-meus-estudos-east-us systemd[1]: Reloading finished in       286 ms.                                                                                         8368 2025-06-09T21:54:40.719938+00:00 vm-linux-meus-estudos-east-us systemd[1]: Starting man-db.servi      ce - Daily man-db regeneration...                                                                8369 2025-06-09T21:54:40.742710+00:00 vm-linux-meus-estudos-east-us systemd[1]: Starting apache2.serv      ice - The Apache HTTP Server...                                                                  8370 2025-06-09T21:54:40.791638+00:00 vm-linux-meus-estudos-east-us systemd[1]: Started apache2.servi      ce - The Apache HTTP Server.                                                                     8371 2025-06-09T21:54:40.930751+00:00 vm-linux-meus-estudos-east-us systemd[1]: Reloading requested f      rom client PID 4957 ('systemctl') (unit session-52.scope)...                                     8372 2025-06-09T21:54:40.930867+00:00 vm-linux-meus-estudos-east-us systemd[1]: Reloading...
 8373 2025-06-09T21:54:41.262962+00:00 vm-linux-meus-estudos-east-us systemd[1]: Reloading finished in       332 ms.
 8374 2025-06-09T21:54:41.332593+00:00 vm-linux-meus-estudos-east-us systemd[1]: Starting apache-htcac      heclean.service - Disk Cache Cleaning Daemon for Apache HTTP Server...                           8375 2025-06-09T21:54:41.335909+00:00 vm-linux-meus-estudos-east-us systemd[1]: Started apache-htcach      eclean.service - Disk Cache Cleaning Daemon for Apache HTTP Server. 

/apache
```
#### Substituindo palavras
Para substituir termos no documento? Podemos usar a sintaxe `%s/palavra_atual/palavra_nova/forma_de_substituir`. Vamos destrinchar esse comando:
- `%s`: Indica a troca de palavras
- `/palavra_atual`: A palavra que você deseja trocar
- `/palavra_nova`: A palavra que será utilizada para substituir
- `forma_de_substituir`: Isso na verdade refere-se ao comportamente da troca. Existem alguns:
    - `g`: Substituição global, ou seja, para todas as ocorrências.
    - `c`: SOlicita confirmação para cada substituição; se você remover o “c” do comando, ele troca todas as ocorrências sem a maior cerimônia
    - OBS: Você pode combinar os dois comportamentos, ou seja, aplicar a substituição para todas as ocorrências, porém solicitando confirmação para todas elas.

Na Linha de comando mostro como substituir todas as ocorrências da palavra “apache” por "nova-palavra"
- Nesse caso, será aplicado para todas as palavras (globalmente) e solicitar a confirmação
```bash
:%s/apache/nova-palavra/gc
```
Ao executar o comando com o `c`, o vi vai solicitar a confirmação de cada palavra. Nessa resposta, podemos definir que:
- y: Sim, ou seja, a troca sera aplicada para essa ocorrência
- n: Não, ou seja, a troca não será aplicada para essa ocorrência e vai partir para proxima
- a: All, 
```bash
 8364 2025-06-09T21:54:40.239460+00:00 vm-linux-meus-estudos-east-us systemd[1]: Finished fwupd-refres      h.service - Refresh fwupd metadata and update motd.
 8365 2025-06-09T21:54:40.389684+00:00 vm-linux-meus-estudos-east-us systemd[1]: Reloading requested f      rom client PID 4799 ('systemctl') (unit session-52.scope)...
 8366 2025-06-09T21:54:40.389768+00:00 vm-linux-meus-estudos-east-us systemd[1]: Reloading...          8367 2025-06-09T21:54:40.676750+00:00 vm-linux-meus-estudos-east-us systemd[1]: Reloading finished in       286 ms.                                                                                         8368 2025-06-09T21:54:40.719938+00:00 vm-linux-meus-estudos-east-us systemd[1]: Starting man-db.servi      ce - Daily man-db regeneration...                                                                8369 2025-06-09T21:54:40.742710+00:00 vm-linux-meus-estudos-east-us systemd[1]: Starting apache2.serv      ice - The Apache HTTP Server...                                                                  8370 2025-06-09T21:54:40.791638+00:00 vm-linux-meus-estudos-east-us systemd[1]: Started apache2.servi      ce - The Apache HTTP Server.                                                                     8371 2025-06-09T21:54:40.930751+00:00 vm-linux-meus-estudos-east-us systemd[1]: Reloading requested f      rom client PID 4957 ('systemctl') (unit session-52.scope)...                                     8372 2025-06-09T21:54:40.930867+00:00 vm-linux-meus-estudos-east-us systemd[1]: Reloading...
 8373 2025-06-09T21:54:41.262962+00:00 vm-linux-meus-estudos-east-us systemd[1]: Reloading finished in       332 ms.
 8374 2025-06-09T21:54:41.332593+00:00 vm-linux-meus-estudos-east-us systemd[1]: Starting apache-htcac      heclean.service - Disk Cache Cleaning Daemon for Apache HTTP Server...                           8375 2025-06-09T21:54:41.335909+00:00 vm-linux-meus-estudos-east-us systemd[1]: Started apache-htcach      eclean.service - Disk Cache Cleaning Daemon for Apache HTTP Server.                             @@@
 
 replace with nova-palavra (y/n/a/q/l/^E/^Y)?  
```
