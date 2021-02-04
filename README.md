# awesome-KBSQA

In general, this repository focuses on IR-based knowledge base simple question answering (KBSQA) techniques.

## datasets

### WebQuestions

https://github.com/brmson/dataset-factoid-webquestions or
https://nlp.stanford.edu/software/sempre/

It consists of 5,810 human-annotated questions, matched with answers corresponding to entities of Freebase. (no triple has to be returned, only a single entity). It was first used by Bordes et al. (2014b).

### SimpleQuestions

https://git.uwaterloo.ca/jimmylin/BuboQA-data/raw/master/SimpleQuestions_v2.tgz or
https://research.fb.com/downloads/babi/

The SimpleQuestions dataset consists of a total of 108,442 questions written in natural language by human English-speaking annotators each paired with a corresponding fact, formatted as (subject, relationship, object), that provides the answer but also a complete explanation. Facts have been extracted from the Knowledge Base Freebase. The authors randomly shuffle these questions and use 70% of them (75910) as training set, 10% as validation set (10845), and the remaining 20% as test set.

The annotators were explicitly instructed to phrase the question differently as much as possible.

#### FB2M and FB5M subsets

`/SimpleQuestions_v2/freebase-subsets/`

FB2M contains about 2M entities and 5k relationships. FB5M, is much larger with about 5M entities and more than 7.5k relationships. They were first used by Bordes et al. (2014a).

### 30MQA Corpus (30M Factoid Question-Answer Corpus)

https://academictorrents.com/details/973fb709bdb9db6066213bbc5529482a190098ce

### WikiQuestions

https://ad-publications.cs.uni-freiburg.de/student-projects/qa-completion/

A question dataset containing 4,390,597 questions and corresponding answer entities that are generated by rephrasing Wikipedia sentences as questions. The rough pipeline of the question generation (QG) system is as follows: A Wikipedia dump with Freebase entity mentions is preprocessed by annotating entities with their types. The preprocessed Wikipedia dump is then parsed using a dependency parser. For each sentence, entities that fulfill certain grammatical criteria are selected as answer entities. A fitting WH-word is selected for an answer entity and various transformations are performed over the sentence to rephrase it as a question. Finally, the generated questions are filtered to avoid ungrammatical or otherwise unreasonable questions.

## baselines

### Bordes et al. (2014b), WSEQA (weakly supervised embedding models for QA)

> Antoine Bordes, Jason Weston, and Nicolas Usunier. "Open question answering with weakly supervised embedding models." ECML-PKDD, 2014.


- [PDF](https://link.springer.com/content/pdf/10.1007/978-3-662-44848-9_11.pdf)
- CODE
- METHODOLOGY: `Both the question and the KB triple are embedded as a $k$-dimensional vector and compared based on inner product. In particular, each question token and each KB entity/predicate is learned as $k$-dimensional and the embeddings of questions and KB triples are summed based on their presences.`
- REMARKS:
  - questions can be regarded as a bag of words.
  - large question-answer datasets are generated by templates and their paraphrase to more types of questions are preserved by a multitask learning process over PARALEX dataset.

### Bordes et al. (2014a), SGEQA (subgraph embeddings for QA)

> Antoine Bordes, Sumit Chopra, and Jason Weston. "Question answering with subgraph embeddings." arXiv preprint arXiv:1406.3676 (2014).

- [PDF](https://arxiv.org/pdf/1406.3676)
- CODE
- METHODOLOGY: `It extends the question token and KB entity/predicate embedding in Bordes et al. (2014a). However, it further embeds answer as path plus all connected entities to the answer (i.e., a subgraph), so as to answer more sophisticated questions that answers may not be directly connected to the question entities.`
- REMARKS:
  - how subgraph embedding is constructed: 1) each single entity is embedded, 2) the 1- and 2-hops paths are embedded, consisting of embeddings of start and end entities, and embeddings of relations in between; 3) the entire subgraph of entities connected to the candidate answer entity are all embedded, each is being represented as a entity embedding plus a relation type embedding.
  - its hypothesis is that including more information about the answer in its representation will lead to improved result.

