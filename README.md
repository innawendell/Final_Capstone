## Ads Text Generation
### Background :speaker:
Currently, humans write ads themselves, however, this genre has many "go-to" clichés. Therefore, it could be worthwhile to create a product that helps people save time by automating the process of ad creation. In this project, I explore some possible solutions to this challenge:

* Part I. Recommendation Systems that suggest appropriate ad texts /ad headlines from the existing database of texts and headlines
* Part II. First experimental work towards generating ads texts from scratch 

## Part I :sunrise:

### The Data
The data was obtained from: https://www.kaggle.com/kotobotov/context-advertising. The database was collected in October 2016 - January 2017. It contains ads from such countries as Russia, Ukraine, Belarus, and Kazakhstan and includes a variety of advertised product types and services.

### Solution One. Recommendation Systems - Unsupervised Approach :paperclip: 
#### Stages:
- [x]  Lemmatize with Mystem parser
- [x] Training Word2Vec
- [x]  Creation of document vectors from word vectors
- [x]  Best matches based on cosine similarity
- [x]  Evaluation based on ranking

#### Unsupervised Model Results: :bar_chart:
* Mean rank of the correct headline: **16.98**
* Proportion of the correct headlines in the top three results: **0.837**

### Solution Two. Recommendation Systems - Supervised Approach :paperclip:

#### Stages:
- [x]  We split our dataset into half: correct and incorrect ads text –ads headline pairs (second half is shuffled)
- [x]  Features: concatenated ads text and ads headline vectors with PCA applied
- [x]  Train a binary classifier to identify correct /incorrect pairs (0/1)
- [x]  Evaluate not only based on accuracy but also the rank of the correct pair (out of all other possible pair combinations in a sample of 1000)

#### Models:
* Logistic Regression
* Random Forest
* XGBoost
* Recurrent Neural Network (RNN): different preprocessing pipeline -  each wordvector is indexed in an embeddings matrix, which is used in an embeddings layer

#### Supervised Models. Best Results: :bar_chart:
* XGBoost: mean rank of the correct headline: **10.35**, proportion of the correct headlines in the top 3 results: **0.855**
* RNN: mean rank of the correct headline: **10.835**, proportion of the correct headlines in the top 3 results: **0.79**

## Part II. Text Generation :rocket:

#### Stages:
- [x]  Preprocessing: no lemmatization because we want to preserve correct grammatical forms
- [x]  Training word2vec model
- [x]  Creation of unique indices for the most frequent words in the vocabulary
- [x]  Creation of an embedding matrix that keeps all the word vector values by index to be used in our Embedding layer
- [x]  Creation of predictors and labels, e.g. in the indexed sentence "I love data science" [1, 2, 3, 4]:  predictor [1, 2] - label [3], predictor [1, 2, 3] - label [4]
- [x]  Padding all predictors to the same lengths (due to Keras' inner workings)

#### Some successful examples: 
Russian  | Translation
------------ | -------------
“окна”| “windows”
“окна в хабаровске от производителя скидка на все 5% бесплатная доставка сравните цены всех интернет магазинов”|”windows in khabarovsk (directly) from a manufacturer 5% off free delivery compare prices of all online stores”)
“Blow dryer” |  “фен”
'фен philips низкая цена гарантия большой выбор быстрая доставка гарантия жми сравните цены всех интернет магазинов’|‘blow dryer phillips low price warranty large selection fast delivery warranty click (here) compare prices of all online stores

#### Evaluation: :clipboard:
Despite some noticeable glitches (overuse of certain frequent phrases like "compare prices of all internet stores", "directly from the manufacturer" and inappropriate insertion of words ('hp' in texts about knives, apartments, and hotels and '220 volt' in a text about hotels, the model still looks very promising because it has managed to produce grammatically correct forms (e.g. the genitive case after the prepositions "от" and "для"("from" and "for"), the prepositional case for "location". Additionally, it has learned some frequently used catch phrases within the advertising genre ("low prices", "warranty", "free delivery", "discounts", "sale", "10% off")

#### Future Goals:
* Refine the model and find metrics to measure its performance
* Generate ads based on product key words or product descriptions 
* Sell our generator to Yandex :metal: 


 


















