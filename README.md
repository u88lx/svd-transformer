
# Experiment 4 - Transformer for Software Vulnerability Detection

This repo is for the application of Transformer based Neural Network for software vulnerability detection

## Transformer
* Originally developed for NLP 
* Original paper, Attention is All You Need (https://arxiv.org/abs/1706.03762)
* Consisted of 2 parts,`Encoder` and `Decoder`.  

## Datasets

* VDiscovery from Russell et. al (2018) https://arxiv.org/abs/1807.04320
* Original dataset can be downloaded from here https://osf.io/d45bw/

## Methodology

#### Tokkenization
* We tokkenize the source codes based on the syntactic structure.
* Punctuations are kept because they are crucial in source codes. Therefore, if we remove them, we will love the syntactic structure of the source codes, thus alternate the semantic meaning of that functions.

#### Transformer + Bi-directional Long-short Term Memory
* We implemented the Transformer encoder layer only to train embeddings of the source codes.
* The output of the encoder is feed into a 2 layer bi-directional LSTM.
* The LSTM will output 2 tensors.
* Softmax activation with cross entropy loss is used.

## Training & Evaluation

#### Model training
* The ratio of the dataset for training : validation : testing  = 80% : 10% : 10% 
* The ratio follows the actual ratio used in Russell et. al (2018)
* The training session:
	* 8 x Nvidia Tesla K80 (11GB memory) / 8 x Nvidia Tesla K80
	* Pytorch 1.2 (CuDNN v9.2)
	* Training time ~90 minutes per epoch / 180+ minutes per epoch
	
#### Model tuning
Highlighted the best value for each metric.  

|Name| Tokenizer|Ep|Weights|Vocab|TP|FP|TN|FN|Acc|Pr|Rec|PR-AUC|AUC|MCC|F1|
|--- |---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
| Run_1|Custom|11|1:5|10K|  5834 	|  **6974**  	| **112192**  	|2419| **0.9263**  	|  **0.4555** 	| 0.7069  	| 0.4926| 0.9042|0.5307| 0.5540|
| Run_5|Custom|14|1:3|10K|  5394 	|  5770  	| 113396  	|2859| 0.9323  	|  0.4832 	| 0.6536  	| 0.4887| 0.9030|0.5268| 0.5556|
| Run_7|Clang|16|1:5.5|8K|  5999 	|  7216  	| 111950  	|2254| 0.9257  	|  0.4540 	| 0.7267  	| **0.4967**| **0.9115**|**0.5379**| **0.5589**|
| Run_8|Clang|14|1:5.5|7K|  **6034** 	|  7379  	| 111787  	|**2219**| 0.9247  	|  0.4499 	| **0.7311**  	| 0.4966| 0.9107|0.5367| 0.5570|

#### Model evaluation

* This is a table of results for all the implementation done for Draper VDISC dataset.
 
| Reference|TP|FP|TN|FN|Acc|Precision|Recall|PR-AUC|AUC|MCC|F1|
|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|---	|
|  Russel et.al (2018) - CNN+RF|  NA 	|  NA  	| NA  	|NA| NA  	|  NA 	| NA 	| 0.518| 0.904|0.536| 0.566|
|  Russel et.al (2018) - CNN	|  NA 	|  NA  	| NA  	|NA| NA  	|  NA 	| NA 	| 0.467| 0.897|0.509| 0.540|
|  Replication of Russell (2018) - CNN 4th model	|  5093 	|  8666  	| 110500  	|3160| 0.9071  	|  0.3701 	| 0.6171  	| 0.3665| 0.8830|0.4317| 0.4627|
|  ASTNN (CodeSensor-Complete)|  390 	|  3551  	| 113984  	|7806| 0.9097  	|  0.099 	| 0.0476  	| 0.0535| 0.4278|0.0246| 0.0643|
|  ASTNN (CodeSensor-Minimal)	|  1091 	|  14145  	| 93641  	|5267| 0.8299  	|  0.0717 	| 0.1716  	| 0.0520| 0.4760|0.0272| 0.1010|


#### Discussion
* This Transformer based approach manages to beat some of the metrics reported by Russell et. al. (2018)
* However, this model is in early stages. There a still room for improvements since a lot of hyper-parameter tuning can be done.
* It is also safe to note that, this could be potential path to explore as Transformer (2017) architecture has evolved greatly. For example, BERT (2018) and ALBERT (2019). These advancements in Transformer network allows them to achieve SOTA performance on benchmark datasets in NLP.
* What about source codes?
