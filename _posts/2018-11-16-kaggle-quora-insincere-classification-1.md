---
title: 【kaggle】【quora insincere questions classification】1 quora insincere questions classification
date: 2018-11-16 17:08:31
---

### <b>General Description
In this competition you will be predicting whether a question asked on Quora is sincere or not.  

An insincere question is defined as a question intended to make a statement rather than look for helpful answers. Some characteristics that can signify that a question is insincere:  

* Has a non-neutral tone 具有非中性的音调   
  * Has an exaggerated tone to underscore a point about a group of people 有一个夸张的语气来强调一个关于一群人的观点
  * Is rhetorical and meant to imply a statement about a group of people 是口头上的, 意思是暗示一个关于一群人的说法
* Is disparaging or inflammatory 是诋毁性的还是煽动性的
  * Suggests a discriminatory idea against a protected class of people, or seeks confirmation of a stereotype 暗示对受保护的人群的歧视性观念, 或寻求对陈规定型观念的确认
  * Makes disparaging attacks/insults against a specific person or group of people 对某一特定的人或一群人进行诋毁性的攻击
  * Based on an outlandish premise about a group of people 基于一个关于一群人的奇怪前提
  * Disparages against a characteristic that is not fixable and not measurable 与不可修复和不可衡量的特征的差异
* Isn't grounded in reality 没有根植于现实
  * Based on false information, or contains absurd assumptions 基于虚假信息, 或包含荒谬的假设
* Uses sexual content (incest, bestiality, pedophilia) for shock value, and not to seek genuine answers 使用性内容 (乱伦, 兽交、恋童癖) 的震撼价值, 而不是寻求真正的答案  

The training data includes the question that was asked, and whether it was identified as insincere (target = 1). The ground-truth labels contain some amount of noise: they are not guaranteed to be perfect.  

Note that the distribution of questions in the dataset should not be taken to be representative of the distribution of questions asked on Quora. This is, in part, because of the combination of sampling procedures and sanitization measures that have been applied to the final dataset.  

### File descriptions  
* train.csv - the training set  
* test.csv - the test set  
* sample_submission.csv - A sample submission in the correct format  
* enbeddings/ - (see below)  

### Data fields
* qid - unique question identifier
* question_text - Quora question text
* target - a question labeled "insincere" has a value of 1, otherwise 0  

This is a Kernels-only competition. The files in this Data section are downloadable for reference in Stage 1. Stage 2 files will only be available in Kernels and not available for download.  

### What will be available in the 2nd stage of the competition?  

In the second stage of the competition, we will re-run your selected Kernels. The following files will be swapped with new data:  
* test.csv - This will be swapped with the complete public and private test dataset. This file will have ~56k rows in stage 1 and ~376k rows in stage 2. The public leaderboard data remains the same for both versions. The file name will be the same (both test.csv) to ensure that your code will run.
* sample_submission.csv - similar to test.csv, this will be changed from ~56k in stage 1 to ~376k rows in stage 2 . The file name will remain the same.  

### Embeddings  
External data sources are not allowed for this competition. We are, though, providing a number of word embeddings along with the dataset that can be used in the models. These are as follows:  
* GoogleNews-vectors-negative300 - https://code.google.com/archive/p/word2vec/
* glove.840B.300d - https://nlp.stanford.edu/projects/glove/
* paragram_300_sl999 - https://cogcomp.org/page/resource_view/106
* wiki-news-300d-1M - https://fasttext.cc/docs/en/english-vectors.html
