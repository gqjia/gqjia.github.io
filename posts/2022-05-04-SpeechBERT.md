# SpeechBERT: An Audio-and-text Jointly Learned Language Model for End-to-end Spoken Question Answering

这篇是2019年的论文，这是论文的第四版，先前版本的名字是 [**SpeechBERT: Cross-Modal Pre-trained Language Model for End-to-end Spoken Question Answering.**](https://readpaper.com/paper/2981458636)

## 论文概述

该论文主要针对端到端的SQA（end-to-end Spoken Question Answering）任务。先前的研究证明，端到端的模型相比先使用ASR再进行TQA（Text Question Answering）的方法而言，能够避免ASR模型中存在的错误识别，直接从音频数据中提取信息。为此作者使用语音和文本数据训练了一个 SpeechBERT 模型，该模型不仅可以完成端到端的SQA任务，还可以用于其他语音理解任务（spoken language understanding task）。

![image-20220504102032850](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205041020911.png)

在先前语音方向的研究中，Word2vec 首次尝试将已知边界的音频单词转换为只携带语音信息、完全没有语义的嵌入。Speech2Vec 仿照 Word2vec 中的 Skip-gram 和 CBOW 的训练过程来提取语义特征。









## 模型结构

SpeechBERT模型在训练时，首先需要使用文本数据训练一个BERT模型，然后使用语音语义的联合表征（Initial Phonetic-Semantic Joint Embedding）在下游任务上进行微调。

![image-20220504105007343](https://raw.githubusercontent.com/Moriarty12138/PictureBed/main/img/202205041050401.png)

















## 论文结论

音频信号以语音的形式存在，同时携带语义信息。通常的做法是将音频信号转为文本，因此也会将转换过程中的错误传递给下游任务。Speech可以绕过ASR任务中产生的误差，直接将音频信息应用于下游任务。这个研究方向对于语音处理的任务是有用的，在未来仍需继续深入研究。

