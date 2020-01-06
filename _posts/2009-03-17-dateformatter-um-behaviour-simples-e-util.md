---
layout: post
postid: zipfiles-with-python
title: DateFormatter, um behaviour simples e útil
categories: old cakephp
---

Desenvolvimento de sistemas envolve boa parte de planejamento e também de
engenharia.  Uma das piores coisas que se pode ter, especialmente ao se dar
manutenção em aplicações feitas inadequadamente é precisar alterar algo no
sistema que não foi desenvolvido com o princípio DRY em mente, geralmente
porque o "desenvolvedor" baseava-se na programação orientada a
copiar-e-colar.

Um exemplo ilustrativo do que quero dizer, p.ex., é a formatação de datas e
horas.  Geralmente o formato de data e hora que deve ser apresentado ao
usuário depende da localização do site, do público alvo, além de outros
fatores.  Por exemplo, para um site de notícias em português talvez seja bom
mostrar-se datas por extenso, (como “17 de março de 2009”) ou no formato
dd/mm/aaaa.  Supondo que você já tenha iniciado a construção do sistema neste
formato de data em todas os 50 scripts de páginas de sua aplicação até que um
dia decidiu-se que as datas deveriam ficar num formato mm/dd/aaaa (por exemplo,
03/17/2009).  E agora?

Além disso, geralmente os dados são armazenados de forma diferente da maneira
como são exibidos.  Os bancos de dados usam por padrão o formato aaaa-mm-dd
para datas.  Nas mãos de um programador displicente, conversão de formatos de
datas pode facilmente se tornar um problema chato.

Por hora, o CakePHP é minha ferramenta de opção.  Além de ter uma comunidade de
desenvolvedores e usuários muito numerosa e atuante, uma das coisas que me
fascinam no CakePHP é a clareza e a facilidade propiciadas para uso de boas
práticas de programação.  Um dos melhores exemplos disso é o behaviour
DateFormatter.

DateFormatter é um dos behaviours mais simples e ao mesmo tempo mais úteis que
já encontrei para CakePHP.  Ele nos permite especificar a o formato de seus
dados temporais tal como você deseje exibí-los aos usuários e o formato
adequado para armazenamento em banco de dados de uma maneira centralizada,
organizada e gerenciável.  Tudo o que você precisa é definir os formatos
desejados para exibição de suas datas (o que pode depender da localização de
sua aplicação) e o formato no banco de dados que o behaviour pode lidar com as
formatações transparentemente.

# Formatando Datas

Para utilizar o DateFormatter behaviour em sua aplicação,
copie o código do site, salvando-o num arquivo em
/app/models/behaviours/date_formatter.php.

Como o nome indica, um behaviour em CakePHP é um comportamento que
agrupa uma funcionalidade comum a vários models.  Para usá-lo em toda
a sua aplicação, aplique-o a seu app_model.php:

```php
<?php
class AppModel extends Model {
// …
var $actsAs = array(‘DateFormatter’, …);
// …
}

E pronto!

Como definido no código do behaviour, or padrão, suas datas para o usuário
estarão no formato dd/mm/aaaa e para o banco em aaaa-mm-dd (como mesmos
parâmetros do comando date do PHP).  O código está muito bem comentado e você
pode modificar seus formatos de data nos atributos do behaviour sem qualquer
dificuldade, mas você também pode definir estes formatos nas configurações do
CakePHP.

Se você analisar o código da implementação do behaviour, verá que é de uma
simplicidade ímpar!

Essencialmente, antes de salvar um registro de um model no banco (callback
beforeSave do AppModel), ele converterá os campos de data para o formato de
banco definido e aplicará o formato de data de exibição depois de obtê-lo no
banco (callback afterFind).

O que é melhor é que o DateFormatter, com os formatos consistentes, pode ser
utilizado transparentemente sem qualquer problema de compatibilidade com
máscaras em javascript (como, p.ex., plugins MaskedInput e MeioMask do jQuery)
ou elementos de formulário para seleção de data (como o Advanced DatePicker
Helper ou AnnoDommini para CakePHP).

O CakePHP também tem o TimeHelper para ajudar na formatação de datas e horas,
mas como é aplicado na camada de visão, não há definição centralizada de
formato.

DateFormatter passou a ser imprescindível em minhas aplicações CakePHP.
Espero que lhe ajude também!

[Publicado originalmente em 2009-03-17](https://mfandrade.wordpress.com/2009/03/17/dateformatter-um-behaviour-simples-e-util/).
