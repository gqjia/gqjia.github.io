---
title: 【NLP】【NLP with python】0 preface
date: 2018-11-04 20:21:31
---

[《Natural Language Processing with Python– Analyzing Text with the Natural Language Toolkit》](http://www.nltk.org/book/)  

---
   
python处理file.txt并输出以ing结尾的词：  
```python
for line in open("file.txt"):
    for word in line.split():
        if word.endswith('ing'):
            print(word)
```
