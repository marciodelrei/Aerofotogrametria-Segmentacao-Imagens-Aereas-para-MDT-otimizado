# Aerofotogrametria - Segmentação de Imagens Aéreas para MDT otimizado
<p align="center">
  <img alt="intro01" src="http://static.wixstatic.com/media/32ebcf_1113d51260504980b1c65fedfba2383d~mv2.jpg/v1/fill/w_737,h_605,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/32ebcf_1113d51260504980b1c65fedfba2383d~mv2.jpg" width="280" hspace="10" />
  &nbsp;
  <img alt="intro02" src="https://www.ptatopografia.com.br/imagens/informacoes/empresas-aerofotogrametria-07.jpg" width="300" hspace="10" />
</p>

## Índice
1. [Resumo](#resumo)
2. [Abstract](#abstract)
3. [Introdução](#introducao)
4. [Conceitos](#conceitos)
5. [Motivação](#motivacao)
6. [Metodologia](#metodologia)
7. [Resultados](#resultados)
8. [Discussão](#discussao)
9. [Conclusão](#conclusao)
10. [Referências](#referencias)

## Resumo
Ferramentas e aplicativos de Aerofotogrametria vêm ganhando popularidade e sendo usados como uma ótima alternativa para criação de modelos tridimensionais representando fielmente e virtualmente relevos reais. Dentre eles, Agisoft Metashape (antigo Agisoft Photoscan), Drone Deploy, Open Drone Map (ODM - foco deste experimento). Estes softwares fazem uso de algoritmos de técnicas de fotogrametria e triangulação de pontos conhecidos em amostras tomadas de pontos consecutivos, diferentes e com pequenos intervalos de tempo, feito normalmente por VANTS (ex. drones multirrotores). O input destas operações, normalmente são muitas fotos da Área de Interesse. Tais operações resultam em produtos mensuráveis como por exemplo Ortofotos e Modelo Digital do Terrenos em 3D, todos georrefenciados.

Para melhorar a precisão do Modelo Digital do Terreno e torná-lo mais fiel em sua representação digital, além de pós processamentos baseados em medições de GPS e outros, há possibilidade de se utilizar máscaras, criadas manulamente com editores gráficos, que identificam elementos fora do espectro de Terreno Natural, como casas, construções, carros, cursos e espelhos d'água, grandes massas verdes como aglomerados de árvores e florestas. Este experimento focou na construção automática destas máscaras com técnicas de Inteligência Artificial, mais precisamente o Aprendizado Profundo (Deep Learning em inglês). A técnica utilizada foi a Segmentação de Imagens através da Aquitetura de Rede U-NET, onde foi criado um modelo usando dataset com 400 imagens diferentes e suas respectivas anotações, classificando cada pixel das imagens. 

Após criação do modelo de IA (obteve 86% de acurácia com o dataset de validação, indicando que mais imagens anotadas são necessárias para melhoria do modelo de IA), foi usado um dataset desconhecido do modelo para inferência. Para mensurar o ganho com da aplicação das máscaras geradas por IA, foram feitos dois processamentos do dataset não conhecido pelo modelo com o software ODM usando os mesmos parâmetros. A diferença entre os processamentos foi a inclusão das mácaras geradas pela IA no segundo processamento para comparação e avaliação do experimento.

Como resultado, foram identificadas pequenas áreas no Modelo Digital de Terreno, que mostraram alterações quando as máscaas da IA foram injetadas no processamento, indicando que apesar de ter sido um resulatdo sutil, podem influenciar na melhoria de qualidade final do processamento.

Concluiu-se que é bem promissor o uso da IA para identificação de elementos anómalos em imagens de aerolevantamento para melhoria de qualidade de geração de Modelos Digitais de Terreno. Vale registrar que o dataset anotado que foi usado para a criação do modelo da IA, era de outro país, possuindo características naturais diferentes do nosso país (Brasil).

## Abstract
Tools and applications for aerial photogrammetry have been gaining popularity and are being used as a great alternative for creating three-dimensional models that faithfully and virtually represent real terrain. Among these tools are Agisoft Metashape (formerly Agisoft Photoscan), Drone Deploy, and Open Drone Map (ODM - the focus of this experiment). These software applications make use of algorithms based on photogrammetry techniques and triangulation of known points in samples taken from consecutive, different, and closely spaced points in time, typically using unmanned aerial vehicles (e.g., multirotor drones). The input for these operations typically consists of many photos of the area of interest. These operations result in measurable products such as orthophotos and georeferenced 3D digital terrain models.

To improve the accuracy of the digital terrain model and make it more faithful in its digital representation, in addition to post-processing based on GPS measurements and others, it is possible to use masks manually created with graphic editors that identify elements outside the natural terrain spectrum, such as buildings, houses, cars, bodies of water, and large green masses such as clusters of trees and forests. This experiment focused on the automatic construction of these masks using artificial intelligence techniques, more specifically Deep Learning. The technique used was Image Segmentation using the U-Net Network Architecture, where a model was created using a dataset with 400 different images and their respective annotations, classifying each pixel of the images.

After creating the model, an unknown dataset was used for inference. To measure the gain from the application of the masks generated by AI, two processings of the unknown dataset were performed using the ODM software with the same parameters. The difference between the processings was the inclusion of the masks generated by AI in the second processing for comparison and evaluation of the experiment.

As a result, small areas were identified in the Digital Terrain Model that showed changes when the AI masks were included in the processing, indicating that although the result was subtle, it can influence the overall improvement in the final quality of the processing.

It was concluded that the use of AI for the identification of anomalous elements in aerial survey images is very promising for improving the quality of Digital Terrain Model generation. It is worth noting that the annotated dataset used for the creation of the AI model was from another country, with different natural characteristics compared to our country (Brazil).

## Introdução

A técnica de Aerofotogrametria acelera o processo de estudos de relevo através do aerolevantamento feito por sensores remotos, neste caso Drones ou Vants que capturam imagens geralmente de forma autônoma, seguindo um plano de vôo previamente estudado. Após coleta das fotos, softwares aplicam algoritmos para gerarem produtos como Ortofotos e Modelos Digitais de Superfície (MDS) que representam o relevo ou o Área de Interesse (AOI ou POI Point of Interest em inglês). O MDS, de uma forma lúdica, é como se um lençol fosse sobreposto ao um terreno, onde inclui a representação de todos elementos capturados como construções, casas, carros, etc - que por vezes podem ser tratados neste experimento como **anomalias**, além do terreno natural.

Com algoritmos embarcados nestes softwares, gera-se o Modelo Digital de Terreno (MDT), tais algoritmos buscam eliminar as anomalias citadas acima, através de técnicas de triangulação e medição dos pontos tridimensionais gerados. Estes algoritmos entregam um produto muito bom quando bem parametrizados antes de seu processamento.

## Motivação
O software Open Drone Map ([ODM](https://community.opendronemap.org)) e outros mais, possuem capacidade de adicionar mascáras em preto/branco ([ODM Masks](https://docs.opendronemap.org/masks/)) para fotos antes de seu processamento identificando as anomalias em cada foto tomada e desta forma o processamento irá desconsiderar tais máscaras no momento da geração do MDT.

As máscaras que identificam anomalias, devem ser criadas com editores gráficos (Ex.:[GIMP](https://www.gimp.org/)), de forma manual, artesanal e individualmente em cada foto, pintando de preto os pixels que representam as anomalias que devem ser excluídas do MDT.

<p align="center">
  <img alt="mask01" src="https://user-images.githubusercontent.com/1951843/93247037-ade87a00-f75b-11ea-8b42-25bc1d89279d.png" width="200" hspace="10" />
  <img alt="mask01" src="https://user-images.githubusercontent.com/1951843/93247007-a2954e80-f75b-11ea-87b3-4f04bd1737b9.png" width="200" hspace="10" />
</p>
<p align="center">
  <img alt="mask01" src="https://user-images.githubusercontent.com/1951843/93246970-8f827e80-f75b-11ea-8179-5a8fdd9f5193.png" width="400" hspace="10" />
</p>
<p align="center">
    <a src="https://docs.opendronemap.org/masks/"><i>fonte: ODM - Using Image Masks</i></a>
</p>
Acima um exemplo da imagem original, com sua respectiva máscara e seu MDT processado.

Em um pequeno dataset com até 20 imagens, apesar de ser extenuante, é plausível. Quando estamos diante de um dataset acima de 20 imagens, o que é normal em aplicações deste tipo, pois as imagens se recobrem em pelo menos 70%, tornando-se uma tarefa longa para que se possa começar o processamento e nos dias atuais com cronogramas mais apertados, pode-se impactar diretamente cronogramas.

Este experimento teve como base técnica, uma variação de Deep Learning conhecida como Segmentação de Imagens através da Arquitetura de Rede U-NET.

Para um melhor entendimento do experimento, trago alguns conceitos de tecnologia e processos importantes que fazem parte deste experimento.

## Conceitos
Esta seção traz alguns conceitos e informações técnicas para melhor entendimento do experimento.

### Aerofotogrametria
A aerofotogrametria é uma técnica de obtenção de informações do terreno ou superfície terrestre a partir de fotografias aéreas. Ela é utilizada para criar modelos tridimensionais, como exmplo: modelos digitais de superfície, mapas topográficos e ortofotos de áreas geográficas extensas, representando cidades, florestas, rios, entre outros como. A técnica envolve o uso de câmeras montadas em aeronaves, drones ou balões para capturar imagens aéreas do terreno. Em seguida, são aplicados algoritmos e técnicas de processamento de imagem para extrair informações geográficas e topográficas precisas e úteis para uma variedade de aplicações em engenharia, planejamento urbano, agricultura, meio ambiente, entre outras áreas.

Através de técnicas de aerofotogrametria, é possível obter:
+ Ortofotos;
+ Modelo Digital de Superfície e de Terreno;
+ Curvas de Nível;
+ Cálculos Volumétricos
+ etc...

### Segmentação de imagens
Segmentação de imagens é uma tarefa de processamento de imagens que envolve a classificação de cada pixel de uma imagem criando regiões ou segmentos significativos, onde cada segmento representa uma região semântica da imagem. O objetivo é separar as regiões de interesse da imagem de acordo com sua semântica, tornando mais fácil a análise e o processamento dessas regiões isoladamente. A segmentação de imagens é frequentemente usada em aplicações de visão computacional, como reconhecimento de objetos, análise de imagens médicas, carros autônomos e robótica. O seu uso vem se mostrando mais eficaz que a análise humana para identificação de anomalias ou determinados elementos em imagens, incluindo vídeos e capturas em tempo real.

<p align="center">
  <img alt="segim01" width=400 src= "https://miro.medium.com/v2/resize:fit:720/format:webp/1*kUrAUjDQ0zr1cegMvUrUqg.jpeg"/>
  <img alt="segim02" width=400 src="https://miro.medium.com/v2/resize:fit:786/format:webp/1*oypCvGATxpr4MJh-BM2TWw.png"/>
</p>

### Arquitetura U-Net
![](https://miro.medium.com/v2/1*n1PFaorpCSvxiIaA2Ub1bA.png)

A U-Net é uma arquitetura de rede neural convolucional desenvolvida para segmentação de imagem, que foi proposta em 2015 por Olaf Ronneberger, Philipp Fischer e Thomas Brox. O nome "U-Net" vem da forma da arquitetura, que se parece com a letra "U".

A ideia por trás do U-Net é usar uma arquitetura de rede neural convolucional com camadas de "encoding" e "decoding" para segmentar imagens. A rede tem uma forma simétrica, com camadas de convolução para comprimir as informações da imagem e camadas de convolução transposta para expandir as informações e gerar uma máscara de segmentação para a imagem.

O U-Net foi projetado para lidar com imagens médicas, onde a segmentação é necessária para a identificação precisa de estruturas como tumores e órgãos. No entanto, a arquitetura tem sido utilizada com sucesso em outras aplicações de segmentação de imagem, como segmentação de objetos em imagens de satélite.

Devido à sua eficácia em tarefas de segmentação de imagem, o U-Net tornou-se uma das arquiteturas mais populares e amplamente utilizadas em visão computacional e aprendizado profundo.

A arquitetura U-Net pode ser considerada uma variação/extensão de um autoencoder, já que a sua estrutura é composta por um "encoder" que reduz a dimensão do espaço de características (representações) e um "decoder" que reconstrói a imagem original a partir dessas representações.

No entanto, a U-Net possui uma diferença significativa em relação a um autoencoder convencional: a presença de caminhos de atalho (skip connections) que conectam camadas do encoder diretamente ao decoder. Esses caminhos de atalho permitem a preservação de informações de alta resolução do input original, o que ajuda a melhorar o desempenho da rede na tarefa de segmentação de imagens.

Portanto, a U-Net pode ser considerada uma extensão do autoencoder convencional, que é projetado especificamente para tarefas de segmentação de imagens, onde a preservação da resolução espacial é fundamental.

### Aerial Semantic Segmentation Drone Dataset (400 imagens e 24 classes)
<p align="center">
  <img alt="segim01" width=400 src= "https://www.tugraz.at/fileadmin/_migrated/pics/fyler3.png"/>
</p>
O Semantic Drone dataset está disponível [aqui](https://www.tugraz.at/index.php?id=22387) ou [aqui](https://www.kaggle.com/datasets/bulentsiyah/semantic-drone-dataset).

O Semantic Drone Dataset concentra-se na compreensão semântica de cenas urbanas para aumentar a segurança dos procedimentos autônomos de voo e pouso de drones. As imagens retratam mais de 20 casas de nadir (vista aérea) adquiridas a uma altitude de 5 a 30 metros acima do solo. Uma câmera de alta resolução foi usada para adquirir imagens em um tamanho de 6000x4000px (24Mpx). O conjunto de treinamento contém 400 imagens disponíveis publicamente.

Classes:
<table>
  <tr>
    <td>tree</td>
    <td>grass</td>
    <td>other vegetation</td>
    <td>dirt</td>
    <td>gravel</td>
  </tr>
  <tr>
    <td>rocks</td>
    <td>water</td>
    <td>paved area</td>
    <td>pool</td>
    <td>person</td>
  </tr>
    <tr>
    <td>dog</td>
    <td>car</td>
    <td>bicycle</td>
    <td>roof</td>
    <td>wall</td>
  </tr>
  <tr>
    <td>fence</td>
    <td>fence-pole</td>
    <td>window</td>
    <td>door</td>
    <td>obstacle</td>
  </tr>
</table>
Fonte: [Graz University of Technology - http://dronedataset.icg.tugraz.at](http://dronedataset.icg.tugraz.at)

Licença:
O Drone Dataset é disponibilizado gratuitamente para entidades acadêmicas e não acadêmicas para fins não comerciais, como pesquisa acadêmica, ensino, publicações científicas ou experimentação pessoal. A permissão é concedida para usar os dados desde que você concorde:

- Que o conjunto de dados vem "AS-IS", sem garantia expressa ou implícita. Embora todos os esforços tenham sido feitos para garantir a precisão, nós (Graz University of Technology) não aceitamos qualquer responsabilidade por erros ou omissões.
- Que você inclua uma referência ao "Semantic Drone Dataset" em qualquer trabalho que faça uso do conjunto de dados. Para trabalhos de pesquisa ou outro link de mídia para a página da web Semantic Drone Dataset.
- Que você não distribua este conjunto de dados ou versões modificadas. É permitido distribuir trabalhos derivados desde que sejam representações abstratas deste conjunto de dados (como modelos treinados nele ou anotações adicionais que não incluam diretamente nenhum de nossos dados) e não permitam recuperar o conjunto de dados ou algo semelhante em personagem.
- Que você não pode usar o conjunto de dados ou qualquer trabalho derivado para fins comerciais como, por exemplo, licenciar ou vender os dados ou usar os dados com o objetivo de obter um ganho comercial.
- Que todos os direitos não expressamente concedidos a você são reservados por nós (Graz University of Technology).


## Metodologia
A metodologia para geração das máscaras, serão baseados em algoritmos de aprendizado de máquina, mais precisamente redes neurais convolucionais. A função e treinar um modelo capaz de detectar automaticamente os elementos de *des*interesse na imagem. Esse modelo foi treinado com imagens anotadas manualmente que indiquem a localização desses objetos na imagem. Dessa forma, o modelo pode aprender padrões específicos associados a cada tipo de elemento e ser capaz de detectá-los com maior precisão.

Para abordagem, foi criado um modelo baseados na arquitetura U-NET para inferências.Foi usado um dataset possuindo 400 imagens e suas respectivas imagens com 24 classes anotadas.
O script hospedado no Kaggle sob o link: [aerial-semantic-segmentation-puc-rio-bi-master-tcc.ipynb](https://www.kaggle.com/code/marciodelrei/aerial-semantic-segmentation-puc-rio-bi-master-tcc?kernelSessionId=134697143), mostra o treinamento do modelo. Uma cópia do notebook foi colocada neste repositório para fácil acesso sob o nome: aerial-semantic-segmentation-puc-rio-bi-master-tcc.ipynb. Ele está com o output, tornando-se pesado para renderização no view do Github.

Após as inferências feitas, um pós-processamento foi aplicado em cada um das máscaras geradas pela U-NET, convertendo as cores de cada pixel para preto ou branco respeitando a classificação de "não terreno" para anomalias, sendo estes pixels coloridos de preto e os pixels restantes de cada imagem, considerados "terreno", sendo pintados de branco.
O script de inferência foi feito em máquina local e uma cópia foi do notebook foi colocada neste repositório para fácil acesso sob o nome: [model_inference.ipynb](https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/blob/main/model_inference.ipynb).

Foi utilizado um dataset não conhecido do modelo de IA para efeitos comparativos. Dois processamentos foram feitos no ODM com os mesmos parâmentros. A diferença foi que no primeiro processamento, foram utilizadas somente as imagens originais e no outro foram adicionadas as máscaras para serem analisadas e observadas para que a comparação resultante fosse criada.

## Resultados
Conforme citado, produtos interessantes podem gerados e como exemplo foi exposto no Skecthfab o Modelo Digital de Superfície para apreciação: [MDS no Sketchfab](https://skfb.ly/oIsHv)
<p align="center">
  <img alt="mds" width=800 src= "https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/blob/main/MDS_processado.jpg">
</p>

Os parâmetros nos processamento dos MDT's foram os mesmos:
<p align="center">
  <img alt="mds" width=800 src= "https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/blob/main/Painel%20-%20WebODM.png">
</p>

Como resultado, comparamos o MDT gerado pelos 2 processamentos Imagens Sem máscaras e Imagens com Máscaras inferidas pela IA:
<p align="center">
  <img alt="mds" width=800 src= "https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/blob/main/MDT_compare.gif">
</p>

A imagem acima demonstra uma alteração sutil entre os MDT's, porém isto já indica que o mascaramento influencia em sua geração.

Abaixo geramos curvas de nível que representam o terreno e fica um pouco mais claro esta diferença, onde cores azuis demontram o MDT Sem máscaras e linha magenta representa o MDT com máscaras IA, comprovando a eliminação de anomalias.

<p align="center">
  <img alt="MDT sem máscaras IA- Curvas de nível de 1m" src="https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/blob/main/MDT_CN1m_SemMascara.png" width="200" hspace="10" />
  <img alt="MDT com máscaras IA - Curvas de nível de 1m" src="https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/blob/main/MDT_CN1m_ComMascara.png" width="200" hspace="10" />
</p>
<p align="center">
  <img alt="MDT comparativo" src="https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/blob/main/MDT_CN1m_Comparativo.png" width="400" hspace="10" />
</p>

## Conclusão
Com a apresentação dos resultados, mesmo que sejam sutis, a injestão de máscaras para processamento sensibilizam os algoritmos de geração de MDT. A criação de modelos de Segmentação Semântica de Imagens com auxílio de IA pode economizam tempo na geração de máscaras que detectam anomalias. Ao analisarmos as saídas do algoritmo do Kaggle do modelo, vemos que muitas imagens anotadas são necesárias para que possamos melhorar a acurácia do modelo de IA. A realização do treinamento do modelo de IA com datasets anotados com cenários regionais, provavelmete ajudarão o modelo nas inferências.

A utilização de modelos pré-treinados como o SAM (Segment Anything Model) do Facebook, por exemplo, não foram testados e podem ser utilizados para trabalho futuro em cima dos algoritmos aqui apresentados, com as devidas alterações para seu pleno funcionamento.

## Referências e Links interessantes
------------------------------------------------------------------------------------------------------------
<p>Drone-Image-Segmentation</p>
Drone Image Segmentation -U-Net architecture based on Manoj Gopalakrishna in Kaggle
[Manoj ](https://www.kaggle.com/code/manojgowda65/drone-image-segmentation-u-net-architecture)
https://www.kaggle.com/code/marciodelrei/drone-image-segmentation/edit
<p>Drone-Image-Segmentation</p>
Drone Image Segmentation -U-Net architecture based on Manoj Gopalakrishna in Kaggle
[Manoj ](https://www.kaggle.com/code/manojgowda65/drone-image-segmentation-u-net-architecture)
https://www.kaggle.com/code/marciodelrei/drone-image-segmentation/edit

- [satellite-image-deep-learning](https://www.satellite-image-deep-learning.com) de Robin Cole. Deep learning applied to satellite imagery
[Github](https://github.com/satellite-image-deep-learning)

- [Semantic Segmentation of Aerial Imagery Using U-Net in Python](https://towardsdatascience.com/semantic-segmentation-of-aerial-imagery-using-u-net-in-python-552705238514) de Andrew Joseph Davies
[Github](https://github.com/satellite-image-deep-learning)

- [MBRSC dataset](https://www.kaggle.com/humansintheloop/semantic-segmentation-of-aerial-imagery)

- Solução baseada em modelo U-NET usando como fonte de conhecimento o artigo "[Semantic Segmentation of Aerial Imagery Using U-Net in Python](https://towardsdatascience.com/semantic-segmentation-of-aerial-imagery-using-u-net-in-python-552705238514)" de Andrew Joseph Davies

- [SAM - Segment Anythin Model. Facebook] (https://github.com/facebookresearch/segment-anything)

