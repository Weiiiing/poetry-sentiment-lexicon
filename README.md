# poetry-sentiment-lexicon
A sentiment lexicon in classical Chinese poetry domain constructed by deep learning

## 1. Introduction
- Based on a corpus of “Appreciation Dictionary of Tang Poetry”, a poetry sentiment lexicon is constructed by combining supervised sentiment term extraction (entity recognition) and sentiment term classification (text classification). 

## 2. File
- "Hownet" is the China HowNet emotional dictionary.
- "NTUSD" is the National Taiwan University sentiment lexicon.
- "Li Jun lexicon" is the Li Jun sentiment lexicon of Tsinghua University.
- "DUTIR" is the ontology database of emotion vocabulary of the Dalian University of Technology.
- "FCCPSL" is the constructed poetry sentiment lexicon.
- "Appreciation Dictionary of Tang Poetry" is the experimental corpus.

## 3. Methods and results
### 3.1 Sentiment term extraction
A list of multi-source sentiment words is collected from the general sentiment vocabularies and related domains to form the knowledge base. Next, the sentiment words are used to match the terms in the domain text, and the annotated text is then mapped to a character labeling sequence. Next, the BERT model is used to realize Chinese character embedding, after which the emotion radical (ER) features of Chinese characters are incorporated to extend the BERT vector to boost semantic information. The enhanced vector is input into a neural network to predict the labels of sequences to extract sentiment terms. The extracted terms include domain terms in the original vocabularies and unregistered terms predicted by the model. The results are as follows:


|feature|P(%)|R(%)|F1(%)|Labeled|Recognized|Correct|  
|------|---|---|---|---|---|---|
|BERT|96.31|95.60|95.95|60225|59780|57574|
|BERT+ER|96.29|95.89|96.09|60225|59977|57750|


### 3.2 Sentiment term classification
To classify extracted terms, the knowledge base of the adjusted DUTIR was resorted to label domain term classes to start deep learning. The word knowledge of each term (term definitions, synonyms, and associative words) is integrated to augment the term embedding. We used BiLSTM-Attention to train the classication model. After obtaining the optimal model, the unannotated terms are predicted by the model via hierarchical supervised classification according to the term class structure of the adjusted DUTIR. The predicted terms are finally merged into the annotated terms to construct a hierarchical sentiment lexicon.


|Class|Macro_P (%)|Macro_R (%)|Macro_F1 (%)|Acc (%)|
|------|---|---|---|---|
|total(8081)|91.55|91.19|91.23|91.38|
|positive(4513)|81.67|68.37|73.4|89.16|
|negative(3568)|82.99|80.03|81.11|85.03|
|pleasure(733)|81.22|78.6|79.58|85.26|
|favour(3707)|68.61|45.38|50.63|85.32|
|sadness(1049)|84.03|76.52|79.07|85.04|
|disgust(2519)|79.5|60|65.72|86.18|


## 4. FCCPSL structure
The FCCPSL with a three-layer hierarchy and 14,368 terms was built.

![image](https://user-images.githubusercontent.com/55570101/157238183-f34a7bfc-6b47-445d-8327-05ea7fcd66fb.png)

Detailed information of the third-layer class:

|class|term number|  
|------|---|
|praise |5582|
|like |658|
|ease |305|
|joy |835|
|faith |251|
|wish |153|
|peculiar |96|
|criticize |3665|
|sorrow |1313|
|vexed |574|
|guilt |83|
|fear |494|
|misgive |80|
|anger |152|
|miss |127|

## Declaration
This work is the original research of the "Data Analysis and Computing" group in the School of Information Management, Nanjing University.
