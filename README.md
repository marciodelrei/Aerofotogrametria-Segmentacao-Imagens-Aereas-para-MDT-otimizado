<p align="center">
  <img alt="TCC Bi Master" src="https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/blob/main/header.jpg" />
</p>

Trabalho de final de curso PUC-Rio BI MASTER

Autor: Márcio d'El-Rei (marcio.delrei@gmail.com)

Orientadora: Prof DSc Manoela Kohler
__________________________________________________________________________________________________

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
Ferramentas e aplicativos de aerofotogrametria vêm ganhando popularidade e sendo usados como uma ótima alternativa para a criação de modelos tridimensionais que representam fielmente e virtualmente relevos reais. Entre eles, destacam-se o Agisoft Metashape (anteriormente conhecido como Agisoft Photoscan), o Drone Deploy e o Open Drone Map (ODM - foco deste experimento). Esses softwares utilizam algoritmos de técnicas de fotogrametria e triangulação de pontos conhecidos em amostras obtidas a partir de pontos consecutivos, diferentes e com pequenos intervalos de tempo, geralmente capturadas por VANTs (por exemplo, drones multirrotores). O input dessas operações geralmente consiste em várias fotos da área de interesse. Essas operações resultam em produtos mensuráveis, como ortofotos e modelos digitais de terreno em 3D, todos georreferenciados.

Para melhorar a precisão do modelo digital de terreno e torná-lo mais fiel em sua representação digital, além de realizar pós-processamentos baseados em medições de GPS e outros, é possível utilizar máscaras criadas manualmente com editores gráficos, que identificam elementos fora do espectro de terreno natural, como casas, construções, carros, corpos d'água, áreas verdes extensas como aglomerados de árvores e florestas. Este experimento se concentrou na construção automática dessas máscaras com técnicas de Inteligência Artificial, mais especificamente o Aprendizado Profundo (Deep Learning em inglês). A técnica utilizada foi a Segmentação de Imagens por meio da Arquitetura de Rede U-NET, em que um modelo foi criado usando um conjunto de dados (dataset) com 400 imagens diferentes e suas respectivas anotações, classificando cada pixel das imagens.

Após a criação do modelo de IA (que obteve 86% de acurácia com o dataset de validação, indicando que mais imagens anotadas são necessárias para melhorar o modelo de IA), um dataset desconhecido foi usado para inferência. Para medir o ganho com a aplicação das máscaras geradas pela IA, foram realizados dois processamentos do dataset não conhecido pelo modelo com o software ODM, usando os mesmos parâmetros. A diferença entre os processamentos foi a inclusão das máscaras geradas pela IA no segundo processamento, para comparação e avaliação do experimento.

Como resultado, foram identificadas pequenas áreas no modelo digital de terreno que mostraram alterações quando as máscaras da IA foram incluídas no processamento, indicando que, mesmo sendo um resultado sutil, isso pode influenciar na melhoria da qualidade final do processamento.

Concluiu-se que o uso da IA para identificação de elementos anômalos em imagens de aerolevantamento é bastante promissor para melhorar a qualidade da geração de modelos digitais de terreno. Vale ressaltar que o conjunto de dados anotado usado para criar o modelo de IA era de outro país, possuindo características naturais diferentes das do nosso país (Brasil).

## Abstract
Tools and applications of aerial photogrammetry have been gaining popularity and being used as an excellent alternative for creating three-dimensional models that faithfully and virtually represent real terrains. Among them, Agisoft Metashape (formerly known as Agisoft Photoscan), Drone Deploy, and Open Drone Map (ODM - the focus of this experiment) stand out. These software utilize algorithms from photogrammetry techniques and known point triangulation in samples taken from consecutive, different, and closely timed points, usually carried out by multirotor drones. The input for these operations typically consists of multiple photos of the area of interest. These operations result in measurable products such as orthophotos and georeferenced 3D Digital Terrain Models.

