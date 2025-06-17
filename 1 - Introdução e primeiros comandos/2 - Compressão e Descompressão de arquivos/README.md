## Compressão de arquivos

Alguns comandos úteis na hora de comprimir/descomprimir arquivos. Um caso comum é o arquivo zip.


### unzip - descompactar arquivos zip

>Iremos utilizar uma biblioteca externa para realizar a descompressão de arquivos. Portanto, utilize o comando `sudo apt install unzip`

Existirão ocasiões em que você precisará descompactar arquivos do tipo 
“zip”, popularmente conhecido como “deszipar”, e o comando é este: 
- `unzip -v arquivoZipado.zip`

O parâmetro “`-v`” permitirá listar cada arquivo e diretório que ele está 
descompactando. Sem o parâmetro, o comando faz o trabalho e devolve o prompt 
da maneira mais silenciosa possível

É interessante realizar um teste antes de descomprimir um arquivo. Com o parâmetro `-t`, é possível fazer um "teste" e verificar os arquivos que serão descomprimidos, caso haja sucesso.
- `unzip -tv arquivoZipado.zip`

>A Linha de comando $ `unzip -tv arquivoZipado.zip` exibe todos os arquivos e diretórios de arquivoZipado.zip e testa a integridade dos arquivos sem, no entanto, realmente descompactá-los, o que pode ser muito útil em vários casos.

>Existe também o comando chamado `unrar`, que faz a mesma coisa que o unzip, porém para arquivos .RAR

### tar - empacotar arquivos
O comando tar é utilizado para empacotar arquivos, ou seja, transformar vários arquivos em um único arquivo compactado, facilitando o armazenamento e a distribuição. Ele pode ser usado com diferentes algoritmos de compressão.

Primeiro, instale as ferramentas necessárias para compressão, caso ainda não tenha feito isso:
```bash
sudo apt install gzip bzip2
```

Comando absolutamente indispensável, tar permite empacotar e 
desempacotar arquivos, ou seja, vários arquivos se tornam um só e vice-versa. 

Quando vamos comprimir mais de um arquivo, é necessário "empacota-los", ou seja, pegar todos os arquivos e coloca-los em um so. Para isso, usamos o comando `tar`. Esse comando pode ser aliado com alguns parâmetros interessantes:
- `-c`: Cria um novo arquivo comprimido.
- `-z`: Usa o algoritmo de compressão `gzip`.
    - `-j`: Usa o algoritmo de compressão `bzip2`
- `-v`: Exibe os arquivos que estão sendo incluídos no pacote (modo verboso).
- `-f`: Permite nomear o arquivo comprimido

Além disso, é possível especificar 2 extensões para o arquivo, sendo que:
- `tar`: Indica o arquivo comprimido
- `gz`: Indica o protocolo de zipagem do arquivo, nesse caso é o gzip.

O tar pode ser combinado com diferentes tipos de compressão. Por exemplo:
- `tar -czvf arquivo-comprimido.tar.gz`: Usando gzip para compressão.
- `tar -czvf arquivo-comprimido.tgz`: Uma variante, com a mesma funcionalidade.

>Para usar o outro algoritmo de compressão bzip2, basta trocar o parâmetro `z` pelo `j` e modificar a extensão de `gz` para `bz2`, por exemplo: `tar -cjvf arquivo-comprimido.tar.bz2 *.txt`

Em seguida, é necessário informar quais arquivos farão parte da compressão. É possível especificar os arquivos manualmente, ou determinar vários arquivos de acordo com uma extesão específica. 

Suponha que temos os seguintes arquivos e diretórios:
```bash
//Verificando os arquivos presentes
:~$ ls 
Arquivo.txt   Arquivos   Downloads           Pictures   Videos          novoDiretorio3
Arquivo2.txt  Desktop    Music               Public     novoDiretorio   thinclient_drives
Arquivo3.txt  Documents  NomeAtualizado.txt  Templates  novoDiretorio2
```

