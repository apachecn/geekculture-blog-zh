# 使用 Keras & LSTM 进行情感分析

> 原文：<https://medium.com/geekculture/sentiment-analysis-using-rnn-keras-e545fbe000?source=collection_archive---------19----------------------->

## 使用 Keras、Python 和 LSTM 构建情感分析器

![](img/92b69bf3d70dbb7f31eeb9702754d16e.png)

Photo by [Austin Distel](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

[**情绪**](https://dictionary.cambridge.org/dictionary/english/sentiment) 是基于对某一情况的感觉或对某一事物的思考方式而产生的思想、观点或想法。可以是关于电影、食物、餐馆等等。

借助*用户情感*，我们倾向于推荐电影、美食等。

这里，我们将重点关注由 25000 条评论组成的 **IMDB 电影数据集**，这些评论在训练集中带有正面/负面情感标签，在测试集中也带有相同数量的标签。这个数据集的好处是它默认带有 Keras，你不需要从另一个网站下载。

让我们开始实现吧。

导入所有的**基础库**:

```
import numpy as np
import pandas as pd
import tensorflow as tf
import keras
```

**导入 IMDB 数据集**:

```
from keras.datasets import imdb(x_train, y_train),(x_test, y_test) = imdb.load_data(num_words=5000)
```

确认数据集大小为 25000:

```
print(x_train.shape)
print(x_test.shape)
print(y_train.shape)
print(y_test.shape)
```

让我们分析一下训练集:

```
x_train[0]
```

**输出**:

```
[1,
 14,
 22,
 16,
 43,
 530,
 973,
 458,
 4468,
 112,
 50,
 670,
 2,
 9,
 35,
 .
 .
 .
 178,
 32] 
```

这些是**单词索引**的向量表示。我们需要**填充**这个序列到最多 500 个单词。为此，Keras 为我们提供了 *pad_sequences* 方法:

```
from keras.preprocessing import sequencex_train = sequence.pad_sequences(x_train, 500)
x_test = sequence.pad_sequences(x_test, 500)
```

现在让我们想象一下它是如何改变我们的训练组合的:

```
x_train[10]
```

**输出**:

```
array([   0,    0,    0,    0,    0,    0,    0,    0,    0,    0,    0,
          0,    0,    0,    0,    0,    0,    0,    0,    0,    0,    0,
          0,    0,    0,    0,    0,    0,    0,    0,    0,    0,    0,
          0,    0,    0,    0,    0,    0,    0,    0,    0,    0,    0,
          0,    0,    0,    0,    0,    0,    1,  785,  189,  438,   47,
        110,  142,    7,    6,    2,  120,    4,  236,  378,    7,  153,
         19,   87,  108,  141,   17, 1004,    5,    2,  883,    2,   23,
          8,    4,  136,    2,    2,    4,    2,   43, 1076,   21, 1407,
        419,    5,    2,  120,   91,  682,  189, 2818,    5,    9, 1348,
         31,    7,    4,  118,  785,  189,  108,  126,   93,    2,   16,
        540,  324,   23,    6,  364,  352,   21,   14,    9,   93,   56,
         18,   11,  230,   53,  771,   74,   31,   34,    4, 2834,    7,
          4,   22,    5,   14,   11,  471,    9,    2,   34,    4,  321,
        487,    5,  116,   15,    2,    4,   22,    9,    6, 2286,    4,
        114, 2679,   23,  107,  293, 1008, 1172,    5,  328, 1236,    4,
       1375,  109,    9,    6,  132,  773,    2, 1412,    8, 1172,   18,
          2,   29,    9,  276,   11,    6, 2768,   19,  289,  409,    4,
          2, 2140,    2,  648, 1430,    2,    2,    5,   27, 3000, 1432,
          2,  103,    6,  346,  137,   11,    4, 2768,  295,   36,    2,
        725,    6, 3208,  273,   11,    4, 1513,   15, 1367,   35,  154,
          2,  103,    2,  173,    7,   12,   36,  515, 3547,   94, 2547,
       1722,    5, 3547,   36,  203,   30,  502,    8,  361,   12,    8,
        989,  143,    4, 1172, 3404,   10,   10,  328, 1236,    9,    6,
         55,  221, 2989,    5,  146,  165,  179,  770,   15,   50,  713,
         53,  108,  448,   23,   12,   17,  225,   38,   76, 4397,   18,
        183,    8,   81,   19,   12,   45, 1257,    8,  135,   15,    2,
        166,    4,  118,    7,   45,    2,   17,  466,   45,    2,    4,
         22,  115,  165,  764,    2,    5, 1030,    8, 2973,   73,  469,
        167, 2127,    2, 1568,    6,   87,  841,   18,    4,   22,    4,
        192,   15,   91,    7,   12,  304,  273, 1004,    4, 1375, 1172,
       2768,    2,   15,    4,   22,  764,   55,    2,    5,   14, 4233,
          2,    4, 1375,  326,    7,    4, 4760, 1786,    8,  361, 1236,
          8,  989,   46,    7,    4, 2768,   45,   55,  776,    8,   79,
        496,   98,   45,  400,  301,   15,    4, 1859,    9,    4,  155,
         15,   66,    2,   84,    5,   14,   22, 1534,   15,   17,    4,
        167,    2,   15,   75,   70,  115,   66,   30,  252,    7,  618,
         51,    9, 2161,    4, 3130,    5,   14, 1525,    8,    2,   15,
          2,  165,  127, 1921,    8,   30,  179, 2532,    4,   22,    9,
        906,   18,    6,  176,    7, 1007, 1005,    4, 1375,  114,    4,
        105,   26,   32,   55,  221,   11,   68,  205,   96,    5,    4,
        192,   15,    4,  274,  410,  220,  304,   23,   94,  205,  109,
          9,   55,   73,  224,  259, 3786,   15,    4,   22,  528, 1645,
         34,    4,  130,  528,   30,  685,  345,   17,    4,  277,  199,
        166,  281,    5, 1030,    8,   30,  179, 4442,  444,    2,    9,
          6,  371,   87,  189,   22,    5,   31,    7,    4,  118,    7,
          4, 2068,  545, 1178,  829], dtype=int32)
```

现在我们需要构建一个 word_to_id 字典，这样这些索引就可以转换成单词，以便进一步分析。在字典中，我们将为索引 0 提供填充令牌，为索引 1 提供开始令牌，为索引 2 提供 UNK 令牌。所以我们必须将默认索引移动 3 来调整这些标记。

```
word_to_id = imdb.get_word_index()
word_to_id = {k:(v+3) for k,v in word_to_id.items()}
word_to_id["<PAD>"] = 0
word_to_id["<START>"] = 1
word_to_id["<UNK>"] = 2
```

构建完 word_to_it 之后，我们需要对 word 进行标识:

```
id_to_word = {idx:word for word, idx in word_to_id.items()}
```

现在我们可以为 id_to_word 提供一个索引，它将输出与之相关的单词。

```
id_to_word[20]
```

**输出**:

```
'movie'
```

现在，我们甚至可以阅读我们训练集中的内容:

```
print(" ".join(id_to_word[id] for id in x_train[10]))
```

**输出**:

```
<PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <PAD> <START> french horror cinema has seen something of a <UNK> over the last couple of years with great films such as inside and <UNK> romance <UNK> on to the scene <UNK> <UNK> the <UNK> just slightly but stands head and <UNK> over most modern horror titles and is surely one of the best french horror films ever made <UNK> was obviously shot on a low budget but this is made up for in far more ways than one by the originality of the film and this in turn is <UNK> by the excellent writing and acting that <UNK> the film is a winner the plot focuses on two main ideas prison and black magic the central character is a man named <UNK> sent to prison for <UNK> he is put in a cell with three others the <UNK> insane <UNK> body building <UNK> <UNK> and his retarded boyfriend <UNK> after a short while in the cell together they <UNK> upon a hiding place in the wall that contains an old <UNK> after <UNK> part of it they soon realise its magical powers and realise they may be able to use it to break through the prison walls br br black magic is a very interesting topic and i'm actually quite surprised that there aren't more films based on it as there's so much scope for things to do with it it's fair to say that <UNK> makes the best of it's <UNK> as despite it's <UNK> the film never actually feels <UNK> and manages to flow well throughout director eric <UNK> provides a great atmosphere for the film the fact that most of it takes place inside the central prison cell <UNK> that the film feels very <UNK> and this immensely <UNK> the central idea of the prisoners wanting to use magic to break out of the cell it's very easy to get behind them it's often said that the unknown is the thing that really <UNK> people and this film proves that as the director <UNK> that we can never really be sure of exactly what is round the corner and this helps to <UNK> that <UNK> actually does manage to be quite frightening the film is memorable for a lot of reasons outside the central plot the characters are all very interesting in their own way and the fact that the book itself almost takes on its own character is very well done anyone worried that the film won't deliver by the end won't be disappointed either as the ending both makes sense and manages to be quite horrifying overall <UNK> is a truly great horror film and one of the best of the decade highly recommended viewing
```

您还可以尝试训练集中的其他索引来进一步分析用户评论。也试着分析一下 y_train 集合。你会看到 *2 标签*或者 **0** (负面评价)或者 **1** (正面评价)。

下一步将是**建立一个模型**，它可以输入训练集并输出*用户评论好坏的概率*。

1.  创建顺序模型的实例
2.  在这种情况下，添加具有最大 vocab 大小和输出尺寸的嵌入层。
3.  现在添加一层 LSTM。
4.  为了输出，我们必须添加一个具有一个节点的密集层。
5.  最后，我们必须用损失函数作为二进制交叉熵，优化器作为 adam，度量作为准确性来编译用于训练的模型。

**进口**:

```
from keras.layers import Embedding, LSTM, Dense, Dropout
from keras import Sequential
```

**模型搭建**:

```
embedding_vector_length = 32 
model = Sequential() 
model.add(Embedding(5000, embedding_vector_length, input_length=500)) 
model.add(LSTM(100)) 
model.add(Dense(1, activation='sigmoid')) 
model.compile(loss='binary_crossentropy',optimizer='adam', metrics=['accuracy']) model.summary()
```

**输出**:

```
Model: "sequential_1"
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
embedding_1 (Embedding)      (None, 500, 32)           160000    
_________________________________________________________________
lstm_1 (LSTM)                (None, 100)               53200     
_________________________________________________________________
dense_1 (Dense)              (None, 1)                 101       
=================================================================
Total params: 213,301
Trainable params: 213,301
Non-trainable params: 0
_________________________________________________________________
```

现在，通过提供训练集、标签、时期、批次等参数来训练模型。我们将使用 validation_data，并为其提供 x_test 和 y_test，以便在每个时期我们可以分析训练集和验证集的损失和准确性。

*批量 _ 大小，历元，LSTM 单位等。都是超参数，可以将***进一步调整为***。***

```
**model.fit(x_train, y_train, validation_data=(x_test, y_test), epochs=10, batch_size=64)**
```

****输出**:**

```
**Epoch 1/5
391/391 [==============================] - 297s 750ms/step - loss: 0.5608 - accuracy: 0.7023 - val_loss: 0.3261 - val_accuracy: 0.8634
Epoch 2/5
391/391 [==============================] - 262s 670ms/step - loss: 0.2781 - accuracy: 0.8933 - val_loss: 0.3209 - val_accuracy: 0.8713
Epoch 3/5
391/391 [==============================] - 320s 820ms/step - loss: 0.2390 - accuracy: 0.9074 - val_loss: 0.3154 - val_accuracy: 0.8704
Epoch 4/5
391/391 [==============================] - 370s 947ms/step - loss: 0.2252 - accuracy: 0.9115 - val_loss: 0.3198 - val_accuracy: 0.8698
.
.
.**
```

****在测试中评估我们的车型**:**

```
**model_eval = model.evaluate(x_test, y_test)**
```

****输出**:**

```
**782/782 [==============================] - 103s 131ms/step - loss: 0.4072 - accuracy: 0.8279**
```

**根据您在训练时选择的超参数，您的情况可能会有所不同。**

****保存模型**，以便下次可以直接加载，避免训练阶段。**

```
**model.save('imdb.h5')**
```

**现在让我们预测一些随机评论，看看我们的模型表现如何:**

```
**# for user prediction
def user_input_processing(review):
    vec = []
    for word in review.split(" "):
        if word[-1] == ".":
            word = word[:-1]
        vec.append(word_to_id[str.lower(word)])
    vec_padded = sequence.pad_sequences([vec], 500)
    print(review, model.predict(vec_padded))**
```

****一个好的复习例子**:**

```
**user_input_processing("One of the finest films made in recent years.")**
```

****输出**:**

```
**One of the finest films made in recent years. [[0.9403163]]**
```

**我们可以看到我们的模型如何给出 0.94 的**输出，从而接近 1。因此有了**高度肯定的评价**。****

****一个差评例子**:**

```
**user_input_processing("Predictable and bad. The acting was terrible and the story was common.")**
```

****输出**:**

```
**Predictable and bad. The acting was terrible and the story was common. [[0.20036736]]**
```

**我们可以看到我们的模型如何给出 0.2 的**输出，从而接近 0。因此出现了**高度负面的评论**。****

**你甚至可以从 IMDB 网站上获取一些用户评论，看看这个模型预测了什么。**

**如果你喜欢读这篇文章，**为它鼓掌。如果你有任何问题或建议，可以在评论区联系我。****