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
Ferramentas e aplicativos de Aerofotogrametria vêm crescendo e sendo usados como uma ótima alternativa para criação de modelos tridimensionais representando fielmente e virtualmente relevos reais. Dentre eles, Agisoft Metashape (antigo Agisoft Photoscan), Drone Deploy, Open Drone Map (ODM - foco deste experimento). Estes softwares se prevalecem de algoritmos de estereoscopia e triangulação de pontos conhecidos em amostras tomadas de pontos diferentes. O input destas operações, normalmente são muitas fotos do Local de Interesse. Estas operações resultam em produtos mensuráveis como Ortofotos e Modelo Digital do Terrenos em 3D, todos georrefenciados.

Para melhorar a precisão do Modelo Digital do Terreno e torná-lo mais fiel em sua representação digital, há possibilidade de se utilizar máscaras, criadas manulamente com editores gráficos, que identificam elementos fora do espectro de Terreno, como casas, construções, carros, cursos e espelhos d'água, grandes massas verdes como aglomerados de árvores e florestas. Este experimento foca na construção automática destas máscaras através de algoritmo conversor e da Segmentação de Imagens através da Aquitetura de Rede U-NET, onde serão criados modelos usando datasets diferentes com suas respectivas anotações. As máscaras serão reprocessadas nos ODM e seus resultados servirão de comparativo como produto deste experimento.

## Abstract
Title: Automatic Mask Generation for Improving Digital Terrain Models in Photogrammetry

Aerophotogrammetry tools and applications have been growing in popularity and are being used as excellent alternatives for creating three-dimensional models that faithfully and virtually represent real terrains. Among these tools are Agisoft Metashape (formerly Agisoft Photoscan), Drone Deploy, and Open Drone Map (ODM - the focus of this experiment). These software leverage stereoscopy algorithms and triangulation of known points from different samples. Typically, the input for these operations consists of multiple photos of the Area of Interest. The outputs of these operations include measurable products such as Orthophotos and 3D Digital Terrain Models, all georeferenced.

To enhance the accuracy and fidelity of the Digital Terrain Model representation, masks can be used to identify elements outside the Terrain spectrum, such as buildings, houses, vehicles, water bodies, and large green areas such as clusters of trees and forests. These masks are manually created using graphic editing software. This experiment focuses on the automatic construction of these masks using a conversion algorithm and Image Segmentation through the U-NET Network Architecture. Models will be created using different datasets and their respective annotations. The masks will then be reprocessed in ODM, and the results will serve as a comparative analysis for this experiment.

## Introdução

A técnica de Aerofotogrametria acelera o processo de estudos de relevo através do aerolevantamento feito por sensores remotos, neste caso Drones ou Vants que capturam imagens normalmente de forma autônoma, seguindo um plano de vôo previamente estudado. Após coleta das fotos, softwares aplicam algoritmos para gerarem produtos como Ortofotos e Modelos Digitais de Superfície (MDS) que representam o relevo ou o Ponto de Interesse (POI). O MDS inclui a representação de todos elementos capturados como construções, casas, carros, etc - que serão tratados neste artigo como **anomalias**, além do terreno natural.

Com algoritmos embarcados nestes softwares, gera-se o Modelo Digital de Terreno (MDT), tais algoritmos buscam eliminar as anomalias citadas acima, através de triangulação e medição dos pontos tridimensionais gerados. Estes algoritmos entregam um produto muito bom quando bem parametrizados antes de seu processamento.

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

Isto em um pequeno dataset com até 20 imagens, apesar de ser extenuante é plausível, porém quando estamos diante de um dataset acima de 20 imagens, o que é normal em aplicações deste tipo, pois as imagens se recobrem em pelo menos 70%, torna-se impraticável.

Este experimento terá como foco a técnica uma variação de Deep Learning conhecida como Segmentação de Imagens através da Arquitetura de Rede U-NET.

Para um melhor entendimento do experimento, trago alguns conceitos de tecnologia e processos importantes que fazem parte deste experimento.

## Conceitos
Esta seção traz alguns conceitos e informações técnicas para melhor entendimento do experimento.

### Aerofotogrametria
A aerofotogrametria é uma técnica de obtenção de informações do terreno ou superfície terrestre a partir de fotografias aéreas. Ela é utilizada para criar modelos digitais de superfície, mapas topográficos e ortofotos de áreas geográficas extensas, como cidades, florestas, rios, entre outros. A técnica envolve o uso de câmeras montadas em aeronaves, drones ou balões para capturar imagens aéreas do terreno. Em seguida, são aplicados algoritmos e técnicas de processamento de imagem para extrair informações geográficas e topográficas precisas e úteis para uma variedade de aplicações em engenharia, planejamento urbano, agricultura, meio ambiente, entre outras áreas.

Através de técnicas de aerofotogrametria, é possível obter:
+ Ortofotos;
+ Modelo Digital de Superfície e de Terreno;
+ Curvas de Nível;
+ Cálculos Volumétricos.

### Segmentação de imagens
Segmentação de imagens é uma tarefa de processamento de imagens que envolve a divisão de uma imagem em regiões ou segmentos significativos, onde cada segmento representa uma região semântica da imagem. O objetivo é separar as regiões de interesse da imagem de acordo com sua semântica, tornando mais fácil a análise e o processamento dessas regiões isoladamente. A segmentação de imagens é frequentemente usada em aplicações de visão computacional, como reconhecimento de objetos, análise de imagem médica e robótica.

