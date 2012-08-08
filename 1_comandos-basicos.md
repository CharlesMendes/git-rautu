# Comandos básicos do Git

Após a configuração inicial, podemos criar um repositório e começar a trabalhar com arquivos que serão "rastreados" pelo Git.

Um repositório (ou *repo*) é um diretório local onde os arquivos de um projeto serão armazenados. O Git funciona normalmente em um repositório local, e não é necessário haver um servidor central para o armazenamento dos arquivos (como é comum em *sistemas controladores de versão centralizados* como CVS e Subversion). No entanto, o uso de um servidor central é fundamental para projetos colaborativos e de software livre.

Um servidor do Git pode ser local (um servidor privado por exemplo), ou remoto. Um servidor remoto é mais simples e fácil de ser utilizado pois já existem alguns serviços disponíveis para hospedar repositórios do Git. O GitHub é um destes servidores, mas existem [vários outros disponíveis](https://git.wiki.kernel.org/index.php/GitHosting). A escolha de um deles em particular não irá interferir no funcionamento do Git, pois os comandos são os mesmos. A diferença se dá apenas na visualização *on-line* e outras facilidades que cada servidor apresenta. Aqui vamos utilizar o GitHub.

## Criando um repositório

Para criar um repositório no GitHub, clique no botão `New repository` disponível na sua página inicial, especifique um nome e opcionalmente uma descrição. Aqui neste exemplo vou criar um repo chamado `git-teste`, que está disponível em (https://github.com/fernandomayer/git-teste). 

Até aqui o repositório foi criado no GitHub, e isso é importante pois agora temos um endereço no servidor para onde vamos enviar os arquivos criados localmente. Agora é a hora de criar um diretório local e iniciar o Git fazendo ele se comunicar com o servidor. Para isso fazemos

```bash
# cria o diretório
$ mkdir git-teste
# entra
$ cd git-teste
# inicia o git
$ git init
```

O comando `git init` serve para "iniciar" o rastreamento de arquivos pelo Git. Esse comando cria um diretório (oculto) `.git`, contendo as configurações necessárias para o funcionamento do sistema. Esse comando só é necessário uma vez.

Para informar o Git que este diretório deve se comunicar com o servidor do GitHub criado acima fazemos

```bash
$ git remote add origin git@github.com:fernandomayer/git-teste.git
```

O comando `git remote add` serve para adicionar um repositório "remoto", que por padrão o Git chama de `origin`, e que nada mais é do que um "atalho" para o endereço do servidor. Note que neste caso estamos usando um endereço no formato do `SSH`, mas poderia ser também o endereço `https`.

Nesse ponto podemos utilizar o comando `git status` para verificar o que está contecendo no repositório

```bash
$ git status 
# On branch master
#
# Initial commit
#
nothing to commit (create/copy files and use "git add" to track)
```

Duas coisas chamam a atenção: o Git está mostrando que estamos em um *branch* (ramo) chamado `master`, e que não há nada para ser enviado ou adicionado ao repositório (*commit*). Um branch é como uma "linha de desenvolvimento", sendo que sempre deve existir uma principal (que por padrão é chamada de `master`), mas podem existir muitas outras. Por enquanto vamos trabalhar apenas com o branch principal.

Para começar a usar o Git precisamos criar arquivos. No GitHub, um arquivo chamdo `README` serve para identificar um projeto e aparece autoomaticamente na página principal do repositório. Por isso, vamos criar esse arquivo (usando um editor de texto qualquer) e adicionar algum texto como

```
Repositório criado para testes com o GitHub.
```

Depois de salvar o arquivo podemos ver novamente o status

```bash
$ git status 
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	README
nothing added to commit but untracked files present (use "git add" to track)
```

Agora vemos que o Git automaticamente identificou que existe um arquivo criado no repositório, mas ele ainda não está sendo rastreado. Se quisermos que o Git rastreie todas as modificações de um arquivo temos que adicioná-lo ao repositório com o comando `git add`

```bash
$ git add README
```

E o status irá mostrar que o arquivo foi adicionado, mas ainda não foi enviado para o repositório local

```bash
$ git status 
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#	new file:   README
#
```

Nesse momento, o arquivo está em uma *staging area*, ou um lugar pronto para ser enviado ao repositório local. Até aqui o arquivo não faz parte do repositório do Git! Para isso temos que "enviá-lo", ou fazer um *commit*

```bash
$ git commit -m 'primeiro commit'
[master (root-commit) 1490ab0] primeiro commit
 1 file changed, 1 insertion(+)
 create mode 100644 README
$ git status 
# On branch master
nothing to commit (working directory clean)
```

Agora o arquivo faz parte do repositório do Git, mas apenas *localmente*. Para enviá-lo ao servidor remoto (já configurado), usamos o comando `git push` com dois argumentos. O primeiro indica o servidor remoto (`origin`), e o segundo o branch (`master`)

```bash
$ git push origin master 
Counting objects: 3, done.
Writing objects: 100% (3/3), 258 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To git@github.com:fernandomayer/git-teste.git
 * [new branch]      master -> master
```