# Thai_NC

## **Introduction**
- **Motivation**

  Noun compounds (NCs), sequences of nouns acting as a unit representing one concept, are very pervasive in text, especially noun compounds with two adjacent nouns. The meaning of NCs depends on the element or noun constituent and, in linguistic literature, the meaning is called ‘compositional’ if we can derive the meaning of such NNs from the meaning of its elements, on the other hands, the meaning is ‘lexicalized’ if the meaning of NNs cannot be derived from its elements. This means that those NCs with lexicalized readings are stored in mental lexicon of native speakers.

  The interpretation of NCs is to determine the implicit semantic relations between two adjacent nouns. The task of identifying such relations is called **‘noun compound interpretation (NCI)’** or **semantic relation classification** in NLP. For example, *olive oil*  is ‘oil that made OF olives’, but *baby oil* is ‘oil that made FOR baby’. Although both have the same oil as the head, determining the category of what entity they refer to, the interpretation is different because of implicit semantic relation between that head and their modifiers *olive* and *oil*, respectively. The meanings of these two noun compounds are compositional since we can derive the compound meaning from its constituents. However, the interpretation of *snake oil*, which means ‘deceptive marketing or healthcare scam’, is different because the meaning cannot be derived from either *snake* or *oil*, thus the meaning is lexicalized. 

  NCI task plays a key role in various NLP applications that require text understanding such as question and answering (Ahn et al., 2005), text entailment (Nakov, 2013), machine translation (Baldwin and Tanaka 2014). The effective NCI task will need to be able to generalize well to unseen data (the data that has not been in training sets). This kind of task, however, is one of the difficult tasks due to the lack of standardized annotation of semantic relations and require the world knowledge to understand the hidden relations. The semantics of compounds have previously been analyzed by linguists and different semantic relations have been proposed, for instance, Levi (1978) proposed 12 relations using verbs and prepositions to identify implicit relations such as tear gas is *GAS CAUSES TEAR*, olive oil is *OIL FROM OLIVE*. Later the semantic relations have been revised and reanalyzed for noun compounds in huge corpora (Tratz and Hovy, 2010) and standardized annotation dataset are proposed publicly in SEM-EVAL task for noun compound semantic relation classification (Butnariu et al., 2008). There is no agreement in terms of the granularity and the coverage of the dataset though.

