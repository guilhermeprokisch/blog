---
published: true
title: Usando Containers para Análise de Dados
image: touring.jpg
---

_Adaptação do post: [How to get started with data science in containers](http://blog.kaggle.com/2016/02/05/how-to-get-started-with-data-science-in-containers/)_

<p class="intro"><span class="dropcap">S</span>e você trabalha com análise de dados e acompanha sites de tecnologia já deve ter ouvido ao menos falar no Docker. Ou com certeza já usou algum site que roda R ou Python, ou outra linguagem, direto do navegador (Kaggle, Data Academy, Data Quest, Hacker Rank, Coursera, entre outros) todos eles usam Docker por trás. Se você não sabe direito o que é Docker, nunca ouviu falar ou nem nem pra que serve, aqui vai uma introdução.</p>

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

Vamos criar uma outra máquina com mais recursos chamada docker2:
_Se você usa Linux, pode ignorar esse passo._

{% highlight sh %}
$ docker-machine create -d virtualbox --virtualbox-disk-size "50000" --virtualbox-cpu-count "4" --virtualbox-memory "8092" docker2
{% endhighlight %}

Obviamente você pode os ajustar os parametros _disk-size_, _cpu-count_ e _memory_ para seu sistema.

Inicie a VM.

{% highlight sh %}
$ docker-machine start docker2
$ eval $(docker-machine env docker2)
{% endhighlight %}

Se mais tarde, quando você abrir outra janela do terminal e o Docker avisar exibir uma mensagem como "Cannot connect to the Docker daemon. Is the docker daemon running on this host?". Você precisará inicar a VM novamente rodando as duas linhas acima.

## A Mágica

Para um pouco de divertimento rode agora:
{% highlight sh %}
$ docker run -it kaggle/python python
{% endhighlight %}

Como você não tem a imagem "kaggle/python" no seu computador a primeira vez que rodar o esse comando o Docker irá baixar os arquivos necessários.

Depois de terminar você provalmente estará dentro de um terminal do Python com todas as bibliotecas mais importantes .

{% highlight python %}
import tensorflow
import pandas as pd
import keras
import sklearn as sk
{% endhighlight %}

## O Mínimo de Docker.

Uma vez instalado o Docker. Você precisa entender alguns conceitos e comandos básicos para usa-lo. Existem muitos tutoriais na internet sobre Docker que explicam em detalhes. Você pode começar dando uma olhada no canal do Rafael Gomes que vez um ótimo [video](https://www.youtube.com/watch?v=0-AK020S1ak) introdutório.

Mas básicamente lembre que para rodar um container você precisa de uma imagem. Uma imagem você pode ou cria-la do zero a partir de uma arquivo de texto chamado Docker File ou pela-la pronta do DockerHub. (No caso desse tutorial vamos usar a imagem oficial do Kaggle para o Python)

Vamos pegar a imagem oficial do Kaggle chamada "kaggle/python" do DockerHub para isso rode.

{% highlight sh %}
$ docker pull kaggle/python
{% endhighlight %}

Esse comando baixa e instala (build) a imagem no seu Docker.

Por exemplo, se você quiser baixar também a imagem do R, "kaggle/rstats" rode.

{% highlight sh %}
$ docker pull kaggle/rstats
{% endhighlight %}

Para listar as imagens instaladas no seu Docker rode:

{% highlight sh %}
$ docker images
{% endhighlight %}

## Rodando o Container.

Depois de ter a imagem você pode rodar quantos containers quiser com base nela. 

Talvez essa parte essa a mais importante e complexa do Docker. O comando "docker run NOME_DA_IMAGEM" inicia de fato um container. O fato é que existem muitos parametros que podem ser agregados ao "docker run" e eles dependem de entender alguns conceitos do Docker.

Alguns para lembrar são.

-v : (Volume). Conecta algum caminho da sua máquina local com uma pasta dentro do container. E então tudo que é escrito na pasta dentro do container será copiado para a pasta local e não será apagada depois do container ser removido.
-it : Roda e entra dentro do container de forma interativa.
-p :  Conecta alguma porta local com uma porta dentro do container.
-rm : Remove o container depois de sair.

O comando -v existe por que um container _não armazena dados_. Ele é uma _"máquina virtual"_ que toda vez que iniciado volta ao estado inicial como descrito na imagem.

Finalmente rode:

{% highlight sh %}
$ docker run -it kaggle/python
{% endhighlight %}



----------------------------------
É verdade que existe uma curva de aprendizado do Docker um pouco lenta. Pode demorar uns dias até você notar a importância. Principalmente se você for mais para a área de análise de dados mais do que desenvolvendor ou programador. 

Segue um aula de introdução ao básico do Docker:

<iframe width="560" height="315" src="https://www.youtube.com/embed/0-AK020S1ak" frameborder="0" allowfullscreen></iframe>



