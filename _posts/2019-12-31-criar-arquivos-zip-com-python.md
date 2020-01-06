---
layout: post
title: Criando arquivos zip com Python
categories: Python
---

Estou fazendo algumas primeiras incursões em [Python](http://www.python.org).
Realmente é uma linguagem bastante suscinta e também muito elegante.  Descobrir
o jeito "pythônico" de fazer as coisas, especialmente para nós que viemos de
outras linguagens como Java, chega a ser revelador (Não é raro se ter alguns
momentos do tipo: "Puxa, só isso!?").

Como novato querendo aprender a mexer em Python, vou relatar aqui meus
progressos.  Basicamente o objetivo é anotar para eu mesmo me lembrar, mas se
puder ajudar a mais alguém, sem dúvida será bom.

Enfim, chega de papo.  Estou montando uma solução para criação e transferência
de arquivos compactados entre dois hosts.  Eu poderia implementá-la em Java,
Shell ou mesmo PHP sem problemas, mas optei por Python justamente para me
forçar a aprender um pouco mais.

A primeira coisa que precisei foi apenas criar arquivos zip.  Em Python é
bastante simples:

```python
import zipfile.ZipFile
meuzip = ZipFile('arquivo.zip', 'a')
meuzip.write('conteudo.txt')
meuzip.close()
```

Neste código, `ZipFile` é a classe que manipula arquivos compactados,
`arquivo.zip` é o nome do arquivo no qual os demais serão adicionados (neste
caso, apenas o arquivo conteudo.txt que está no mesmo diretório).  O segundo
parâmetro do construtor da classe (‘a’ppend) é um flag bem no estilo dos flags
de [manipulação de arquivos em C](http://www.vivaolinux.com.br/artigo/Manipulando-arquivos-em-C-(parte-1)/).

Ao fechar o arquivo depois de gravar, se você vir o diretório corrente, lá já
estará o arquivo.zip recém-criado.

Simples, não!?  E o que é melhor: nada dos chatos [streams de Java](http://java.sun.com/developer/technicalArticles/Streams/ProgIOStreams/). 😛

Para mais informações, não deixe de ler a documentação do [módulo zipfile do
Python](http://docs.python.org/library/zipfile.html).

[Publicado originalmente em 2012-01-17](https://mfandrade.wordpress.com/2012/01/17/criar-arquivos-zip-em-python/).