### Bordes et al. (2015), MemNN (memory networks)

> Antoine Bordes, Nicolas Usunier, Sumit Chopra, and Jason Weston. "Large-scale simple question answering with memory networks." arXiv preprint arXiv:1506.02075 (2015).


- [PDF](https://arxiv.org/pdf/1506.02075)
- [CODE for MemNN](https://github.com/facebook/MemNN)
- METHODOLOGY: `It studies the simple question answering in a large-scale KG setting. First, it finds candidate facts by matching $n$-grams of words of questions to aliases of freebase entities and keeping all facts having one of the matching entities. Second, the scoring is performed using embedding model learned from memory networks that stores historical facts for supervision.`

### Golub and He (2016), CQAA (Character-level question answering with attention)


> David Golub, and Xiaodong He. "Character-level question answering with attention." EMNLP 2016.


- [PDF](https://arxiv.org/pdf/1604.00727)
- [CODE](https://github.com/davidgolub/SimpleQA)
- METHODOLOGY: `Questions are embedded as one-hot encodings of characters in the question based on LSTM encoder. each KG entity/predicate is embedded as a single embedding based on a character-level CNN-based encoder. An LSTM-based decoder with attention is integrated with a relevant function to find 1) head entity and 2) predicate to find candidate facts in KG. The probabilities of candidates are calculated based on cosine similarity between question and candidates. Candidate entities and predicates are selected as similar to Bordes et al. (2015), i.e., by substring matching.`

### Dai et al. (2016), CFO (Conditional focused)

> Zihang Dai, Lei Li, and Wei Xu. "CFO: Conditional focused neural question answering with large-scale knowledge bases." ACL 2016.

- [PDF](https://arxiv.org/pdf/1606.01994)
- [CODE](https://github.com/zihangdai/cfo)
- METHODOLOGY: `It first zooms in a question to find more probable candidate subject mentions by n-gram pruning, and infers the final relation and subject with a unified conditional probabilistic framework. Once the relation and subject have been found in KG, a KG query can be used to retrieve the answers for the question. To be specific, a two-layer BiGRU is used to map the tokens of a question to the embedding of the mentioned relation; a similar idea is applied to map the question to the embedding of the mentioned subject and the difference is that an additional linear component of subject-relation score is combined. Embedding of relation or subject can be either pretrained from TransE or embedded based on type and structure information. Considering the large amount of candidates, a focused pruning technique is designed such that the focused mentions of the question will be identified by a neural network and used in n-gram matching. During the inference, only the most probable n-gram predicted by the model will be retained, and then used as the subject mention to generate the candidate pool.`

### Yin et al. (2016), ACNN (Attentive Convolutional Neural Network)

> Wenpeng Yin, Mo Yu, Bing Xiang, Bowen Zhou, and Hinrich Schütze. "Simple question answering by attentive convolutional neural network." COLING 2016.

- [PDF](https://arxiv.org/pdf/1606.03391)
- [CODE1](https://github.com/yinwenpeng/KBQA_IBM)
- [CODE2](https://github.com/yinwenpeng/KBQA_IBM_New)
- METHODOLOGY: `It matches the subject entity in a fact candidate with the entity mention in the question by a character-level convolutional neural network, and matches the predicate in that fact with the question by a word-level CNN enhanced with attentive maxpooling.`

### Lukovnikov et al. (2017), WCE (word- and character- level encoder)

> Denis Lukovnikov, Asja Fischer, and Jens Lehmann. "Neural network-based question answering over knowledge graphs on word and character level." WWW 2017.

- [PDF](https://dl.acm.org/doi/pdf/10.1145/3038912.3052675?casa_token=iLERoPpVmdgAAAAA:X0Wha2ssSGvCh2zWbm1glDTeBtzA9GlZXB7BcKsG7ZZEPj0Fvm18_zYyLoI1v5GGGSNU6uBN8SNg)
- [CODE](https://github.com/WDAqua/teafacto)
- METHODOLOGY: `It learns to rank subject-predicate pairs to retrieve relevant facts. In particular, questions are encoded as vectors by GRU, whereas subject and predicate are encoded by both word- and character-level GRU. Each pair of candidate subject-predicate pair is compared to the question in terms of embeddings based on Cosine similarity.`

### Mohammed et al. (2018), BuboQA

> Salman Mohammed, Peng Shi, and Jimmy Lin. "Strong baselines for simple question answering over knowledge graphs with and without neural networks." NAACL 2018.

- [PDF](https://arxiv.org/pdf/1712.01969)
- [CODE](https://github.com/castorini/BuboQA)
- METHODOLOGY: `It consists of entity detection, entity linking, relation prediction, and evidence combination. In particular, the entity detection is first performed to identify the entity being queried through bi-LSTM. Subsequently, identified entity name is linking to KG entities by $n$-gram fuzzy matching. Then, relation is found through bi-GRU as a classification task over all possible relations. At last, all possible entity-relation pairs are evaluated based on the product of their component scores.`
- REMARKS:
  - several different baselines are proposed, and BiLSTM-BiGRU achieves the best results over simpleQuestions dataset.
  - it can actually be regarded as a non-embedding based method, however, it handles the simple QA problem in an information retrieval paradigm, as others.

### Hao et al. (2018), PRSQA (Pattern-revising Simple Question Answering)

> Yanchao Hao, Hao Liu, Shizhu He, Kang Liu, and Jun Zhao. "Pattern-revising enhanced simple question answering over knowledge bases." COLING 2018.

- [PDF](https://www.aclweb.org/anthology/C18-1277.pdf)
- CODE
- METHODOLOGY: `It conducts pattern extraction and entity linking first to extract possible entity mention and question pattern, so as to reduce the candidates resulted from an $n$-gram matching. Then, pattern-revising over all candidate patterns are performed to correct the wrong pattern and get the right head entity mention. Then, a join fact search is performed that integrates the cosine similarities between the LSTM encodings of entity and question as well as those of entity+relation and question. The LSTM encodings are generated in both character and word levels.`

### Gupta et al. (2018), RRSQA (Retrieve and Re-rank Simple Question Answering)

> Vishal Gupta, Manoj Chinnakotla, and Manish Shrivastava. "Retrieve and re-rank: A simple and effective IR approach to simple question answering over knowledge graphs." EMNLP-FEVER 2018.

- [PDF](https://www.aclweb.org/anthology/W18-5504.pdf)
- CODE
- METHODOLOGY: `A two phase approach consisting of candidate generation and candidate re-ranking to answer questions. In particular, Solr, as an inverted index, is used to search top-$k$ relevant candidates given a question as the input query. BM25 serves as the scoring metric. Then, candidate re-ranking is based on a Triplet-Siamese-Hybrid CNN, which combines the latent representations of question, tuple, and question+tuple. The siamese network is trained by maximizing the similarity difference between a positive tuple and a negative tuple.`

### Petrochuk et al. (2018), UPSQA (upperbound simple question answering)

> Michael Petrochuk, and Luke Zettlemoyer. "Simplequestions nearly solved: A new upperbound and baseline approach." EMNLP 2018.

- [PDF](https://arxiv.org/pdf/1804.08798)
- [CODE](https://github.com/PetrochukM/Simple-QA-EMNLP-2018)
- METHODOLOGY: `The top-$k$ head entity recognition is performed first such that distribution of head entities over questions are modeled by a biLSTM and a linear-chain CRF. Then, the relation classification is conducted over the given question and the candidate head entity by a bi-LSTM.`
- REMARKS:
  - For entity candidate selection, all hyperparameters are hand tuned and then a limited set are further tuned with grid search to increase validation accuracy. GloVe embeddings and Adam were used for bi-LSTM model, and CRF is used to decode the K candidates with Viterbi algorithm.
  - For relation classification, all hyperparameters of LSTM are hand tuned and then a limited set are further tuned with Hyperban.

### Huang et al. (2019), KGEQA (Knowledge Graph Embedding based Question Answering)

> Xiao Huang, Jingyuan Zhang, Dingcheng Li, and Ping Li. "Knowledge graph embedding based question answering." WSDM 2019.

- [PDF](https://dl.acm.org/doi/pdf/10.1145/3289600.3290956?casa_token=_jdAPjXRy5MAAAAA:zjFI6xu8CC6rpBIRREu3aQCfpIxE4fmWVQ1Ru0mwzcuav2tjiRJyLLn91p0zjZ_THpXQB7sxMW8A)
- [CODE](https://github.com/xhuang31/KEQA_WSDM19)
- METHODOLOGY: `All KG entities and predicates are embeddings with TransE or TransR. Next, bi-LSTM with attention is applied to generate the corresponding KG embeddings of mentioned entity and predicate. Also, position of head entity mention is identified by a bi-LSTM and used to find candidates based on fuzzy matching. A joint similarity function is designed to integrate the similarities between entity/predicate embeddings and matched texts.`

### Zhao et al. (2019), SRJS (subgraph ranking and joint-scoring)

> Wenbo Zhao, Tagyoung Chung, Anuj Goyal,andAngeliki Metallinou. "Simple question answering with subgraph ranking and joint-scoring." NAACL 2019.

- [PDF](https://arxiv.org/pdf/1904.04049)
- CODE
- METHODOLOGY: `It proposes a subgraph ranking and joint-scoring approach to SQA. The ranking method combines literal and semantic scores of KG subgraph to deal with inexact match. The joint-scoring model couples the dependency of subject matching and relation matching with well-order loss.`

### Lukovnikov et al. (2019), BERTQA

> Denis Lukovnikov, Asja Fischer, and Jens Lehmann. "Pretrained transformers for simple question answering over knowledge graphs." ISWC 2019.

- [PDF](https://arxiv.org/pdf/2001.11985)
- [CODE](https://github.com/lukovnikov/semparse/tree/master/semparse/simplequestions)
- METHODOLOGY: `It consists of 1) entity span detection, 2) relation classification, and 3) heuristic-based post-processing for final facts. In steps 1) and 2), BERT was used to predict the entity mention and relation in an integrated way. Top entities are found through fuzzy string matching and the relation with highest probability is combined with its entity to form a candidate fact. For each candidate fact, only one relation is considered.`

### Sharath et al. (2020), LMEQA (Language Model Embedding based Question Answering)

> Japa Sai Sharath, and Rekabdar Banafsheh. "Question answering over knowledge base using language model embeddings." IJCNN 2020.

- [PDF](https://arxiv.org/pdf/2010.08883)
- CODE
- METHODOLOGY: `It first uses Bert base uncased for the initial experiments. It further fine-tunes these embeddings with a two-way attention mechanism from the knowledge base to the asked question and from the asked question to the knowledge base answer aspects. The approach is based on a CNN architecture with a Multi-Head Attention mechanism to represent the asked question. Candidate generation is conducted based on Freebase API in a way of fuzzy matching.`

## Other Resources

### Relevant Papers

#### Survey Papers

- [Core Techniques of Question Answering Systems over Knowledge Bases: a Survey](https://hal.archives-ouvertes.fr/hal-01637143/document)
- [Introduction to Neural Network based Approaches for Question Answering over Knowledge Graphs](https://arxiv.org/pdf/1907.09361.pdf)
- A Survey of Question Answering over Knowledge Base
- Deep Learning in Question Answering

#### NLP Techniques

- [Comparative Study of CNN and RNN for Natural Language Processing](https://arxiv.org/abs/1702.01923)

### GitHub Repository

- https://github.com/PetrochukM/Simple-QA-EMNLP-2018/
- https://github.com/simba0626/Question-Answering, some tutorials and slides about KBQA.
- https://github.com/BshoterJ/awesome-kgqa, some papers, projects, and resources of KBQA.
