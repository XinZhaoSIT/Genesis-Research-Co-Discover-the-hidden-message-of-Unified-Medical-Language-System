# Genesis-Research-Co-Discover-the-hidden-message-of-Unified-Medical-Language-System

## Problem description

- During the pandemic outbreak, many patients are suffering pain and loss due to the low efficiency of developing the COVID vaccine

- In order to alleviate the suffering of patients, we aim to accelerate new drug discovery by applying artificial intelligence in the medical industry (e.g. node embedding and networks embedding)

- We are not only able to model disease relationship based on pairwise similarities, but also identify drug and diseases phenotypic relationships

- Our ultimate goal is to benefit drug innovation and development by lowering the R&D cost

## Objectives

1. Find out how the similarities are connected and under what specific context

2. Minimize translational gaps between animal experiments and clinical outcomes  in order to have more accurate results

## Dataset

We use Unified Medical Language System (UMLS) as our data sources, and we’ve been collecting entities and their relations from UMLS

Data source is licensed and you can find more information from this link: https://www.nlm.nih.gov/research/umls/index.html

We use Java to decode UMLS to edge list. You can find source code from this link: https://github.com/blpercha/umls-to-graph

There are many ontologies are currently supported and provide hierarchical relationships from MRHIER.RRF \
We choose to analyze National Cancer Institution (NCI) subgraph and expect to predict hidden relationships with our model. \
The NCI graph visualiztion is shown below:
![image](https://user-images.githubusercontent.com/73065775/177395527-aee81f17-f58f-421e-8bd1-b1f4cd1c868e.png)

- NCI Thesaurus provides definitions, synonyms, and other information on nearly 10,000 cancers and related diseases, 17,000 single agents and related substances, and other topics related to cancer and biomedical research
- We discovered 312,695 hierarchical relationships in NCI knowledge graph

## Node2vec Node Embedding
We use node2vec neural network to embedding NCI knowledge graph
![image](https://user-images.githubusercontent.com/73065775/177395982-9c77fcb7-a4e0-473b-a4a2-5e8041a972af.png)

Random walks parameters:
- p = 1
- q = 1
- dimensions = 128
- num_walks = 4
- walk_length = 10

Node2vec model parameters:

- window_size = 100
- epochs = 1
- workers = multiprocessing.cpu_count()

## Link Prediction

Explore unseen relations based on NCI knowledge graph by three steps:
- train classifiers with node embeddings;
- select the best performance classifier;
- evaluate the selected classifier on unseen data to validate its ability to generalize.

AUC score on test set: 
using L1 norm is 0.809

Visualization: The blue points are positive edges, which means two nodes are connected based on our model’s prediction, the red points are negative edges, which means the edges should not exist.
![image](https://user-images.githubusercontent.com/73065775/177396864-c5824181-72cb-4e15-9a68-bf51946c8259.png)

## Deep Graph CNN

We use CNN to recognize a protein whether they are enzymes or non-enzymes:

- We use the proteins dataset, a collection of graphs each with an attached categorical label
- Each graph represents a protein and graph labels represent whether they are enzymes or non-enzymes
- The dataset includes 1113 graphs with 39 nodes and 73 edges on average for each graph
- Graph nodes have 4 attributes, and each graph is labelled as belonging to 1 of 2 classes

![image](https://user-images.githubusercontent.com/73065775/177397219-cd4e7985-0161-4ff9-b6a4-ae7e704d66d5.png)

Model training and validation:
![image](https://user-images.githubusercontent.com/73065775/177397408-17ec05e0-2965-4a86-a4d2-ca97c7105161.png)

## Conclusion

- Link prediction model can predict unseen paths or edges for two random nodes, and DGCNN model can predict whether the protein is enzymes or not
- With DGCNN model as a hidden path for NCI knowledge, it will faster to find the connections between drug and cancer by implementing link prediction model
- By combining of link prediction model and DGCNN model, now we have made the prediction more accurate and cautious, so it is a worthwhile direction of research






