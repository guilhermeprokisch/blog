---
published: true
title: Usando Containers para Análise de Dados
image: touring.jpg
---

_Adaptação do post: [How to get started with data science in containers](http://blog.kaggle.com/2016/02/05/how-to-get-started-with-data-science-in-containers/)_

<p class="intro"><span class="dropcap">Se</span> trabalha com análise de dados e acompanha sites de tecnologia já deve ter ouvido ao menos falar no Docker. Ou com certeza já usou algum site que roda R ou Python, ou outra linguagem, direto do navegador (Kaggle, Data Academy, Data Quest, Hacker Rank, Coursera, entre outros) todos eles usam Docker por trás. 

Se você não sabe direito o que é Docker, nunca ouviu falar ou nem nem pra que serve, aqui vai uma introdução.

Docker Containers resolvem um monte de problemas ao mesmo tempo: Eles fazem ficar muito fácil a intalação de bibliotecas complicadas; Eles fazem sua análise reprodutível - extremamente importante para  a **ciência** de dados; Eles fazem sua análise ficar bem fácil para compartilhamento; Eles te dão uma economia enorme na preparação do ambiente de análise, epecialmente para quem usa Python em ciência de dados.

### O que são Containers?

Containers são como máquinas virtuais ultraleves. Se você já usou o Virtual Box sabe que restaurar uma VM de um snapshot pode demorar alguns minutos, mas em Docker Containers essa restauração é praticamente instatanea. Então você pode colocar dentro do Container tudo que seu projeto necessita para rodar. Você pode especificar os mínimos detalhes: Desde a versão espécifica do sistema operacional, a versão da sua linguagem de programação até versão das bibliotecas e de suas dependências. Tudo isso em um único arquivo que roda em milesegundos e está isolado do seu sistema principal. Toda vez que você restaurar o container, sua execução é idêntica dando a você reprodutibilidade. Containers também vão rodar igualmente em qualquer sistema operacional que tenha Docker: OS X, Windows e Linux, então colaboração entre colaboração se torna muito mais fácil.

Você deve estar agora pensando: Ahh então são "Máquinas Virtuais", o desempenho deve ser lento. 

Não! Você está enganado! E isso é o mais impressionante de containers. 

Containers não são VM e seu desempenho comparado a sua máquina de "verdade" é quase imperceptível. Isso por que containers rodam a nível de Kernel e acessam os os recursos de sua máquina de uma maneira diferente de máquinas virtuais. O foco nesse post será como usar o Docker para Análise de Dados. O final postarei alguns links para mais detalhes técniso. Agora mãos a massa!

### Mas por que usar containers?

Pessoalmente, eu acho que containers eliminam uma dor de cabeça que ocorre algumas vezes em usar Python para análise de dados. Python e R são duas grandes linguagens para estatística, e cada uma tem seus prós e contras. Mas uma grande diferença entre elas é como elas trabalham com bibliotecas e pacotes. O install.packages() do R funciona relativamente bem, é raro acontecer conflitos entre pacotes. Se você tenta rodar um script que usa uma biblioteca que não está instalada no seu sistema, com alguns cliques você pode instalar ela pelo CRAN. 

Mas em Python, o fluxo do processo é mais ou menos assim. Você nota que precisa da biblioteca X, dai você chama pip install X, mas X também depende de outras bibliotecas A, B e C. Mas B já existe no seu sistema via easy_install, então o pip cancela a parte a instalação de B e remove alguns arquivos acumulados usados para instalacão. Mas import B não funciona. Ou você discobre que C roda somente com uma versão mais nova do numpy, dai você instala a versão mais nova, e somente depois descobre que as outras bibliotecas do project Y e Z dependem de uma versão antiga do numpy e param de funcionar.. e assim vai.. (ou pode acontecer)

Instalações no Python podem apresentar problemas assim, ou ainda conflitos entre as versões do Python dentro de um mesmo sistema. A virtuaenv ajuda um pouco, mas na minha experiência só atrasa o Crash. Exemplo: O Python da virutalenv chama uma biblioteca, mas o jupyter dentro da virtualenv chama a versão da biblioteca do python do sistema.

É claro que todos esses problemas podem ser resolvidos. O Python tem um comunidade muito ativa e disposta. Mas toma tudo isso toma tempo e paciência. 

Mas se você usa um container tudo isso desaparece. Você só tem que investir tempo configurando o seu container uma vez. Na verdade, se você usar uma imagem como a do Kaggle, você não tem que se procupar em configurar quase nada para sua análise de dados tudo já vira pronto pra usar. Eles tem imagens extremamente incríveis que tem quase todos os pacotes necessários para análise de dados, machine learning, deep learning e etc. Você pode  conferir os pacotes que vem dentro da imagens aqui. ([Python](https://github.com/Kaggle/docker-python/blob/master/Dockerfile), [R](https://github.com/Kaggle/docker-rcran), [Julia](https://github.com/Kaggle/docker-julia))

## Como começar

O primeiro passo é instalar o Docker no seu sistema. A instalação é bem simples, basta seguir os passos indicados para seu sistema operacional no [site do Docker](https://docs.docker.com/).

Um ponte importante para entender é que, a tecnologia do Docker é nativa de uma funcionalidade do Kernel do Linux, então no OSX e no Windows o Docker roda dentro de máquina virtual que contém o Linux. 

A máquina virtual padrão no Windows e do OSX é um pouco pequena e pode ser problematica para algumas tarefas de análise de dados. 

{% highlight shell %}
$ docker-machine create -d virtualbox --virtualbox-disk-size "50000" --virtualbox-cpu-count "4" --virtualbox-memory "8092" docker2
{% endhighlight %}





----------------------------------
É verdade que existe uma curva de aprendizado do Docker um pouco lenta. Pode demorar uns dias até você notar a importância. Principalmente se você for mais para a área de análise de dados mais do que desenvolvendor ou programador. 

Segue um aula de introdução ao básico do Docker:

<iframe width="560" height="315" src="https://www.youtube.com/embed/0-AK020S1ak" frameborder="0" allowfullscreen></iframe>