To improve the accuracy of the Digital Terrain Model and make it more faithful in its digital representation, in addition to post-processing based on GPS measurements and others, it is possible to use manually created masks with graphic editors that identify elements outside the natural terrain spectrum, such as buildings, houses, cars, bodies of water, and large green masses such as clusters of trees and forests. This experiment focused on the automatic construction of these masks using Artificial Intelligence techniques, specifically Deep Learning. The technique employed was Image Segmentation using the U-NET Network Architecture, where a model was created using a dataset of 400 different images and their respective annotations, classifying each pixel of the images.

After the creation of the AI model (which achieved 86% accuracy with the validation dataset, indicating that more annotated images are needed to improve the AI model), an unknown dataset was used for inference. To measure the improvement achieved through the application of the masks generated by AI, two processes were carried out on the unknown dataset using the ODM software with the same parameters. The difference between the processes was the inclusion of the masks generated by AI in the second process, for comparison and evaluation of the experiment.

As a result, small areas in the Digital Terrain Model were identified, which showed alterations when the AI masks were injected into the processing, indicating that, although subtle, they can influence the final quality improvement of the processing.

It was concluded that the use of AI for identifying anomalous elements in aerial survey images is highly promising for improving the quality of generating Digital Terrain Models. It is worth noting that the annotated dataset used to create the AI model was from another country, with different natural characteristics from our country (Brazil).

## Introdução

A técnica de aerofotogrametria acelera o processo de estudo do relevo por meio do aerolevantamento realizado por sensores remotos, neste caso, drones ou VANTs, que capturam imagens geralmente de forma autônoma, seguindo um plano de voo previamente definido. Após a coleta das fotos, os softwares aplicam algoritmos para gerar produtos como ortofotos e Modelos Digitais de Superfície (MDS) que representam o relevo ou a Área de Interesse (AOI ou POI - Point of Interest em inglês). O MDS, de forma simplificada, é como se um lençol fosse sobreposto ao terreno, incluindo a representação de todos os elementos capturados, como construções, casas, carros, etc. - que às vezes podem ser tratados neste experimento como anomalias, além do terreno natural.

Com algoritmos incorporados nesses softwares, é gerado o Modelo Digital de Terreno (MDT). Esses algoritmos buscam eliminar as anomalias mencionadas anteriormente, por meio de técnicas de triangulação e medição dos pontos tridimensionais gerados. Esses algoritmos fornecem um produto de alta qualidade quando bem parametrizados antes do processamento.

