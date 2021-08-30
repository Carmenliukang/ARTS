# ARTS Week 35

> ![](https://github.com/Carmenliukang/ARTS/blob/master/image/35/1.jpg)
>> 什么也无法舍弃的人，什么也改变不了

***

## Algoithm

> [零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/)

### 概述

给定不同面额的硬币和一个总金额。写出函数来计算可以凑成总金额的硬币组合数。假设每一种面额的硬币有无限个。

示例 1:

输入: amount = 5, coins = [1, 2, 5]
输出: 4 解释: 有四种方式可以凑成总金额:
5=5 5=2+2+1 5=2+1+1+1 5=1+1+1+1+1 示例 2:

输入: amount = 3, coins = [2]
输出: 0 解释: 只用面额2的硬币不能凑成总金额3。 示例 3:

输入: amount = 10, coins = [10]
输出: 1

注意:

你可以假设：

0 <= amount (总金额) <= 5000 1 <= coin (硬币面额)<= 5000 硬币种类不超过 500 种 结果符合 32 位符号整数

### coding

```python
class Solution:
   def change(self, amount: int, coins: list[int]) -> int:
      dp = [0] * (amount + 1)
      dp[0] = 1

      for coin in coins:
         for x in range(coin, amount + 1):
            dp[x] += dp[x - coin]
      return dp[amount]
```

***

## Review

> [NLP tutorial for Text Classification in Python](https://vijayaiitk.medium.com/nlp-tutorial-for-text-classification-in-python-8f19cd17b49e)

### 概述

![](https://github.com/Carmenliukang/ARTS/blob/master/image/35/4.png)

**Text classification** is one of the important task in supervised machine learning (ML). It is a process of assigning
tags/categories to documents helping us to automatically & quickly structure and analyze text in a cost-effective
manner. It is one of the fundamental tasks in Natural Language Processing with broad applications such as
sentiment-analysis, spam-detection, topic-labeling, intent-detection etc.

代码相关的链接
[NLP-text-classification-model](https://github.com/vijayaiitk/NLP-text-classification-model)

#### Step 1: Importing Libraries

```python3
import pandas as pd
import numpy as np
# for text pre-processing
import re, string
import nltk
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from nltk.stem import SnowballStemmer
from nltk.corpus import wordnet
from nltk.stem import WordNetLemmatizer

nltk.download('punkt')
nltk.download('averaged_perceptron_tagger')
nltk.download('wordnet')
# for model-building
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.naive_bayes import MultinomialNB
from sklearn.metrics import classification_report, f1_score, accuracy_score, confusion_matrix
from sklearn.metrics import roc_curve, auc, roc_auc_score
# bag of words
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.feature_extraction.text import CountVectorizer
# for word embedding
import gensim
from gensim.models import Word2Vec

```

#### Step 2: Loading the data set & EDA

[Natural Language Processing with Disaster Tweets](https://www.kaggle.com/c/nlp-getting-started/overview)

Loading the data set in Kaggle Notebook:

```python
import pandas as pd

df_train = pd.read_csv('../input/nlp-getting-started/train.csv')
df_test = pd.read_csv('../input/nlp-getting-started/test.csv')

```

![](https://github.com/Carmenliukang/ARTS/blob/master/image/35/5.png)

#### Step 3: Text Pre-Processing

1. Simple text cleaning processes

   * Removing punctuations, special characters, URLs & hashtags
   * Removing leading, trailing & extra white spaces/tabs
   * Typos, slangs are corrected, abbreviations are written in their long forms

2. Stop-word removal
3. Stemming: Refers to the process of slicing the end or the beginning of words with the intention of removing affixes(
   prefix/suffix)
4. Lemmatization: It is the process of reducing the word to its base form

![](https://github.com/Carmenliukang/ARTS/blob/master/image/35/6.png)

```python

import re
import nltk
import string
from nltk.corpus import wordnet
from nltk.tokenize import word_tokenize
from nltk.stem import WordNetLemmatizer


# convert to lowercase, strip and remove punctuations
def preprocess(text):
   text = text.lower()
   text = text.strip()
   text = re.compile('<.*?>').sub('', text)
   text = re.compile('[%s]' % re.escape(string.punctuation)).sub(' ', text)
   text = re.sub('\s+', ' ', text)
   text = re.sub(r'\[[0-9]*\]', ' ', text)
   text = re.sub(r'[^\w\s]', '', str(text).lower().strip())
   text = re.sub(r'\d', ' ', text)
   text = re.sub(r'\s+', ' ', text)
   return text


# STOPWORD REMOVAL
def stopword(string):
   a = [i for i in string.split() if i not in stopwords.words('english')]
   return ' '.join(a)


# LEMMATIZATION
# Initialize the lemmatizer
wl = WordNetLemmatizer()


# This is a helper function to map NTLK position tags
def get_wordnet_pos(tag):
   if tag.startswith('J'):
      return wordnet.ADJ
   elif tag.startswith('V'):
      return wordnet.VERB
   elif tag.startswith('N'):
      return wordnet.NOUN
   elif tag.startswith('R'):
      return wordnet.ADV
   else:
      return wordnet.NOUN


# Tokenize the sentence
def lemmatizer(string):
   word_pos_tags = nltk.pos_tag(word_tokenize(string))  # Get position tags
   a = [wl.lemmatize(tag[0], get_wordnet_pos(tag[1])) for idx, tag in
        enumerate(word_pos_tags)]  # Map the position tag and lemmatize the word/token
   return " ".join(a)


```

#### Step 4: Extracting vectors from text (Vectorization)

Bag-of-Words(BoW) and Word Embedding (with Word2Vec) are two well-known methods for converting text data to numerical
data.

**Count vectors**: It builds a vocabulary from a corpus of documents and counts how many times the words appear in each
document

![](https://github.com/Carmenliukang/ARTS/blob/master/image/35/7.jpg)

**Term Frequency-Inverse Document Frequencies (tf-Idf)**

```python3
import nltk

# SPLITTING THE TRAINING DATASET INTO TRAIN AND TEST
X_train, X_test, y_train, y_test = train_test_split(df_train["clean_text"], df_train["target"], test_size=0.2,
                                                    shuffle=True)
# Word2Vec
# Word2Vec runs on tokenized sentencesdf_train
X_train_tok = [nltk.word_tokenize(i) for i in X_train]
X_test_tok = [nltk.word_tokenize(i) for i in X_test]
```

Bag-of-Words & Word2Vec

```python

# Tf-Idf
tfidf_vectorizer = TfidfVectorizer(use_idf=True)
X_train_vectors_tfidf = tfidf_vectorizer.fit_transform(X_train)
X_test_vectors_tfidf = tfidf_vectorizer.transform(X_test)


# building Word2Vec model
class MeanEmbeddingVectorizer(object):
   def __init__(self, word2vec):
      self.word2vec = word2vec
      # if a text is empty we should return a vector of zeros
      # with the same dimensionality as all the other vectors
      self.dim = len(next(iter(word2vec.values())))


def fit(self, X, y):
   return self


def transform(self, X):
   return np.array([
      np.mean([self.word2vec[w] for w in words if w in self.word2vec]
              or [np.zeros(self.dim)], axis=0)
      for words in X
   ])


w2v = dict(zip(model.wv.index2word, model.wv.syn0))
df['clean_text_tok'] = [nltk.word_tokenize(i) for i in df['clean_text']]
model = Word2Vec(df['clean_text_tok'], min_count=1)
modelw = MeanEmbeddingVectorizer(w2v)
# converting text to numerical data using Word2Vec
X_train_vectors_w2v = modelw.transform(X_train_tok)
X_val_vectors_w2v = modelw.transform(X_test_tok)
```

#### Step 5. Running ML algorithms

**Logistic Regression**: We will start with the most simplest one Logistic Regression. You can easily build a
LogisticRegression in scikit using below lines of code

```python
#FITTING THE CLASSIFICATION MODEL using Logistic Regression(tf-idf)
lr_tfidf = LogisticRegression(solver='liblinear', C=10, penalty='l2')
lr_tfidf.fit(X_train_vectors_tfidf, y_train)
#Predict y value for test dataset
y_predict = lr_tfidf.predict(X_test_vectors_tfidf)
y_prob = lr_tfidf.predict_proba(X_test_vectors_tfidf)[:, 1]
print(classification_report(y_test, y_predict))
print('Confusion Matrix:', confusion_matrix(y_test, y_predict))

fpr, tpr, thresholds = roc_curve(y_test, y_prob)
roc_auc = auc(fpr, tpr)
print('AUC:', roc_auc)

```

![](https://github.com/Carmenliukang/ARTS/blob/master/image/35/8.png)

**Naive Bayes**: It’s a probabilistic classifier that makes use of Bayes’ Theorem, a rule that uses probability to make
predictions based on prior knowledge of conditions that might be related

```python
#FITTING THE CLASSIFICATION MODEL using Naive Bayes(tf-idf)
nb_tfidf = MultinomialNB()
nb_tfidf.fit(X_train_vectors_tfidf, y_train)
#Predict y value for test dataset
y_predict = nb_tfidf.predict(X_test_vectors_tfidf)
y_prob = nb_tfidf.predict_proba(X_test_vectors_tfidf)[:, 1]
print(classification_report(y_test, y_predict))
print('Confusion Matrix:', confusion_matrix(y_test, y_predict))

fpr, tpr, thresholds = roc_curve(y_test, y_prob)
roc_auc = auc(fpr, tpr)
print('AUC:', roc_auc)

```

![](https://github.com/Carmenliukang/ARTS/blob/master/image/35/9.png)

***

## Tip

> [Redis-缓冲区](https://time.geekbang.org/column/article/291277)

### 概述

S/C 结构中，缓冲区是为了能够暂存 输入/输出 的命令，这些不能过大。

### 客户端输入和输出缓冲区

1. 输入缓冲区:会先把客户端发送过来的命令暂存起来，Redis 主线程再从输入缓冲区中读取命令，进行处理。当 Redis 主线程处理完数据后，会把结果写入到
2. 输出缓冲区:当 Redis 主线程处理完数据后，会把结果写入到输出缓冲区，再通过输出缓冲区返回给客户端

![](https://github.com/Carmenliukang/ARTS/blob/master/image/35/2.jpg)

#### 输入缓冲区溢出

出现的原因：

1. 写入了 bigkey，比如一下子写入了多个百万级别的集合类型数据；
2. 服务器端处理请求的速度过慢，例如，Redis 主线程出现了间歇性阻塞，无法及时处理正常发送的请求，导致客户端发送的请求在缓冲区越积越多。

查看方式

```shell

CLIENT LIST
id=5 addr=127.0.0.1:50487 fd=9 name= age=4 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=26 qbuf-free=32742 obl=0 oll=0 omem=0 events=r cmd=client
```

另一类是与输入缓冲区相关的三个参数：

* cmd，表示客户端最新执行的命令。这个例子中执行的是 CLIENT 命令
* qbuf，表示输入缓冲区已经使用的大小。这个例子中的 CLIENT 命令已使用了 26 字节大小的缓冲区。
* qbuf-free，表示输入缓冲区尚未使用的大小。这个例子中的 CLIENT 命令还可以使用 32742 字节的缓冲区。
* qbuf 和 qbuf-free 的总和就是，Redis 服务器端当前为已连接的这个客户端分配的缓冲区总大小。

这个例子中总共分配了 26 + 32742 = 32768 字节，也就是 32KB 的缓冲区。

redis server 默认设置maxmemory 4。如果超过，那么就会出发内存淘汰机制。

##### 解决问题

1. 缓冲区调大
2. 从数据命令的发送和处理速度入手。

方法1，暂时不可取，因为redis未开放出相关的参数，默认设置未1G.

#### 输出缓冲区溢出

输出缓冲包含两种:

1. 16KB 固定缓冲大小,OK,err
2. 动态增加缓冲空间，暂存大小可变的响应结果

出现这种情况的原因：

1. bigkey
2. MONITOR
3. 缓冲区大小设置不合理

##### MONITOR

命令会持续监测redis命令：不推荐生产环境
![](https://github.com/Carmenliukang/ARTS/blob/master/image/35/3.png)

##### 缓存区大小设置

我们可以通过 client-output-buffer-limit 配置项，来设置缓冲区的大小

1. 设置缓冲区大小的上限阈值；
2. 设置输出缓冲区持续写入数据的数量上限阈值，和持续写入数据的时间的上限阈值。

```shell

client-output-buffer-limit normal 0 0 0
```

normal 表示当前设置的是普通客户端，

* 第 1 个 0 设置的是缓冲区大小限制
* 第 2 个 0 和第 3 个 0 分别表示缓冲区持续写入量限制和持续写入时间限制。

使用不同方式进行同步分析

* 普通客户端请求，可以设置成0，0，0
* 如果是其他的模式，可以通过手动设定相关的缓冲区大小

***

## Share

> [链表题目总结](https://github.com/Carmenliukang/ARTS/blob/master/week35.md#share)

### 概述

链表题目整体框架：

```

# 将生成的新的 head 指针引用
# python 中的一切都是引用，所以这里会引用起固定的内存地址
result = head

while head:
	# 相关的函数处理，比如中间某一个节点的删除/多个树枝删除
	# 可以是多个指针，同时中间增加参数
	
	if head.val==num:
	    head.val = head.next.val
	    head.next = head.next.next
	    
  head = head.next

```

#### 快慢指针

```
slow = head
fast = head.next

while fast:
    fast = fast.next
    slow = slow.next

```