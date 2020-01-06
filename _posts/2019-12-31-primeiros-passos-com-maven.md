---
layout: post
title: Primeiros passos com Maven
postid: getting-started-with-maven
categories: Tools Java
---

Quem começa a estudar a linguagem de programação Java, logo de cara se depara
com o conceito de [classpath](http://www.guj.com.br/articles/108), ou seja,
onde residirão as classes que compõem os nossos programas.

Conforme vamos criando projetos Java maiores e mais complexos, essa questão
vai ficando ainda mais complicada.  Para utilizar funcionalidades de bibliotecas
Java devemos incluí-las em nosso projeto.  Com o tempo, manter um conjunto grande
de bibliotecas, algumas com suas próprias dependências e outras talvez até
incompatíveis entre si, torna-se uma tarefa impraticável de realizar
manualmente.

É para atender a essa necessidade que surgem ferramentas para gerenciamento de
dependências como o [Maven](http://maven.apache.org), com o qual daremos nossos
primeiros passos neste artigo.

## O que é o Maven?

O Maven é uma ferramenta para se padronizar a execução de tarefas em projetos
Java.  É utilizada principalmente para automatizar o procedimento de compilação
e _build_, além de abstrair as chamadas dependências do projeto dentre outras coisas.
O Maven faz isso identificando todas as bibliotecas informadas que serão
utilizadas em nosso projeto, baixando-as e organizando-as internamente para uso
quando necessário.  Assim, é preciso estar conectado à internet e as primeiras
execuções geralmente levam algum tempo para baixar as bibliotecas que são
dependências do projeeto além daquelas que o próprio Maven utiliza.

Apesar de ter versões e suportar também outras linguagens e plataformas, o Maven
é uma ferramenta bem consolidada no mundo Java.  É utilizado tanto por ambientes
de desenvolvimento integrados (IDEs) complexos como o [Eclipse](http://eclipse.org),
mas também é bastante poderoso para permitir a você lidar tranquilamente com projetos
Java apenas na linha de comando.

A melhor maneira de começar com Maven é pôr a mão na massa.  Para isso, vamos
instalá-lo em nossa máquina e usá-lo para criar um projeto simples, explorado
os conceitos envolvidos em cada passo.

## Pré-requisito

O Maven é uma ferramenta escrita em Java.  Portanto, ter a máquina virtual Java
instalada é um pré-requisito para se instalar o Maven.  E como é uma ferramenta
para desenvolvedor, é necessária a versão JDK (só o Java JRE não é suficiente).
Para se verificar se o Java está instalado, execute um `javac -version` na linha
de comandos do seu sistema.

```
$ javac -version
javac 1.8.0_45
```
No meu caso, tenho o Java padrão da Oracle versão 1.8.  Se você receber uma
mensagem de erro de comando ou arquivo não encontrado, acesse <http://java.sun.com>,
faça o download e proceda a instalação do Java JDK antes de continuar.

## Instalando o Maven

O procedimento de instalação do Maven é bem trivial: apenas vá até o site
<http://maven.apache.org/download.html> e baixe a versão mais recente do Maven
(no momento da publicação deste artigo, esta era a versão 3.3.3).  A seguir,
simplesmente descompacte o arquivo baixado em algum local de sua escolha em seu
computador.  Se você usa Windows, atente apenas para não deixar o Maven em
local em que alguma pasta tenha espaço em branco no nome.

Coloque também o caminho da subpasta `bin` de dentro da pasta do Maven no PATH
de seu sistema operacional para facilitar seu uso.  No Bash do Linux, no meu
caso por exemplo, supondo que vá deixar o Maven descompactado em
`/home/marcelo/apache-maven-3.3.3`, bastaria executar

```
$ export PATH=$PATH:/home/marcelo/apache-maven-3.3.3/bin
```
Se você usa Windows, definir a variável de ambiente PATH [também é bem simples]
(http://pt.stackoverflow.com/questions/5024/como-mudar-o-path-nos-windows).

Para conferir se tudo está direito, experimente abrir um novo terminal ou prompt
de comandos e digitar `mvn -version`.

```
$ mvn -version
Apache Maven 3.3.3 (7994120775791599e205a5524ec3e0dfe41d4a06; 2015-04-22T08:57:37-03:00)
Maven home: /home/marcelo/apache-maven-3.3.3
Java version: 1.8.0_45, vendor: Oracle Corporation
Java home: /usr/lib/jvm/java-8-oracle/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "3.13.0-53-generic", arch: "i386", family: "unix"
```

Se ao invés de uma saída semelhante a esta você viu alguma mensagem de erro,
assegure-se de que seu Java JDK esteja instalado corretamente e reveja as
orientações acima conferindo se baixou, descompactou e definiu o PATH corretamente.

Se tudo deu certo, estamos prontos para criarmos nosso primeiro projeto com Maven.

## Criando um projeto com Maven

Para criar um projeto Maven básico, pode-se executar o seguinte comando:

```
$ mvn archetype:generate -DartifactId=ola-mundo -DgroupId=info.marceloandrade \
-DinteractiveMode=false
```

Uma primeira característica que você perceberá é que o Maven é bastante
verboso.  Ao executar este comando conectado à internet você verá que o Maven
vai indicar que está fazendo o download de alguns arquivos.  Estes arquivos são
as bibliotecas necessárias para o funcionamento do próprio Maven além das
dependências utilizadas pelo projeto Java que está sendo criado, quando for o
caso.  Depois de alguns instantes, se não ocorrer nenhum problema o comando
termina, mostrando que concluiu com uma mensagem de sucesso, como

```
...
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 32.322 s
[INFO] Finished at: 2015-06-01T00:25:22-03:00
[INFO] Final Memory: 9M/24M
[INFO] ------------------------------------------------------------------------
```
Vale lembrar ainda que os arquivos das bibliotecas baixadas ficam disponíveis
para uso pelo Maven, de forma que só precisam se baixadas na primeira vez.
Assim, se você for criar um próximo projeto Maven similar a este, o
procedimento todo não envolverá baixar as bibliotecas de novo e será bem mais
rápido.

Antes de analisarmos o que o Maven acabou de fazer ao criar nosso projeto,
vamos entender melhor o comando que acabamos de executar.

<dl>
  <dt><kbd>mvn</kbd></dt>
  <dd>O mvn é o próprio binário do Maven em si.</dd>

  <dt><kbd>archetype:generate</kbd></dt>
  <dd>O Maven chama os diversos tipos de projetos Java de "archetypes" (arquétipos).
  Este parâmetro <kbd>archetype:generate</kbd> é chamado de <i>goal</i>, que é
  a tarefa que se quer executar.  Neste caso está se invocando a tarefa <kbd>generate</kbd>
  disponível no plugin <kbd>archetype</kbd> do Maven.</dd>

  <dt><kbd>-DartifactId=ola-mundo</kbd></dt>
  <dd>O <kbd>-D</kbd> é o parâmetro que se usa para passar propriedades (pares de chave-valor)
  para o Java e que também se usa aqui.  O termo do Maven para projetos é "artifact".  E
  neste caso o nome do nosso projeto é dado pela propriedade "artifactId".</dd>

  <dt><kbd>-DgroupId=info.marceloandrade</kbd></dt>
  <dd>A propriedade `groupId` é usada para agrupar e dar unicidade para os projetos
  (como um <a href="http://pt.wikipedia.org/wiki/Namespace"><i>namespace</i></a>.
  Isso nos permitiria identificar unicamente nosso projeto nos repositórios do
  Maven, mesmo que nosso projeto tenha um nome possivelmente muito comum (como é
  o caso de nosso "ola-mundo").  Apesar de não ser uma regra, é uma convenção
  quase unânime no mundo Java que se utilize o domínio do site do programador de
  trás para frente como base para o valor do groupId.</dd>

  <dt><kbd>-DinteractiveMode=false</kbd></dt>
  <dd>O Maven também pode ser executado em modo interativo, isto é, pausando
  sua execução para solicitar que o usuário informe opções ou confirme as ações
  antes de executar.  Passando <kbd>interactiveMode</kbd> como false, fazemos o
  Maven assumir valores default para os parâmetros não-obrigatórios neste goal
  e executar até o fim sem solicitar nossa intervenção.</dd>
</dl>

Experimente depois executar este mesmo comando sem desabilitar o modo
interativo e veja que o Maven vai lhe solicitar que você defina algumas
informações adicionais como escolher o arquétipo do projeto que está criando
dentre a relação de todos os arquétipos possíveis ou que versão você quer dar
inicialmente ao seu projeto.

Verifique foi criada uma pasta para nosso projeto, neste caso chamada de
_ola-mundo_.  Vamos dar uma olhada na estrutura criada para este projeto.

## Estrutura do projeto

Como utilizamos o modo não-interativo, a estrutura de nosso projeto segue o
arquétipo padrão (que, a propósito, chama-se "maven-archetype-quickstart").

```
$ tree ola-mundo
ola-mundo
├── pom.xml
└── src
    ├── main
    │   └── java
    │       └── info
    │           └── marceloandrade
    │               └── App.java
    └── test
        └── java
            └── info
                └── marceloandrade
                    └── AppTest.java

9 directories, 3 files
```

A estrutura deste projeto padrão é simples.  Há apenas uma pasta `src` que irá
conter o código-fonte de nosso projeto.  Esta, por sua vez, contém outras duas
subpastas: `main` e `test`, respectivamente onde residem os códigos da
aplicação em si e os códigos dos testes correspondentes, visando incentivar com
isso a adoção de boas práticas de desenvolvimento de software.

Veja que o Maven criou uma classe padrão chamada `App.java` e sua classe de
teste correspondente `AppTest.java`.  Também perceba que o Maven utilizou o
valor do groupId para derivar a hierarquia de pacotes das classes de nosso
projeto.  São todas definições iniciais, mas você como programador pode (e
deve) fazer alterações e ajustes no seu projeto conforme bem desejar.  Vejamos
a classe `App.java` criada em `src/main/java/info/marceloandrade/`.

```java
package info.marceloandrade;

/**
 * Hello world!
 */
public class App {
    public static void main( String[] args ) {
        System.out.println( "Hello World!" );
    }
}
```

Como vêem, esse arquétipo "maven-archetype-quickstart" gera inicialmente esta
classe simples que apenas exibe `Hello World!` no console.  Como o objetivo deste
artigo é abordar conceitos do Maven e não construir um sistema complexo, este
projeto serve como bom ponto de partida.

O principal arquivo de qualquer projeto Maven é o `pom.xml`.  POM significa
_Project Object Model_ (projeto de modelo de objeto).  É um arquivo xml em que
são declaradas todas as informações sobre o projeto.  Há muita coisa para se
aprender neste arquivo mas que veremos noutra oportunidade.  Por hora, vamos
utilizar o Maven para compilar, empacotar e executar nosso projeto.

## Compilando e empacotando o projeto

Como dito, o Maven é uma forma padronizada de se fazer determinadas ações em
projetos Java. Java é uma linguagem muito assentada em padrões, de uma forma
geral.  Por exemplo, o padrão para se distribuir projetos Java é empacotá-los
em arquivos tipo `jar`.  Se você não sabe, arquivos `jar` nada mais são do qye
arquivos "zipados" contendo toda uma estrutura de projetos Java como o que acabamos
de criar.

Estando na pasta de nosso projeto, podemos empacotá-lo simplesmente com o
comando:

```
$ cd ola-mundo
$ mvn package
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building ola-mundo 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
...
-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running info.marceloandrade.AppTest
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.124 sec

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0

[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ ola-mundo ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 13.046 s
[INFO] Finished at: 2015-06-01T00:30:33-03:00
[INFO] Final Memory: 7M/18M
[INFO] ------------------------------------------------------------------------
```

Novamente, tal como anteriormente ao final de uma verbosa saída, a indicação de
que o _goal_ foi executado com sucesso.  Perceba também que no meio do processo
de empacotamento o Maven executa os testes que tenhamos escrito para nosso
projeto.

Um conceito interessante do Maven é que ele trata cada projeto com um **ciclo de
vida** composto por **fases**.  A execução de uma fase depende que todas as
fases anteriores do ciclo de vida tenham sido executadas com sucesso.  Este
comportamento reproduz sistematicamente o que acontece na realidade.  Por
exemplo, não é possível empacotar o projeto (i.e., fase _package_) se,
por exemplo, seus testes (fase _test_) estiverem falhando.  Da mesma forma, se o
código do projeto estiver com algum erro e não conseguir sequer ser compilado (_compile_),
nem adiantará tentarmos testá-lo, ou muito menos conseguiremos empacotá-lo.

{% comment %} TODO: incluir uma figura das fases e referenciá-la no parágrafo
abaixo "Mas não é muito...?" ao invés de listar todas as fases lá. {% endcomment %}

Abordaremos mais produndamente o ciclo de vida dos projetos Maven em artigos
futuros.  Por hora, veja que depois de termos executado com sucesso o comando
package, o Maven gerou um pacote correspondente a nosso projeto na pasta
`target`:

```
$ ls target/*.jar
ola-mundo-1.0-SNAPSHOT.jar
```

A pasta `target` foi criada e é onde o Maven, por padrão, gera os artefatos de
saída relacionados ao nosso projeto.  Além do pacote, outro artefato bem comum
que pode ser gerado aqui e é muito usado como fonte de documentação é o site do
projeto.  Experimente depois criar o site do projeto com o comando `mvn site` e
veja seu conteúdo.

Enfim, agora nosso projeto está devidamente empacotado.  O pacote .jar gerado
contém as classes de nosso projeto compiladas para serem executadas na máquina
virtual Java.  Como a classe `App.java` possui um método `main`, podemos
executá-la normalmente com:

```
$ java -classpath target/ola-mundo-1.0-SNAPSHOT.jar info.marceloandrade.App
Hello World!
```

Com este comando estamos dizendo que queremos executar a classe
`info.marceloandrade.App` que foi compilada e empacotada no arquivo
`ola-mundo-1.0-SNAPSHOT.jar`.  No jargão do mundo Java, o local no sistema de
arquivos onde se encontram classes compiladas é chamado de *classpath*, que é o
que estamos informando explicitamente ao comando `java` neste caso com a opção
`-classpath`.

_"Mas não é muito trabalho apenas para exibir um 'Hello World!'?"_, você pode
pensar.  Neste caso talvez sim, mas é bom lembrar que o objetivo do Maven é
padronizar a realização de tarefas em Java.  Em projeto maiores e mais complexos
(em que equipes diferentes fiquem responsáveis por módulos diferentes de um grande
sistema) não importa como os projetos estejam organizados, ou onde tenham sido
criados no sistema de arquivos nem nada do tipo.  Você sempre poderá compilar o
projeto projeto com `mvn compile`, executar testes com `mvn test` e empacotar o
projeto para distribuição com `mvn package`.

Nesse sentido, o Maven é uma ferramenta que também pode facilitar a tarefa de
integração de sistemas, e também por visar a automatização de tarefas, os conceitos
de *integração contínua* e até os mais modernos de implantação e *entrega contínua*.

Por hora é isso.  Já vimos bastante coisa mais ainda é só o começo.  Até os
próximos artigos.