## Motivação
O software Open Drone Map  ([ODM](https://community.opendronemap.org)) e outros têm a capacidade de adicionar máscaras em preto/branco ([ODM Masks](https://docs.opendronemap.org/masks/)) às fotos antes do processamento, identificando as anomalias em cada foto capturada. Dessa forma, durante a geração do MDT, o processamento irá desconsiderar essas máscaras.

As máscaras que identificam anomalias devem ser criadas manualmente, de forma artesanal e individual, em cada foto, utilizando editores gráficos como o [GIMP](https://www.gimp.org/). Nesse processo, os pixels que representam as anomalias a serem excluídas do MDT devem ser pintados de preto.

Abaixo está um exemplo da imagem original, juntamente com sua respectiva máscara e o MDT processado.

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

Em um pequeno dataset com até 20 imagens, embora seja cansativo, é viável. No entanto, quando lidamos com um dataset que contém mais de 20 imagens, o que é comum em aplicações desse tipo, pois as imagens têm uma sobreposição de pelo menos 70%, torna-se uma tarefa demorada antes de poder iniciar o processamento. Nos dias atuais, com prazos mais apertados, isso pode ter um impacto direto nos cronogramas.

Este experimento se baseou em uma técnica chamada Segmentação de Imagens por meio da Arquitetura de Rede U-NET, que é uma variação do Aprendizado Profundo (Deep Learning).

Para uma melhor compreensão do experimento, apresentarei alguns conceitos importantes de tecnologia e processos que fazem parte dele.

## Conceitos
Esta seção traz alguns conceitos e informações técnicas para melhor entendimento do experimento.

### Aerofotogrametria
A aerofotogrametria é uma técnica de obtenção de informações do terreno ou superfície terrestre a partir de fotografias aéreas. Ela é utilizada para criar modelos tridimensionais, como por exemplo: modelos digitais de superfície, mapas topográficos e ortofotos de áreas geográficas extensas, representando cidades, florestas, rios, entre outros. A técnica envolve o uso de câmeras montadas em aeronaves, drones ou balões para capturar imagens aéreas do terreno. Em seguida, são aplicados algoritmos e técnicas de processamento de imagem para extrair informações geográficas e topográficas precisas e úteis para uma variedade de aplicações em engenharia, planejamento urbano, agricultura, meio ambiente, entre outras áreas.

Através de técnicas de aerofotogrametria, é possível obter:
+ Ortofotos;
+ Modelo Digital de Superfície e de Terreno;
+ Curvas de Nível;
+ Cálculos Volumétricos
+ etc...

### Segmentação de imagens
A segmentação de imagens é uma tarefa de processamento de imagens que envolve a classificação de cada pixel de uma imagem, criando regiões ou segmentos significativos, onde cada segmento representa uma região semântica da imagem. O objetivo é separar as regiões de interesse da imagem de acordo com sua semântica, facilitando a análise e o processamento dessas regiões isoladamente. A segmentação de imagens é frequentemente utilizada em aplicações de visão computacional, como reconhecimento de objetos, análise de imagens médicas, carros autônomos e robótica. Seu uso tem se mostrado mais eficaz do que a análise humana na identificação de anomalias ou elementos específicos em imagens, incluindo vídeos e capturas em tempo real.

<p align="center">
  <img alt="segim01" width=400 src= "https://miro.medium.com/v2/resize:fit:720/format:webp/1*kUrAUjDQ0zr1cegMvUrUqg.jpeg"/>
  <img alt="segim02" width=400 src="https://miro.medium.com/v2/resize:fit:786/format:webp/1*oypCvGATxpr4MJh-BM2TWw.png"/>
</p>

### Arquitetura U-Net
![](https://miro.medium.com/v2/1*n1PFaorpCSvxiIaA2Ub1bA.png)

A U-Net é uma arquitetura de rede neural convolucional desenvolvida para segmentação de imagem, proposta em 2015 por Olaf Ronneberger, Philipp Fischer e Thomas Brox. O nome "U-Net" deriva da forma da arquitetura, que se assemelha à letra "U".

A ideia por trás da U-Net é utilizar uma arquitetura de rede neural convolucional com camadas de codificação (encoding) e decodificação (decoding) para segmentar imagens. A rede tem uma estrutura simétrica, com camadas de convolução para comprimir as informações da imagem e camadas de convolução transposta para expandir as informações e gerar uma máscara de segmentação para a imagem.

A U-Net foi projetada para lidar com imagens médicas, onde a segmentação é necessária para a identificação precisa de estruturas como tumores e órgãos. No entanto, essa arquitetura tem sido bem-sucedida em outras aplicações de segmentação de imagem, como segmentação de objetos em imagens de satélite.

Devido à sua eficácia em tarefas de segmentação de imagem, a U-Net se tornou uma das arquiteturas mais populares e amplamente utilizadas em visão computacional e aprendizado profundo.

A arquitetura da U-Net pode ser considerada uma variação ou extensão de um autoencoder, pois é composta por um "encoder" que reduz a dimensão do espaço de características (representações) e um "decoder" que reconstrói a imagem original a partir dessas representações.

No entanto, a U-Net possui uma diferença significativa em relação a um autoencoder convencional: a presença de conexões de atalho (skip connections) que conectam camadas do encoder diretamente ao decoder. Essas conexões de atalho permitem a preservação de informações de alta resolução do input original, o que ajuda a melhorar o desempenho da rede na tarefa de segmentação de imagens.

Portanto, a U-Net pode ser considerada uma extensão do autoencoder convencional, projetada especificamente para tarefas de segmentação de imagens, onde a preservação da resolução espacial é fundamental.

### Aerial Semantic Segmentation Drone Dataset (400 imagens e 24 classes)
<p align="center">
  <img alt="segim01" width=400 src= "https://www.tugraz.at/fileadmin/_migrated/pics/fyler3.png"/>
</p>
O Semantic Drone dataset está disponível [aqui](https://www.tugraz.at/index.php?id=22387) ou [aqui](https://www.kaggle.com/datasets/bulentsiyah/semantic-drone-dataset).

O Semantic Drone Dataset tem como foco a compreensão semântica de cenas urbanas para melhorar a segurança dos procedimentos autônomos de voo e pouso de drones. As imagens retratam mais de 20 áreas residenciais capturadas a partir de uma visão nadir (vista aérea), adquiridas a uma altitude de 5 a 30 metros acima do solo. Foi utilizada uma câmera de alta resolução para capturar imagens com tamanho de 6000x4000 pixels (24Mpx). O conjunto de treinamento contém 400 imagens disponíveis publicamente.

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
O Drone Dataset é disponibilizado gratuitamente para entidades acadêmicas e não acadêmicas para fins não comerciais, como pesquisa acadêmica, ensino, publicações científicas ou experimentação pessoal. Ao utilizar os dados, é necessário concordar com as seguintes condições:

- O conjunto de dados é fornecido "NO ESTADO EM QUE SE ENCONTRA", sem garantia expressa ou implícita. Embora tenham sido feitos todos os esforços para garantir a precisão, nós (Universidade de Tecnologia de Graz) não nos responsabilizamos por erros ou omissões.
- É necessário incluir uma referência ao "Semantic Drone Dataset" em qualquer trabalho que faça uso do conjunto de dados. Para trabalhos de pesquisa ou outras formas de mídia, é necessário fornecer um link para a página web do Semantic Drone Dataset.
- A distribuição do conjunto de dados ou versões modificadas não é permitida. No entanto, é permitido distribuir trabalhos derivados que sejam representações abstratas deste conjunto de dados, como modelos treinados com base nele ou anotações adicionais que não incluam diretamente nenhum dos nossos dados, desde que não permitam a recuperação do conjunto de dados ou algo semelhante.
- O conjunto de dados e qualquer trabalho derivado dele não podem ser utilizados para fins comerciais, como licenciamento ou venda dos dados, ou para obter ganhos comerciais.
- Todos os direitos não expressamente concedidos são reservados por nós (Graz University of Technology).


## Metodologia
A metodologia para geração das máscaras será baseada em algoritmos de aprendizado de máquina, mais precisamente redes neurais convolucionais. O objetivo é treinar um modelo capaz de detectar automaticamente os elementos indesejados na imagem. Esse modelo foi treinado com imagens que foram manualmente anotadas para indicar a localização desses objetos na imagem. Dessa forma, o modelo pode aprender padrões específicos associados a cada tipo de elemento e ser capaz de detectá-los com maior precisão.

Para essa abordagem, foi criado um modelo baseado na arquitetura U-Net para inferências. Foi utilizado um conjunto de dados composto por 400 imagens e suas respectivas máscaras anotadas com 24 classes.

O script hospedado no Kaggle sob o link: [aerial-semantic-segmentation-puc-rio-bi-master-tcc.ipynb](https://www.kaggle.com/code/marciodelrei/aerial-semantic-segmentation-puc-rio-bi-master-tcc?kernelSessionId=134697143), mostra o treinamento do modelo. Uma cópia do notebook foi colocada neste repositório para fácil acesso sob o nome: aerial-semantic-segmentation-puc-rio-bi-master-tcc.ipynb. Ele está com o output, tornando-se pesado para renderização no view do Github.

O script está hospedado no Kaggle através do seguinte link: [aerial-semantic-segmentation-puc-rio-bi-master-tcc.ipynb](https://www.kaggle.com/code/marciodelrei/aerial-semantic-segmentation-puc-rio-bi-master-tcc?kernelSessionId=134697143). Uma cópia do notebook foi colocada neste repositório para fácil acesso, com o nome aerial-semantic-segmentation-puc-rio-bi-master-tcc.ipynb. No entanto, devido ao seu tamanho e à presença do output, a visualização do notebook pode ficar lenta no modo de visualização do GitHub.

Após realizar as inferências, um pós-processamento foi aplicado às máscaras geradas pela U-Net. Nesse processo, cada pixel das máscaras foi convertido para preto ou branco, dependendo da classificação. Os pixels que representam "não terreno" (anomalias) foram coloridos de preto, enquanto os pixels restantes de cada imagem, considerados "terreno", foram pintados de branco.

O script de inferência foi executado em uma máquina local e uma cópia do notebook foi disponibilizada neste repositório para fácil acesso, com o nome [model_inference.ipynb](https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/blob/main/model_inference.ipynb).

Foi utilizado um dataset desconhecido pelo modelo de IA com o objetivo de realizar comparações. Foram realizados dois processamentos no ODM com os mesmos parâmetros. A diferença entre eles foi que, no primeiro processamento, foram utilizadas apenas as imagens originais, enquanto no segundo, as máscaras foram adicionadas para análise e observação, a fim de criar a comparação resultante.

## Resultados
O treinamento do modelo alcançou 86% de acurácia conforme o gráfico:
![image](https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/assets/7377875/869a4432-104c-4d5c-8b37-730b18c8e535)

Conforme citado, produtos interessantes podem ser gerados, como exemplo, foi exposto no Skecthfab o Modelo Digital de Superfície para apreciação: [MDS no Sketchfab](https://skfb.ly/oIsHv)
<p align="center">
  <img alt="mds" width=800 src= "https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/blob/main/MDS_processado.jpg">
</p>

Os parâmetros nos processamento dos MDT's foram os mesmos:
<p align="center">
  <img alt="mds" width=800 src= "https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/blob/main/Painel%20-%20WebODM.png">
</p>

Como resultado, comparamos o MDT gerado pelos 2 processamentos um com Imagens Sem máscaras e outro com as Imagens Com Máscaras inferidas pela IA:
<p align="center">
  <img alt="mds" width=800 src= "https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/blob/main/MDT_compare.gif">
</p>

A imagem acima demonstra uma alteração sutil entre os MDTs, o que indica que o mascaramento influencia em sua geração.

Abaixo, foram geradas curvas de nível que representam o terreno, tornando um pouco mais claro essa diferença. As cores azuis representam o MDT sem máscaras, enquanto a linha magenta representa o MDT com máscaras de IA, comprovando a eliminação de anomalias.

<p align="center">
  <img alt="MDT sem máscaras IA- Curvas de nível de 1m" src="https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/blob/main/MDT_CN1m_SemMascara.png" width="380" hspace="10" />
  <img alt="MDT com máscaras IA - Curvas de nível de 1m" src="https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/blob/main/MDT_CN1m_ComMascara.png" width="380" hspace="10" />
</p>
<p align="center">
  <img alt="MDT comparativo" src="https://github.com/marciodelrei/Aerofotogrametria-Segmentacao-Imagens-Aereas-para-MDT-otimizado/blob/main/MDT_CN1m_Comparativo.png" width="800" hspace="10" />
</p>

## Conclusão
Com a apresentação dos resultados, mesmo que sejam sutis, a inclusão de máscaras para processamento sensibiliza os algoritmos de geração de MDT. A criação de modelos de Segmentação Semântica de Imagens com o auxílio de IA pode economizar tempo na geração de máscaras que detectam anomalias. Ao analisarmos as saídas do algoritmo do Kaggle do modelo, observamos que são necessárias muitas imagens anotadas para melhorar a precisão do modelo de IA. O treinamento do modelo de IA com conjuntos de dados anotados com cenários regionais provavelmente ajudará o modelo nas inferências.

A utilização de modelos pré-treinados, como o SAM (Segment Anything Model) do Facebook, por exemplo, não foi testada, mas pode ser explorada em trabalhos futuros com base nos algoritmos apresentados, com as devidas alterações para garantir seu pleno funcionamento.

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

