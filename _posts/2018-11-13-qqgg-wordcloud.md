---
title: 【奇奇怪怪】wordcloud
date: 2018-11-13 17:36:31
---
点了奇奇怪怪的技能。  

---
```python
from wordcloud import WordCloud
import numpy as np
import pandas as pd
df_train = pd.read_csv(r'dataset\ train.csv')
train_qs = pd.Series(df_train['question_text'].tolist()).astype(str)
dist_train = df_train.question_text.apply(lambda x: len(x.split(' ')))
cloud = WordCloud(width=1440, height=1080).generate(" ".join(train_qs.astype(str)))
plt.figure(figsize=(20, 15))
plt.imshow(cloud)
plt.axis('off')
```