Para empacotar todos os arquivos .txt, você pode usar o comando:
- `tar -czvf arquivo-comprimido.tar.gz *.txt`
```bash
## Empacotando todos os arquivos .txt
:~$ tar -czvf arquivo-comprimido.tar.gz *.txt
Arquivo.txt
Arquivo2.txt
Arquivo3.txt
NomeAtualizado.txt

## Resultado
:~$ ls
Arquivo.txt   Desktop    NomeAtualizado.txt  Videos                     novoDiretorio3
Arquivo2.txt  Documents  Pictures            arquivo-comprimido.tar.gz  thinclient_drives
Arquivo3.txt  Downloads  Public              novoDiretorio
Arquivos      Music      Templates           novoDiretorio2
```

>É importante ressaltar que no Linux as extensões de arquivo (.txt, .rar, .md, .jpeg, etc) sãp opcionais. Porém, é importante utiliza-las para manter uma convenção padrão e organizada

### tar - desempacotar arquivos

Realizamos a compressão dos arquivos com os 2 algoritmos: gzip e bzip2
```bash
:~$ ls -la arquivo-comprimido*
-rw-rw-r-- 1 azureuser azureuser 204 Jun  9 19:12 arquivo-comprimido.tar.bz2
-rw-rw-r-- 1 azureuser azureuser 185 Jun  9 19:06 arquivo-comprimido.tar.gz
```

Além disso, movemos cada um oara suas respecivas pastas para melhor organização no momento de descompressão.
```bash
:~$ ls
Arquivo.txt   Desktop    NomeAtualizado.txt  arquivo-comprimido.tar.bz2  thinclient_drives
Arquivo2.txt  Documents  Pictures            arquivo-comprimido.tar.gz
Arquivo3.txt  Downloads  Public              novoDiretorio
Arquivos      GZIP       Templates           novoDiretorio2
BZ2           Music      Videos              novoDiretorio3

:~$ mv arquivo-comprimido.tar.gz GZIP -- MOVENDO O ARQUIVO 
:~$ mv arquivo-comprimido.tar.bz2 BZ2 -- MOVENDO O ARQUIVO

:~$ cd GZIP/
:~/GZIP$ ls
arquivo-comprimido.tar.gz
```

Para descomprimir o arquivo, a sintaxe é semelhante a compressão. Então, temos os mesmos parâmetros do comando anterior, com a diferença do parametro `x` no lugar do `c`. Por exemplo:
`tar -xjvf arquivo-comprimido.tar.gz`. Além disso, agora não é mais necessário repassar os arquivos que serão empacotados, mas sim o nome do arquivo que será descomprimido

- Assim como o comando `unzip`, é possível utilizar o parâmetro `t` para testar antes de realizar a descompressão.
```bash
## TESTANDO A DESCOMPRESSAO
:~/GZIP$ tar -tzvf arquivo-comprimido.tar.gz
-rw-rw-r-- azureuser/azureuser 0 2025-06-09 18:47 Arquivo.txt
-rw-rw-r-- azureuser/azureuser 0 2025-06-09 18:47 Arquivo2.txt
-rw-rw-r-- azureuser/azureuser 0 2025-06-09 19:05 Arquivo3.txt
-rw-rw-r-- azureuser/azureuser 0 2025-06-09 15:43 NomeAtualizado.txt

## REALIZANDO A DESCOMPRESSAO
:~/GZIP$ tar -xzvf arquivo-comprimido.tar.gz
Arquivo.txt
Arquivo2.txt
Arquivo3.txt
NomeAtualizado.txt

## RESULTADO
:~/GZIP$ ls
Arquivo.txt  Arquivo2.txt  Arquivo3.txt  NomeAtualizado.txt  arquivo-comprimido.tar.gz
```

>Para descomprimir o outro arquivo comprimido com bzip2, basta trocar o parâmetro `z` pelo `j` e modificar a extensão de `gz` para `bz2`, por exemplo: `tar -xjvf arquivo-comprimido.tar.bz2`