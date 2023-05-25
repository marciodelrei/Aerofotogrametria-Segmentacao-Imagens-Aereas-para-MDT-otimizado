# Segmentação de Imagens Aéreas para MDT otimizado

## Índice
1. [Resumo/Abstract](#resumoabstract)
2. [Introdução](#introdução)
3. [Metodologia](#metodologia)
4. [Resultados](#resultados)
5. [Discussão](#discussão)
6. [Conclusão](#conclusão)
7. [Referências](#referências)

## Resumo/Abstract {#resumoabstract}
Uma breve descrição do artigo que fornece uma visão geral do problema, da metodologia utilizada, dos principais resultados e das conclusões.

## Introdução {#introdução}
Nesta seção, será introduzido o problema que está sendo abordado, apresentada sua relevância e explicado o objetivo do artigo. Além disso, forneceremos um contexto teórico, revisão da literatura relevante e estabeleceremos a estrutura do restante do artigo.

## Metodologia {#metodologia}
Descreveremos detalhadamente os métodos e procedimentos utilizados para resolver o problema. Explicaremos a abordagem adotada, as técnicas ou ferramentas empregadas, bem como a coleta e análise dos dados. Forneceremos informações suficientes para que outros pesquisadores possam reproduzir o estudo.

## Resultados {#resultados}
Apresentaremos os resultados obtidos em relação ao problema em questão. Utilizaremos tabelas, gráficos ou outros recursos visuais para facilitar a compreensão dos dados. Faremos uma análise objetiva dos resultados e discutiremos sua relevância em relação ao objetivo do estudo.

## Discussão {#discussão}
Nesta seção, interpretaremos e discutiremos os resultados apresentados anteriormente. Analisaremos as descobertas à luz da literatura existente, destacando semelhanças, diferenças e contribuições para a área de estudo. Discutiremos também as limitações do estudo e possíveis direções para pesquisas futuras.

## Conclusão {#conclusão}
Resumiremos as principais descobertas do estudo e destacaremos sua relevância em relação ao problema inicialmente proposto. Evitaremos repetir informações já apresentadas e forneceremos uma visão geral clara do trabalho realizado.

## Referências {#referências}
Listaremos todas as fontes citadas ao longo do artigo, seguindo o estilo de citação apropriado (por exemplo, APA, MLA, ABNT). Certificaremos de incluir todas as informações necessárias para que os leitores possam localizar as fontes.

------------------------------------------------------------------------------------------------------------
<p>Drone-Image-Segmentation</p>
Drone Image Segmentation -U-Net architecture based on Manoj Gopalakrishna in Kaggle
[Manoj ](https://www.kaggle.com/code/manojgowda65/drone-image-segmentation-u-net-architecture)
https://www.kaggle.com/code/marciodelrei/drone-image-segmentation/edit
<p>Drone-Image-Segmentation</p>
Drone Image Segmentation -U-Net architecture based on Manoj Gopalakrishna in Kaggle
[Manoj ](https://www.kaggle.com/code/manojgowda65/drone-image-segmentation-u-net-architecture)
https://www.kaggle.com/code/marciodelrei/drone-image-segmentation/edit

------------------------------------------------------------------------------
# Segmentação de Imagens Aéreas para MDT otimizado
<span font-color="red"> Criar indice </span>
## Introdução
<!-- 
![intro01](http://static.wixstatic.com/media/32ebcf_1113d51260504980b1c65fedfba2383d~mv2.jpg/v1/fill/w_737,h_605,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/32ebcf_1113d51260504980b1c65fedfba2383d~mv2.jpg) -->

<p align=center>
  <img alt="intro01" src="http://static.wixstatic.com/media/32ebcf_1113d51260504980b1c65fedfba2383d~mv2.jpg/v1/fill/w_737,h_605,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/32ebcf_1113d51260504980b1c65fedfba2383d~mv2.jpg" width="400" />
  <img alt="intro02" src="https://www.ptatopografia.com.br/imagens/informacoes/empresas-aerofotogrametria-07.jpg" width="400" />
  
</p>
<p align="center">
  <i>buscar fonte das imagens</i>
</p>

Solução baseada em modelo U-NET usando como fonte de conhecimento o artigo "[Semantic Segmentation of Aerial Imagery Using U-Net in Python](https://towardsdatascience.com/semantic-segmentation-of-aerial-imagery-using-u-net-in-python-552705238514)" de Andrew Joseph Davies

## Aerofotogrametria
A aerofotogrametria é uma técnica de obtenção de informações do terreno ou superfície terrestre a partir de fotografias aéreas. Ela é utilizada para criar modelos digitais de superfície, mapas topográficos e ortofotos de áreas geográficas extensas, como cidades, florestas, rios, entre outros. A técnica envolve o uso de câmeras montadas em aeronaves, drones ou balões para capturar imagens aéreas do terreno. Em seguida, são aplicados algoritmos e técnicas de processamento de imagem para extrair informações geográficas e topográficas precisas e úteis para uma variedade de aplicações em engenharia, planejamento urbano, agricultura, meio ambiente, entre outras áreas.

## Softwares

## MDT - Modelo Digital de Terreno

## MDS - Modelo Digital de Superfície

## Visão Geral dos algoritmos de geração 3D de Superfície (MDS) e Terrenos (MDT)

## Problema
Softwares de aerofotogrametria possuem algoritmos para identificar alterações altimétricas no Modelo Digital de Superfície (MDS) para criação de Modelos Digitais de Terreno (MDT), excluindo construções, casas, prédios, árvores de grande porte, carros e automóveis em geral. Isto para se obter um perfil mais preciso do Terreno. Estes algoritmos, apesar de satisfatórios, não são precisos, uma vez que buscam distâncias e ângulos de pontos tridimensionais para identificar estes elementos.

## Abordagem e Estudos para melhoria de resultados
Uma abordagem possível para melhorar a precisão dos algoritmos de identificação de alterações altimétricas no MDS seria utilizar técnicas de processamento de imagens que permitam a detecção automática de objetos específicos (como construções, casas, prédios, árvores e carros) na imagem. Essas técnicas podem incluir segmentação de imagem, classificação de objetos, detecção de bordas e outras técnicas de processamento de imagem.

Uma possibilidade seria utilizar algoritmos de aprendizado de máquina, como redes neurais convolucionais, para treinar um modelo capaz de detectar automaticamente os objetos de interesse na imagem. Esse modelo pode ser treinado com imagens rotuladas manualmente que indiquem a localização desses objetos na imagem. Dessa forma, o modelo pode aprender padrões específicos associados a cada tipo de objeto e ser capaz de detectá-los com maior precisão.

### Segmentação de imagens
Segmentação de imagens é uma tarefa de processamento de imagens que envolve a divisão de uma imagem em regiões ou segmentos significativos, onde cada segmento representa uma região semântica da imagem. O objetivo é separar as regiões de interesse da imagem de acordo com sua semântica, tornando mais fácil a análise e o processamento dessas regiões isoladamente. A segmentação de imagens é frequentemente usada em aplicações de visão computacional, como reconhecimento de objetos, análise de imagem médica e robótica.

<p align="center">
  <img alt="segim01" width=400 src= "https://miro.medium.com/v2/resize:fit:720/format:webp/1*kUrAUjDQ0zr1cegMvUrUqg.jpeg"/>
  <img alt="segim02" width=400 src="https://miro.medium.com/v2/resize:fit:786/format:webp/1*oypCvGATxpr4MJh-BM2TWw.png"/>
</p>

### U-Net
O modelo U-Net é uma arquitetura de rede neural convolucional desenvolvida para segmentação de imagem, que foi proposta em 2015 por Olaf Ronneberger, Philipp Fischer e Thomas Brox. O nome "U-Net" vem da forma da arquitetura, que se parece com a letra "U".

A ideia por trás do U-Net é usar uma arquitetura de rede neural convolucional com camadas de "encoding" e "decoding" para segmentar imagens. A rede tem uma forma simétrica, com camadas de convolução para comprimir as informações da imagem e camadas de convolução transposta para expandir as informações e gerar uma máscara de segmentação para a imagem.

O U-Net foi projetado para lidar com imagens médicas, onde a segmentação é necessária para a identificação precisa de estruturas como tumores e órgãos. No entanto, a arquitetura tem sido utilizada com sucesso em outras aplicações de segmentação de imagem, como segmentação de objetos em imagens de satélite e detecção de células em imagens de microscópio.

Devido à sua eficácia em tarefas de segmentação de imagem, o U-Net tornou-se uma das arquiteturas mais populares e amplamente utilizadas em visão computacional e aprendizado profundo.

A arquitetura U-Net pode ser considerada uma variação/extensão de um autoencoder, já que a sua estrutura é composta por um "encoder" que reduz a dimensão do espaço de características (representações) e um "decoder" que reconstrói a imagem original a partir dessas representações.

No entanto, a U-Net possui uma diferença significativa em relação a um autoencoder convencional: a presença de caminhos de atalho (skip connections) que conectam camadas do encoder diretamente ao decoder. Esses caminhos de atalho permitem a preservação de informações de alta resolução do input original, o que ajuda a melhorar o desempenho da rede na tarefa de segmentação de imagens.

Portanto, a U-Net pode ser considerada uma extensão do autoencoder convencional, que é projetado especificamente para tarefas de segmentação de imagens, onde a preservação da resolução espacial é fundamental.

**U-Net Arquitetura**
U-Net consists of two critical paths:

    Contraction corresponds to general convolution Conv2D operations, where filters slide across the input image extracting features.
    Following Conv2D, the MaxPooling2D layer operates on groups of pixels and filters values by selecting the maximum. Pooling downsamples the input image size while maintaining feature information.
    Expansion: the downsampled image expands using transpose convolutions or deconvolutions, Conv2DTranspose, to recover the input image spatial information. While upsampling, skip connections concatenate the features between the symmetric contraction and expansion layers.
![](https://miro.medium.com/v2/1*n1PFaorpCSvxiIaA2Ub1bA.png)

### Segmentation Anything da Meta

## Modelos

### Modelo pré-treinado
The MBRSC dataset exists under the CC0 license, available to [download](https://www.kaggle.com/humansintheloop/semantic-segmentation-of-aerial-imagery). It consists of aerial imagery of Dubai obtained by MBRSC satellites and annotated with pixel-wise semantic segmentation in 6 classes. There are three main challenges associated with the dataset:
 
1.     Class colours are in hex, whilst the mask images are in RGB.
2.     The total volume of the dataset is 72 images grouped into six larger tiles. Seventy-two images is a relatively small dataset for training a neural network.
3.     Each tile has images of different heights and widths, and some pictures within the same tiles are variable in size. The neural network model expects inputs with equal spatial dimensions.

![](https://miro.medium.com/v2/1*X0if3zq8fBrgr5YvpluaRw.png)

### Modelo customizado com dados do próprio dataset
## Conclusão
Due to limited computing power, the neural network size was restricted, and training iterations did not exceed 20 epochs. Currently, there are around 1.4 million trainable parameters, as shown in Figure 6. Therefore, model performance can undoubtedly be improved by hyperparameter tuning and employing a more sophisticated network.

Figure 9 depicts some of the least successful segmentations. Some similarities are noticeable in the left-most column of aerial images. Each image is very bright, containing many pixels towards the white end of the RGB spectrum. Comparably, inspecting Figure 8, the most acceptable segmentations are on higher contrasted images. Therefore, it may be reasonable to synthesise additional brighter training images or fine-tune the model on a subset to improve performance.

## Referências e Links
- [satellite-image-deep-learning](https://www.satellite-image-deep-learning.com) de Robin Cole. Deep learning applied to satellite imagery
[Github](https://github.com/satellite-image-deep-learning)

- [Semantic Segmentation of Aerial Imagery Using U-Net in Python](https://towardsdatascience.com/semantic-segmentation-of-aerial-imagery-using-u-net-in-python-552705238514) de Andrew Joseph Davies
[Github](https://github.com/satellite-image-deep-learning)

- [MBRSC dataset](https://www.kaggle.com/humansintheloop/semantic-segmentation-of-aerial-imagery)
