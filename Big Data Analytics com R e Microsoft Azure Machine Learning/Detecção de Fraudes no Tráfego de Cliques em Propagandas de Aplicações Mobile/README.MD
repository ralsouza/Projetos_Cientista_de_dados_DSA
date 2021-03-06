# Detecção de Fraudes no Tráfego de Cliques em Propagandas de Aplicações Mobile

A TalkingData (https://www.talkingdata.com), a maior plataforma de Big Data independente da China, cobre mais de 70% dos dispositivos móveis ativos em todo o país. Eles lidam com 3 bilhões de cliques por dia, dos quais 90% são potencialmente fraudulentos. Sua abordagem atual para impedir fraudes de cliques para desenvolvedores de aplicativos é medir a jornada do clique de um usuário em todo o portfólio e sinalizar endereços IP que produzem muitos cliques, mas nunca acabam instalando aplicativos. Com essas informações, eles criaram uma lista negra de IPs e uma lista negra de dispositivos. Embora bem-sucedidos, eles querem estar sempre um passo à frente dos fraudadores e pediram a sua ajuda para desenvolver ainda mais a solução. Você está desafiado a criar um algoritmo que possa prever se um usuário fará o download de um aplicativo depois de clicar em um anúncio de aplicativo para dispositivos móveis.
Em resumo, neste projeto,o objetivo éconstruir um modelo de aprendizado de máquina para determinar se um clique é fraudulento ou não.

## **Limitações de Hardware**

O arquivo original deste projeto fornecido pela empresa TalkingData era composto de 200 milhões de observações ao longo de 4 dias de atividade. Dessa forma não foi possível processar toda essa quantia de dados em minha máquina, necessitando de um supercomputador ou a adoção de um cluster de computadores. Sendo assim, o projeto inteiro foi feito utilizando 500.000 amostras randômicas deste dataset. O arquivo completo pode ser encontrado em : https://www.kaggle.com/c/talkingdata-adtracking-fraud-detection/data

### **Descrição dos Arquivos**

train.csv - arquivo contendo os dados necessários para a execução do projeto

### **Descrição dos Campos**

- ip: Endereço de ip onde o clique foi feito
- app id: Em qual aplicativo o clique foi feito
- device: Aparelho telefônico no qual o clique foi feito
- os: Versão do sistema operacional do aparelho
- channel: O ID do canal do 'publisher' 
- click_time: Horário do clique
- attributed_time: Essa variável mostra a hora exata do download (caso ele tenha realmente ocorrido)
- is_attributed: Se o clique foi fraudulento ou não ('0' = fraude, '1' = clique legítimo)

![image](https://user-images.githubusercontent.com/45671820/53414157-d60f5d00-39ac-11e9-839d-48cdc2059404.png)

![image](https://user-images.githubusercontent.com/45671820/53414196-eaebf080-39ac-11e9-9d87-2506d0902671.png)

## **Tratamento dos dados**
### **Pré-Processamento e Limpeza**

Neste projeto as práticas de pré-processamento e limpeza dos dados foram:

- Verificação de valores NA, 0 e nulos: Nenhuma anomalia foi encontrada
- Conversão da variável resposta em fator
- Exclusão da variável attributed_time: Essa variável é uma redundância da variável resposta, ou seja, mostra a mesma informação de forma diferente. Se a variável 'attributed_time' apresenta algum valor significa que houve download do aplicativo e, portanto, que o  clique não foi fraudulento. No entanto essa informação já consta na variável resposta (is_attributed). Sendo assim, a coluna foi excluída para evitar um treinamento errado do modelo preditivo no futuro.
- Extração das horas da variável 'click_time': Este dataset é composto pelos dados referentes aos cliques de menos de um dia. Sendo assim, informações como dia/ano/mes/segundo/minuto não possuem importância para a análise deste projeto. Dessa forma, somente as horas serão utuilizadas com o objetivo de identificar os horários picos de fraude etc.
- Como as variáveis preditoras já vieram em modo 'encode' no dataset, nenhum tratamento de conversão de variáveis precisou ser feito.

### **Balanceamento dos dados**

O gráfico abaixo nos mostra que a amostra de 500.000 cliques está desbalanceada com uma predominância significa de cliques fraudulentos ('0'), o que é um fato esperado ja que a consultoria TalkingData afirma que as fraudes representam 90% dos cliques diários. Esse desbalanceamento pode gerar tendências durante o treinamento do modelo preditivo.

![image](https://user-images.githubusercontent.com/45671820/53415572-5c796e00-39b0-11e9-89c4-45c43d4e0ca8.png)

Dessa forma, a técnica de oversampling foi utilizada para balancear o dataset. Essa amostra consiste em criar observações da classe minoritária (no caso, cliques legítimos '1').

![image](https://user-images.githubusercontent.com/45671820/53415588-6602d600-39b0-11e9-8345-9e521692c641.png)

Agora o nosso dataset conta com 1.000.000 de observações.

## **Análise explorátoria**

A análise explorátoria deste projeto será conduzida em duas frentes: 

- Informações sobre todos os cliques fraudulentos
- Informações somente sobre os cliques legítimos

Essa estratégia foi abordada pois cada uma das análises geram informações diferentes.

### **Análise dos cliques fraudulentos**

![rplot1](https://user-images.githubusercontent.com/45671820/53416178-a7e04c00-39b1-11e9-8780-a73387d0c319.png)

![rplot2](https://user-images.githubusercontent.com/45671820/53416230-cc3c2880-39b1-11e9-8145-2c17092bb12b.png)

![rplot3](https://user-images.githubusercontent.com/45671820/53416233-cc3c2880-39b1-11e9-9a99-ca45880224eb.png)

![rplot4](https://user-images.githubusercontent.com/45671820/53416234-cc3c2880-39b1-11e9-92d9-527e88c323e2.png)

![rplot5](https://user-images.githubusercontent.com/45671820/53416235-ccd4bf00-39b1-11e9-8565-7b017affa53e.png)

### **Análise dos cliques legítimos**

![rplot6](https://user-images.githubusercontent.com/45671820/53416486-64d2a880-39b2-11e9-925b-07f701662ff5.png)

![rplot7](https://user-images.githubusercontent.com/45671820/53416488-64d2a880-39b2-11e9-841f-7b78d6d08118.png)

![rplot8](https://user-images.githubusercontent.com/45671820/53416489-656b3f00-39b2-11e9-9735-fdfe4dde8d8d.png)

![rplot9](https://user-images.githubusercontent.com/45671820/53416490-656b3f00-39b2-11e9-9a0f-a3ad2f6dc33b.png)

![rplot10](https://user-images.githubusercontent.com/45671820/53416491-656b3f00-39b2-11e9-8d44-258c53d58e2c.png)

A importância de realizar as análises de cliques fraudulentos/legítimos de forma separada pode ser visto nos gráficos 5 e 10. O horário de pico da ocorrência de cliques fraudulentos ocorre as 0h e 4h da manhã, enquanto o horário de maior quantidade de cliques legítimos ocorre entre 2h e 3h da manhã. 

## **Feature Selection (Seleção de variáveis importantes)**

Utilizando o pacote 'randomForest' e 'caret', a importância das variáveis para o modelo foi calculada de forma que as três variáveis mais importantes fossem selecionadas.

![image](https://user-images.githubusercontent.com/45671820/53417314-1d4d1c00-39b4-11e9-98a2-5840103b679c.png)

Sendo assim, as variáveis escolhidas para fazer parte do modelo preditivo foram: o ip que originou o clique, em qual canal do publisher o clique foi feito, e qual o aplicativo que foi clicado.

## **Criação do Modelo Preditivo**

O dataset foi divido entre dados de treino/teste na proporção 70/30 e o modelo de machine learning foi feito a partir do algoritmo RandomForest.

**RandomForest**: Esse método consiste na criação de árvores de decisão que irão direcionar a classificação de um determinado objeto de acordo com o que foi aprendido pelo modelo durante a fase de treino por meio dos nós de decisão (confira a figura abaixo):

![image](https://user-images.githubusercontent.com/45671820/53093389-0e6af300-34f6-11e9-87a0-fe43bbe590f7.png)

Na figura abaixo podemos observar o desempenho do modelo nos dados de treino (70% do dataset)

![image](https://user-images.githubusercontent.com/45671820/53417983-82edd800-39b5-11e9-8c48-159c9a3ef080.png)

Utilizando as três variáveis escolhidas na etapa de Feature Selection os cliques de teste (30% do dataset balanceado) foram usados para verificar a acurácia das previsões. O resultado pode ser visto abaixo:

![image](https://user-images.githubusercontent.com/45671820/53418053-b466a380-39b5-11e9-9081-beaeb1c1b4c7.png)

Através do nível de precisão é possível verificar que o algoritmo RandomForest obteve um ótimo desempenho de 96,1%.

Nos problemas de classificação, quatro possibilidades podem ocorrer:

- Verdadeiro positivo: Quando o modelo preditivo indica um clique fraudulento e acerta
- Falso Positivo: Quando o modelo preditivo indica um clique legítimo e erra
- Verdadeiro negativo: Quando o modelo preditivo indica um clique fraudulento e erra
- Falso negativo: Quando o modelo preditivo indica um clique legítimo e acerta

Dessa forma, essa taxa de acerto destas possibilidades pode ser vista através da curva ROC, que mostra o quão bom o modelo é capaz de diferenciar as classes (no caso, cliques fraudulentos e legítimos) por meio da Área sobre a Curva (AUC). Quanto maior a AUC, melhor é o modelo preditivo.

![image](https://user-images.githubusercontent.com/45671820/53418290-38209000-39b6-11e9-9a01-c5f97eb9e480.png)

## **Considerações**

As limitações de hardware não permitiram que o projeto fosse executado em sua potência máxima. Por mais que o modelo preditivo tenha apresentado uma precisão de 96,1% o dataset utilizado para o projeto contém poucas informações de um pequeno espaço de tempo (500.000 observações de aproximadamente um dia). Sendo assim ao utilizar esse modelo em futuras ocasiões pode ser possível presenciar uma queda de rendimento do algoritmo, ja que os novos dados de entrada serão muito maiores e englobarão cliques feitos em períodos muito maiores (semanas/meses) nos quais o algoritmo não foi treinado.

No entanto foi possível alcançar um excelente resultado levando em consideração o contexto apresentado acima. O pré processamento dos dados foi de extrema importãncia para a análise exploratória do modelo pois foi possível identificar, dentre outros insights preciosos para o negócio, os horários de maior ocorrência de fraudes e de downloads legítimos. Com essas informações, a empresa TalkingData pode estruturar uma tomada de decisão que envolva reforçar a segurança de seus servidores com prioridade de recursos nestes horários e de tratar as informações obtidas nessa faixa de horário com mais cautela.

A análise também indicou que praticamente todos os cliques fraudulentos são originados do mesmo smartphone. Nesse caso, seria interessante analisar a possibilidade de notificar a marca fabricante sobre o ocorrido pois pode indicar a presença de alguma falha de segurança interna do aparelho.

Por fim, o cientista de dados pode sugerir a criação de um projeto de Real-Time Analytics para a análise de detecção de fraudes. Dessa forma as informações são processadas em tempo real e os resultados a respeito de cliques fraudulentos ou legítimos são atualizados de maneira ininterrupta, facilitando assim a rápida tomada de decisão dos gestores.





