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
To classify extracted terms and make it adaptative to the poetry domain, the knowledge base of the adjusted DUTIR was resorted to label domain term classes to start deep learning. 

The original DUTIR is as follows:

|Class|Subclass
|------|---|
|pleasure|joy (PA)|
|pleasure|ease (PE)|
|favour|respect (PD)|
|favour|praise (PH)|
|favour|faith (PG)|
|favour|like (PB)|
|favour|wish (PK)|
|surprise|surprise (PC)|
|sadness|sorrow (NB)|
|sadness|disappoint (NJ)|
|sadness|guilt (NH)|
|sadness|miss (PF)|
|fear|panic (NI)|
|fear|fear (NC)|
|fear|ashamed (NG)|
|disgust|criticize (NN)|
|disgust|abhor (ND)|
|disgust|envy (NK)|
|disgust|vexed (NE)|
|disgust|misgive(NL)|
|anger|anger (NA)|

The adjusted DUTIR is as follows.

First class|Second class|Third class
|------|---|---|
|positive|pleasure|joy (PA)
|positive|pleasure|ease (PE)
|positive|favour|praise (PD/PH)
|positive|favour|like (PB)
|positive|favour|faith (PG)
|positive|favour|wish (PK)
|positive|surprise|surprise (PC)
|negative|sadness|fear (NI/NC)
|negative|sadness|sorrow (NB/NJ)
|negative|sadness|guilt (NG/NH)
|negative|sadness|miss (PF)
|negative|disgust|criticize (NN/ND/NK)
|negative|disgust|anger (NA)
|negative|disgust|vexed (NE)
|negative|disgust|misgive (NL)

The main changes included the following: 
- the meaning of “respect” in classical poetry is that someone holds another in high esteem or certainty, which can be considered as a type of admiration; thus, it was merged into the class of “praise.”
- both “fear” and “panic” essentially represent a sad feeling of fright, so they were included in the subordinate class of “sadnes.”
- considering that “sorrow” and “disappoint” share a coincident emotion of heart suffering, they were combined.
- “guilt” and “ashamed” have the same connotation of self-reproach, so they were integrated to form the class of “guilt.”
- because “criticize”, “abhor,” and “envy” all contain the meaning of reproach, they were united into the class of “criticize.”
- the class “anger” in classical poetry often express one’s indignation of concrete objects; thus, it was moved to the subordinate class of “disgust.” 
- because “surprise” mainly contains an unexpected happiness in poetry domain and existing work mostly dealt it as positive emotion, it was further incorporated with “pleasure” and “favour” to form the first class of “positive,” and the other second classes of “disgust” and “sadness” formed the first class of “negative.” 

Then the word knowledge of each term (term definitions, synonyms, and associative words) is integrated to augment the term embedding. The pre-trained BERT model was used to extract the feature vector of the augmented text. Then we used BiLSTM-Attention to train the classication model. After obtaining the optimal model, the unannotated terms are predicted by the model via hierarchical supervised classification according to the term class structure of the adjusted DUTIR. The predicted terms are finally merged into the annotated terms to construct a hierarchical sentiment lexicon.


|Superclass|Subclass|Macro_P (%)|Macro_R (%)|Macro_F1 (%)|Acc (%)|
|------|---|---|---|---|---|
|total(8081)|positive/negative|91.55|91.19|91.23|91.38|
|positive(4513)|pleasure/favour/surprise|81.67|68.37|73.4|89.16|
|negative(3568)|sadness/disgust|82.99|80.03|81.11|85.03|
|pleasure(733)|joy/ease|81.22|78.6|79.58|85.26|
|favour(3707)|praise/like/faith/wish|68.61|45.38|50.63|85.32|
|sadness(1049)|fear/sorrow/guilt/miss|84.03|76.52|79.07|85.04|
|disgust(2519)|criticize/anger/vexed/misgive|79.5|60|65.72|86.18|


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
