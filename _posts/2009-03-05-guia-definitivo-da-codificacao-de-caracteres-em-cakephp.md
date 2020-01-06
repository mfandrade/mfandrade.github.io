---
layout: post
postid: cakephp-charset-definitive-guide
title: Guia definitivo da codificação de caracteres em CakePHP
categories: OldButGold CakePHP
---

Codificação de caracteres é talvez o problema mais comum para quem está
iniciando no CakePHP.  Os “sintomas” podem ir desde simplesmente da exibição
de caracteres estranhos (como quadrados ou interrogações) onde deveriam estar
caracteres acentuados, até a não exibição de certos registros em listagens, por
exemplo.

## Entendendo o problema

A codificação de caracteres mais primitiva é a chamada codificação ASCII.
ASCII é uma codificação de 7 bits, ou (27) 128 caracteres.  Essa quantidade
de caracteres é mais que suficiente para representar as letras do alfabeto
inglês em maiúsculas e minúsculas (de a-z e de A-Z, ou 52 caracteres no total),
os dígitos de 0-9, alguns sinais de pontuação (como .;:?!@#$%&, e outros) além
de alguns caracteres não imprimíveis para funções de controle (como quebras de
linha, tabulações, espaçamento, fim de arquivo, etc).

Porém, não havia como representar, em ASCII, caracteres comuns nos idiomas
latinos, como os acentos comuns nos idiomas portugu<em>ê</em>s e
espa<em>ñ</em>ol.  Essa deficiência foi contornada com uma versão estendida da
tabela ASCII em 8 bits, ou 256 posições – o dobro da capacidade.

Mas a tabela ASCII estendida não era uma solução definitiva para codificações
de caracteres.  Principalmente com a popularização da Internet, tornou-se mais
do que necessário unificar as representações de caracteres.  Afinal, ainda
havia problema de compatibilização com caracteres para idiomas orientais tais
como 日本語 (japonês), Русский (russo), Ελληνικά (grego), العربية (árabe) e
muitos outros.

Uma solução adotada pelo organismo internacional de padronização – ISO –
consistia em definir o que se chamou de páginas de conjuntos de caracteres, que
eram padronizadas e específicas para cada um dos diferentes idiomas.  No padrão
da ISO, a página para o conjunto de caracteres dos idiomas latinos é a
ISO-8859-1.

Apesar de o uso de páginas de caracteres ter sido bastante difundido a partir
de sua adoção como padrão ISO, havia o inconveniente do gerenciamento de páginas de codificação.  Por exemplo, se você deseja ter um site
em idiomas com 4 codificações distintas, a rigor, deveria criar cada página,
salvando-as na codificação devida e configurando seu servidor e sua aplicação
para chavear entre as páginas de caracteres.  Esse tipo de tarefa, ainda que
simples, pode representar bastante trabalho.

Surgiu então a iniciativa Unicode, ou conjunto de caracteres universal.  Um
consórcio de diversas entidades se formou com a ideia de definir uma
representação unificada e padronizada para todos caracteres de todos os
diferentes idiomas.  Um grande desafio!  Mas hoje a maioria dos sites web que
estão no ar já utilizam Unicode e seu crescente suporte já é amplamente
difundido em muitas aplicações.  Você pode aprender mais sobre Unicode na
Wikipedia, ou neste excelente artigo do Joel on Software.

## E o "cake" isso tudo tem a ver com o CakePHP?

Para evitar a ocorrência de problemas de codificação de caracteres com sua
aplicação em CakePHP, a recomendação definitiva é: utilize UTF-8 em todo
local que você vir relacionado a codificação de caracteres e você não vai ter
problema.  (É claro que se você tem certeza de que nunca vai ter conteúdo
escrito em outro idioma que não o português do Brasil, ao invés de UTF-8, pode
utilizar tudo na codificação ISO-8859-1).

Abaixo segue um checklist sobre todos os locais que você deve conferir para
esta codificação, em ordem do mais simples e que provavelmente já deve estar
okay, até o mais inusitado e que você só vai querer mexer em caso de
paranoia total.  Se você estiver enfrentando problemas com codificação de
caracteres, vá fazendo uma de cada vez e verifique até que ponto o problema
ainda persiste:

### 1. App.encoding no arquivo core.php

Por padrão, o CakePHP já vem configurado para trabalhar com UTF-8.  Esta
configuração é feita no arquivo core.php (em /app/config).  Confira se
este arquivo contém uma linha como a abaixo:

```php
Configure::write(‘App.encoding’, ‘UTF-8’);
```

Vale lembrar também que este é o valor que é retornado quando se utiliza
$html->charset() numa view ou num layout.


### 2. Codificação de seus arquivos .php

Os arquivos .php que você escrever (particularmente seus models, suas views e
seus controllers) devem ser gravados na codificação UTF-8.  Esta é uma questão
comum já que em alguns editores a configuração da codificação de caracteres
acaba ficando escondida.  Portanto, dê preferência para usar um editor de
código "esperto" que permita especificar adequadamente a codificação de
caracteres ao salvar seus arquivos.  Um detalhe um pouco mais técnico é que a
codificação de seus arquivo deve ser "UTF-8 sem BOM".  BOM significa byte order
mark.  É que UTF-8 por padrão utiliza uma marcação escondida antes de cada
caracter apenas para que os byte da representação interna possam ficar
ordenados.  O problema é que essa marcação interna não é compreendida pelo PHP
que acaba inserindo uns caracteres estranhos como lixo no começo da página.
Se você vir algo como

```
ï»¿
```

então abra seus arquivos .php em um editor de texto esperto e salve-os
novamente na codificação “UTF-8 sem BOM” (procure por algo correlato
no seu editor de texto, geralmente em Preferências, algum item de menu,
barra de status ou até no próprio diálogo Salvar). Citando Túlio Faria,
"nunca use BOM, porque BOM é ruim…" 😛

### 3. Metatag de codificação de caracteres no layout

A especificação da linguagem (X)HTML define informações de metadados
referentes à própria página, as chamadas metatags.  Certifique-se de que
seus arquivos de layout (em /app/views/layouts) possuam esta tag meta em
seu cabeçalho:

```html
<meta http-equiv=”content-type” content=”text/html; charset=UTF-8″ />
```

### 4. Codificação do servidor web Apache

A metatag acima é a forma de indicar, no padrão (X)HTML, em que codificação de
caracteres está o conteúdo da página.  Mas necessariamente isto não significa
(infelizmente) que o navegador irá utilizar esta codificação.  No Mozilla
Firefox, a definição da codificação de caracteres pode ser trocada no menu
Exibir > Codificação (a propósito, ficar experimentando com as opções deste
menu é uma boa maneira de se descobrir em que codificação de caracteres o
conteúdo está).  Assim, para que as páginas de sua aplicação já venham por
padrão com a codificação correta definida, você deve configurar o servidor web
também para serví-las em UTF-8.  No caso do Apache (sem esquecer de reiniciá-lo
depois), adicione a seguinte ao arquivo httpd.conf (ou onde vir ser mais
adequado):

```
AddDefaultCharset UTF-8
```

Se você não tiver acesso às configurações do servidor Apache (uma aplicação já
em produção, p.ex.), pode adicionar a linha acima ao final de seus arquivos
.htaccess na raiz do CakePHP.


### 5. Codificação no banco de dados

Problemas de codificação no banco de dados são um pouco mais sérios porque
podem até fazer com que dados contendo caracteres especiais não apareçam em
algumas listagens.  No caso do MySQL, a codificação pode ser definida na
criação da tabela com a opção CHARACTER SET.  Para definir todos os campos
textuais da tabela em UTF-8, crie-a como no exemplo:

```sql
CREATE TABLE posts (
id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
title VARCHAR(50),
body TEXT,
created DATETIME DEFAULT NULL,
modified DATETIME DEFAULT NULL
) CHARACTER SET utf8 COLLATE utf8_bin;
```

PS: A parte "COLLATE utf8\_bin" é opcional.  Mas se por acaso você quiser
deixar seu banco insensível à diferença entre maiúsculas e minúsculas, você
poderia deixar "COLLATE utf8\_unicode\_ci" (o sufixo "\_ci" significa case
insensitive).

### 6. Alterando a codificação padrão do interpretador PHP via php.ini

Se você seguiu os cinco itens anteriores corretamente, então com 99,9% de
certeza seu problema com codificação de caracteres já terá sido resolvido.
Todavia, se você está certo de que fez tudo corretamente, e está arrancando os
cabelos porque ainda vê quadrados, pontos de interrogação ou caracteres
estranhos onde deveriam estar acentos.  Se já perguntou na lista e não teve a
solução do problema.  Se você já estiver ficando louco e não sabe mais o que
fazer, tente modificar a codificação padrão do interpretador PHP.  No arquivo
php.ini de sua instalação do PHP, deve haver um trecho semelhante a este:

```
; PHP’s built-in default is text/html
default_mimetype = “text/html”
;default_charset = “iso-8859-1”
```

Você pode retirar o ponto-e-vírgula da frente da diretiva "default\_charset" e
modificar seu valor para "utf-8", reiniciando seu servidor web em seguida.  Mas
novamente, só faça isto em último caso!

É isso!  Embora longo, espero que este artigo lhe seja útil!

[Publicado originalmente em 2009-03-05](https://mfandrade.wordpress.com/2009/03/05/guia-definitivo-da-codificacao-de-caracteres-no-cakephp/).