<p align="center">
  <img alt="segim01" width=400 src= "https://miro.medium.com/v2/resize:fit:720/format:webp/1*kUrAUjDQ0zr1cegMvUrUqg.jpeg"/>
  <img alt="segim02" width=400 src="https://miro.medium.com/v2/resize:fit:786/format:webp/1*oypCvGATxpr4MJh-BM2TWw.png"/>
</p>

### Arquitetura U-Net
![](https://miro.medium.com/v2/1*n1PFaorpCSvxiIaA2Ub1bA.png)

A U-Net é uma arquitetura de rede neural convolucional desenvolvida para segmentação de imagem, que foi proposta em 2015 por Olaf Ronneberger, Philipp Fischer e Thomas Brox. O nome "U-Net" vem da forma da arquitetura, que se parece com a letra "U".

A ideia por trás do U-Net é usar uma arquitetura de rede neural convolucional com camadas de "encoding" e "decoding" para segmentar imagens. A rede tem uma forma simétrica, com camadas de convolução para comprimir as informações da imagem e camadas de convolução transposta para expandir as informações e gerar uma máscara de segmentação para a imagem.

O U-Net foi projetado para lidar com imagens médicas, onde a segmentação é necessária para a identificação precisa de estruturas como tumores e órgãos. No entanto, a arquitetura tem sido utilizada com sucesso em outras aplicações de segmentação de imagem, como segmentação de objetos em imagens de satélite e detecção de células em imagens de microscópio.

Devido à sua eficácia em tarefas de segmentação de imagem, o U-Net tornou-se uma das arquiteturas mais populares e amplamente utilizadas em visão computacional e aprendizado profundo.

A arquitetura U-Net pode ser considerada uma variação/extensão de um autoencoder, já que a sua estrutura é composta por um "encoder" que reduz a dimensão do espaço de características (representações) e um "decoder" que reconstrói a imagem original a partir dessas representações.

No entanto, a U-Net possui uma diferença significativa em relação a um autoencoder convencional: a presença de caminhos de atalho (skip connections) que conectam camadas do encoder diretamente ao decoder. Esses caminhos de atalho permitem a preservação de informações de alta resolução do input original, o que ajuda a melhorar o desempenho da rede na tarefa de segmentação de imagens.

Portanto, a U-Net pode ser considerada uma extensão do autoencoder convencional, que é projetado especificamente para tarefas de segmentação de imagens, onde a preservação da resolução espacial é fundamental.

### MBRSC dataset (72 imagens e 6 classes)
O MBRSC dataset está sob a licença CC0, disponível para [download](https://www.kaggle.com/humansintheloop/semantic-segmentation-of-aerial-imagery). Consiste em imagens aéreas de Dubait obtidas por MBRSC satélites e anotadas manualmente com segmentação semântica pixel a pixel em 6 classes. Existem três desafios principais associados ao conjunto de dados:

1. As cores das classes são hexadecimais, enquanto as imagens das máscaras são RGB.
2. O volume total do conjunto de dados é de 72 imagens agrupadas em seis blocos maiores. Setenta e duas imagens é um conjunto de dados relativamente pequeno para treinar uma rede neural.
3. Cada ladrilho tem imagens de diferentes alturas e larguras, e algumas imagens dentro dos mesmos ladrilhos variam em tamanho. O modelo de rede neural espera entradas com dimensões espaciais iguais.

![](https://miro.medium.com/v2/1*X0if3zq8fBrgr5YvpluaRw.png)

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
A metodologia para geração das máscaras, serão baseados em algoritmos de aprendizado de máquina, mais precisamente redes neurais convolucionais. A função e treinar um modelo capaz de detectar automaticamente os objetos de interesse na imagem. Esse modelo pode ser treinado com imagens anotadas manualmente que indiquem a localização desses objetos na imagem. Dessa forma, o modelo pode aprender padrões específicos associados a cada tipo de objeto e ser capaz de detectá-los com maior precisão.

Para abordagem, serão criados 2 modelos aczbaseados na arquitetura U-NET para inferências, para cada modelos serão usados 2 datasets de imagens com suas respectivas máscaras anotadas. Um dataset com 72 imagens possuindo 6 classes anotadas e outro dataset com 400 imagens com 24 classes anotadas.

Após as inferências feitas, será um pós-processamento individual em cada um das máscaras geradas pela U-NET, convertendo as cores de cada pixel para preto ou branco respeitando a classificação de "não terreno" para anomalias, sendo estes pixels coloridos de preto e os pixels restantes de cada imagem serão considerados "terreno", sendo pintados de branco.

-----------------------------------------------------------------------------------------------------------
TODO
## Resultados {#resultados}
Utilizaremos gráficos e outros recursos visuais para facilitar a compreensão dos dados. Faremos uma análise objetiva dos resultados e discutiremos sua relevância em relação ao objetivo do estudo.

## Discussão {#discussão}
Nesta seção, interpretaremos e discutiremos os resultados apresentados anteriormente. Analisaremos as descobertas à luz da literatura existente, destacando semelhanças, diferenças e contribuições para a área de estudo. Discutiremos também as limitações do estudo e possíveis direções para pesquisas futuras.

## Conclusão {#conclusão}
Resumiremos as principais descobertas do estudo e destacaremos sua relevância em relação ao problema inicialmente proposto. Evitaremos repetir informações já apresentadas e forneceremos uma visão geral clara do trabalho realizado.

## Referências e Links
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

Solução baseada em modelo U-NET usando como fonte de conhecimento o artigo "[Semantic Segmentation of Aerial Imagery Using U-Net in Python](https://towardsdatascience.com/semantic-segmentation-of-aerial-imagery-using-u-net-in-python-552705238514)" de Andrew Joseph Davies
