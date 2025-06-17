
## Introdução ao VI

Fica absolutamente impossível de admoinistrar um servidor Linux sem conhecer um editor de textos que possa rodar em modo terminal. Isso porque, a grande maioria dos arquivos presentes no Linux/Unix utilizam arquivos de texto para parametrizar os serviços, configurar elemetnos de grupo, entre outros. Portanto, através dos editores de texto podemos manipular várias configurações. 

Um dos editores de texto mais utilizados no Linux é o vi (lê-se "vi ai") ou vim - VI IMproved (lê-se "vi ai am"), que é uma espécie de vi "melhorado". Com o vi, podemos acessar qualquer arquivo e criar arquivos.

Embora existam outros editores de texto, como o emacs ou o nano (que é bem fácil de usar), o vi possui uma infinidade de recursos interessantes e poderosos. <br>
Praticamente todas as letras minúsculas e maiúsculas disparam comandos no 
editor; portanto, para que este tutorial não fique muito complexo (e chato), vou me 
restringir às funcionalidades mais comuns e úteis.

Normalmente, os sistemas Linux já possuem esse pacote instalado. Mas caso não tenha e precise instalar, utilize o comando `sudo apt install vim`

## Comandos VI
Abaixo, uma lista de comandos interessantes para se usar no vi

>Vale ressaltar que o vi é case sensitive, ou seja, ele diferencia letras minusculas e maisculas. `a` tem um comportamente diferente de `A`, portanto, dobre a atenção ao utilizar os comandos. 

### Entrando no VI
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

### q - saindo do VI
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

#### q! - sair do arquivo sem salvar alterações
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

## Modo de comando e modo de edição
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

### 1. Tecla i
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

### 2. Tecla a
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

### 3. Tecla A
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

### 4. Tecla o
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

### :set nu - numeração de linhas e navegação
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

## Copiar, colar e apagar textos

Não seríamos ninguém sem a possibilidade de copiar e colar textos. Você não 
usaria o vi se isso não fosse possível (bem, nem eu), e, talvez, muitos falem mal do 
vi justamente por não saberem como fazer. 

### Copiando uma linha inteira
Vamos começar copiando uma linha inteira. Vá para a primeira linha (:1 em 
modo de comandos) e digite `yy` (de yank ou “arrancar”) 
no modo de comando. Embora não tenha avisado, ele copiou a primeira 
linha. 

>um único [y] também copiaria, mas ele copia um parágrafo (ou melhor, até achar uma linha em branco)

Podemos avançar para a última linha do arquivo (experiente `G` em modo de 
comando) e digite `p` (de paste ou colar):
```
```

### Copiando varias linhas
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

### Copiando trechos
Você pode copiar trechos de linha também. As teclas `yw` (de word ou 
palavra) copiarão uma única palavra (ou seja, até o vi achar um caracter de espaço), 
`y$` copiará do ponto em que o cursor esteja até o final da linha, enquanto `y^` faz o contrário, copiará de onde estivermos até o início da linha.
- `yw`:
- `y$`:
- `y^`:

### Apagar linhas

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
### Substituindo palavras
Para substituir termos no documento? Podemos usar a sintaxe `%s/palavra_atual/palavra_nova/forma_de_substituir`. Vamos destrinchar esse comando:
- `%s`: Indica a troca de palavras
- `/palavra_atual`: A palavra que você deseja trocar
- `/palavra_nova`: A palavra que será utilizada para substituir
- `forma_de_substituir`: Isso na verdade refere-se ao comportamente da troca. Existem alguns:
    - `g`: Substituição global, ou seja, para todas as ocorrências.
    - `c`: Solicita confirmação para cada substituição; se você remover o “c” do comando, ele troca todas as ocorrências sem a maior cerimônia
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