- **Previous works**

  Previous works on semantic classification in English NCs is that: given a pre-defined set of semantic relations, the task is to classify the semantic relations in NC = W1W2 that hold between W1 and W2. The features used in the classification is the pre-trained static word embeddings (e.g., Word2Vec (Mikolov et al., 2013a, b) (Dima and Hinrichs, 2015) and contextualized word embeddings such as a transformer-based language model (Bidirectional Encoder Representations from Transformers: BERT) (Devlin et al., 2018) (Shwartz and Dagan, 2019). Among the pre-trained word embeddings, transformer-based (BERT) outperformed the state-of-the-art on this task.

- **Objective**

  This work presents the first Thai NCs with two noun constituents dataset with semantic relations and the supervised semantic relation classification with static word embedding (Word2Vec) (Devlin et al., 2018) using the Maximum Entropy model compared with transformer-based pre-trained language model (WangchanBERTa) (Lowphansirikul et al., 2021). The dataset consists of 1,810 Thai NCs collected from Thai National Corpus (TNC) and social media pages. 
  
  To verify the validity of the manual annotation, the inter-annotator agreement is calculated using Kappa’s Cohen *κ* statistics, in which each item of dataset is annotated by two annotators followed by a final decision from the third annotator when the mismatched annotation occurred. The annotation score results in .80 *κ* suggesting substantial agreement. This verified dataset with the semantic relation annotation will then be used as the gold label for the supervised classification.

  The result of the Maximum Entropy model with the static word embedding, however, yield a decent performance (F1 score of 0.42, accuracy of 0.55). Even worse happens with the transformer-based model with F1 score of 0.11, accuracy of 0.39. The details of experimental setups will be discussed in later subsections.


## **Classification model**

For semantic relations in Thai NCs, we need to predict semantic relations inside NCs. 

	Model 1: Maximum Entropy model
	- Static Word embedding: Word2Vec
	- Input: W1|W2  (each noun constituent inside NNs is tokenized with ‘|’)
	- Output: semantic relation 

	Model 2: Transformer-based model 
	- Contextualized Word embedding: WangchanBERTa
	- Input: C1,…,W1 W2,….,Cn (each noun compound is embedded in a context from C1 to Cn,)
	- Output: semantic relation 

The output of these two models are similar because we want to know whether a model understand the implicit relations inside NCs. The input, however, is different. In the MaxEnt model, the input is tokenized noun constituents. Thus, NCs are represented with a concatenation of its constituent embeddings. Whereas the input for transformer-based model, the NCs occurs in contexts which the model can compute word representations in a given context.  

## **Dataset**
- **Noun compound dataset**

    The dataset consists of 1,810 Thai NCs collected from Thai National Corpus (TNC) and social media pages. The head nouns in each NC have been check from the Royal Thai Dictionary. For the semantic annotation, the appropriate verbs and prepositions are used between two nouns (W1W2). After the analysis of total 1,810 NCs, we derive the total of 19 semantic relations, the label distribution and its description are shown below:



1. **LEXICALIZED**        - 467		: e.g.,  *ก้นกบ, กระเป๋ารถเมล์*		

	[The meaning of NC cannot be derived from both words]
2. **W1_MADE_FROM_W2**    - 269 : e.g.,		*กระโปรงผ้า, ขวดแก้ว*		

	[The first word is made from the second word]
3. **W1_OF_W2**           - 214 : e.g.,		*กล้องมือถือ, ก้านกล้วย*		

	[The second word is the possessor of the first word]

4. **ATTRIBUTE_OF_W1**    -	193 : e.g.,		*ห้องแถว, ลมพายุ*		

	[The first word is the head modified by the second word]

5. **W1_FOR_W2**          - 146 : e.g., 	*สระเด็ก, เทียนพรรษา*		

	[The first word is used for the second word]

6. **W1_CONTAIN_W2**      - 89 : e.g.,		*แก้วกาแฟ, ถุงแกง*		

	[The first word contains the second word]

7. **LOCATION_OF_W2**     - 82 : e.g.,		*หน้าแข้ง, สันเขา*		
	
	[The whole meaning represents the location of the second word]

8. **W1_FROM_W2**         - 64 : e.g.,		*กากกาแฟ, ควันเทียน*		

	[The first word derived from the second word]

9. **W1_ABOUT_W2**        - 63 : e.g.,		*ช่องกีฬา, ฮอร์โมนเพศ*		

	[The first word is about the second word]

10. **ATTRIBUTE_OF_W2**   - 62 : e.g.,		*แผ่นผนัง, พวงกุญแจ*		

	[The second word is the head modified by the first word]

11. **GROUP_OF_W2**       - 28 : e.g.,		*กองข้าว, เครือรัฐ*		

	[The plural forms of the second word]

12. **SOURCE_OF_W1**      - 28 : e.g.,		*เพลงไทย, ภาษาจีน*
	
	[The source of the first word comes from the second word]

13. **W1_IN_W2**          - 21 : e.g., 		*รากดิน, โกดังโรงงาน*		

	[The first word exists in the second word]

14. **OTHERS**            - 19 : e.g., 		*กลางเดือน, ต้นเสียง*		

	[If not applicable to any relations, define OTHERS]

15. **BOTH_W1_W2**        - 18 : e.g., 		*กากใย, พ่อแม่*		

	[The whole meaning comes from both words]

16. **W1_USE_W2**         - 14 : e.g., 		*รถม้า, เตาแก๊ส*		

	[The first word uses the second word]

17. **W1_HAVE_W2**        - 14 : e.g., 		*ควันพิษ, สำนักสงฆ์*		

	[The first word has the second word]

18. **LOCATION_OF_W1**     - 11 : e.g., 		*เขตกรุงเทพ, กระดุมหลัง*	

	[The whole meaning represents the location of the first word]

19. **W1_SELL_W2**         - 8 : e.g., 		*รถไอศกรีม, ร้านขนม*		

	[The first word sells the second word]


To verify the labeled semantic relation, two annotators do the annotation and if there is disagreement on any NCs, the third annotator judge what semantic relations should be. All annotators are native speakers. The first two are in linguistic major and the third one is Thai major. Kappa’s Cohen κ statistics is calculated for two annotators and get a good result with a score of .80, which substantial agreement.


## **Experiment Setup**
**Model 1: Maximum Entropy model**

	- Static Word embedding: Word2Vec (Pre-trained from TNC corpus)
	- Vector dimension: 100
	- Vocab size: 61,659
	- Activation function: ReLu

**Model 2: Transformer-based model** 

	- Contextualized Word embedding: WangchanBERTa (Pre-trained from Wikipedia)
	- Learning rate = 1e-5
	- Batch size = 128
	- Epoch = 5

## **Results**
- **The Maximum Entropy model**

    The result of the Maximum Entropy model with the static word embedding outperforms the transformer-based model with a decent performance (F1 score of 0.42, accuracy of 0.55 vs. F1 score of 0.11, accuracy of 0.39, respectively. 

    The reasons for a decent performance of the MaxEnt model are summarized below.

  (1) The highest F1 score tend to happens with a labels with many supports (e.g., F1 score of 0.74 for 51 supports with label ‘W1_MADE_FROM_W2’), this is maybe because the more data, the more pattern of relation that the model learns. The sparse support for some label gives rise to low accuracy.

  (2) The actual reason behind the high accuracy for some label perhaps come from the 'lexical memorization', that is some noun compounds happened frequently in the dataset. For example, the word ‘เชือก’ with other modifier nouns and be labeled with ‘W1_MADE_FROM_W2’, so whenever the model see this word, it memorized this lexicon and then remember the majority of labels leading to the wrong prediction (the model predicts ‘W1_MADE_FROM_W2’ for the word ‘เชือกฟาง’ instead of ‘ATTRIBUTE_OF_W1’. This is because the word ‘เชือก’ co-occurs a lot with other materials in this label e.g., ‘เชือกป่าน’, ‘เชือกปอ’). This is raised by Dima (2016), who proposed that rather than learning from the data, the model memorize the prototypical word from each relation.

  (3) The model maybe get confused from some annotation relation especially ‘W1_MADE_FROM_W2’ and ‘W1_from_W2’. The prediction for the word ‘นมถั่วเหลือง’ is ‘W1_FROM_W2’, but the gold is ‘W1_MADE_FROM_W2’. Although the agreement score shows a good result from annotators, the model does not seem to know the inherent meaning of each element.

- **The transformer-based model**

   The reasons for a very low performance of the transformer-based model, however, maybe due to the very long contexts that the NCs occurs as well as the model that used in the experiment is the 'sequence classification'. Thus, the model does not know which word (which NCs) to attend. The author will use the 'token classification' instead to mask the word boundary and will be reported later. Lastly, the dataset is not huge enough for model to learn. 


## **Conclusion**
This works represent the first semantic annotation dataset with 1,810 noun compounds in Thai and do the semantic relation classification with two different pre-trained word embedding (static ‘Word2Vec’ vs. contextualized ‘WangchanBERTa’) to compare which model is better for learning implicit semantic relations in Thai NCs. The preliminary result is unexpected because the static one outperformed the contextualized one. The technique and the experimental setup will be revised. 
