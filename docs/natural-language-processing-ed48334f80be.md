# tensorflow2.0 自然语言处理概述

> 原文：<https://medium.com/geekculture/natural-language-processing-ed48334f80be?source=collection_archive---------44----------------------->

![](img/262dd71f2f70a52c733741b4bc65c0b9.png)

自然语言处理(简称 NLP)是一门处理自然(人类)语言和计算机语言之间交流的计算学科。NLP 的一个常见例子是拼写检查或自动完成。本质上，NLP 是关注计算机如何理解和/或处理自然/人类语言的领域。

# 递归神经网络

在本教程中，我们将介绍一种新的神经网络，它能够更好地处理序列数据，如文本或字符，称为**递归神经网络**(简称 RNN)。

我们将学习如何使用当前的神经网络来完成以下任务:

*   情感分析
*   字符生成

RNN 很复杂，有许多不同的形式，所以在本教程中，我们将重点介绍它们是如何工作的，以及它们最适合解决什么样的问题。

# 序列数据

在前一篇博客中，我们关注的是可以表示为一个静态数据点的数据，其中时间或步骤的概念是不相关的。以我们的图像数据为例，它只是一个形状张量(宽度、高度、通道)。这些数据不会改变也不关心时间的概念。

在这个博客中，我们将看看文本序列，并学习如何以一种有意义的方式对它们进行编码。与图像不同，序列数据，如长链文本、天气模式、视频以及任何与步骤或时间相关的东西，都需要以特殊的方式进行处理。

但是我说的序列是什么意思，为什么文本数据是序列？这是个好问题。由于文本数据包含许多单词，这些单词以非常特定和有意义的顺序出现，我们需要能够跟踪每个单词以及它何时出现在数据中。简单地将一整段文字编码成一个数据点并不能给我们一个非常有意义的数据图像，而且很难做任何事情。这就是为什么我们将文本视为一个序列，一次处理一个单词。我们将跟踪这些单词出现的位置，并使用这些信息来理解文本的意思。

# 编码文本

正如我们所知，机器学习模型和神经网络不会将原始文本数据作为输入。这意味着我们必须以某种方式将文本数据编码成模型能够理解的数值。有许多不同的方法可以做到这一点，我们将在下面看几个例子。

在我们进入不同的编码/预处理方法之前，让我们通过查看下面两个电影评论来理解我们可以从文本数据中获得的信息。

`I thought the movie was going to be bad, but it was actually amazing!`

`I thought the movie was going to be amazing, but it was actually bad!`

虽然这两个词非常相似，但我们知道它们有非常不同的含义。这是因为单词的排序是文本数据的一个非常重要的属性。

现在，在我们考虑一些不同的文本数据编码方式时，请记住这一点。

# 一袋单词

对我们的数据进行编码的第一个也是最简单的方法是使用一种叫做**的单词包**。这是一个非常简单的技术，句子中的每个单词都用一个整数编码，并被放入一个集合中，该集合不保持单词的顺序，但会跟踪频率。看看下面的 python 函数，它将一串文本编码成单词包。

```
vocab = {} # maps word to integer representing itword_encoding = 1def bag_of_words(text):global word_encodingwords = text.lower().split(“ “) # create a list of all of the words in the text, well assume there is no grammar in our text for this examplebag = {} # stores all of the encodings and their frequencyfor word in words:if word in vocab:encoding = vocab[word] # get encoding from vocabelse:vocab[word] = word_encodingencoding = word_encodingword_encoding += 1if encoding in bag:bag[encoding] += 1else:bag[encoding] = 1return bagtext = “this is a test to see if this test will work is is test a a”bag = bag_of_words(text)print(bag)print(vocab)
```

这不是我们在实践中的做法，但我希望它能让你了解单词袋是如何工作的。请注意，我们已经忘记了单词出现的顺序。事实上，让我们来看看这种编码对于我们上面展示的两个句子是如何工作的。

```
positive_review = “I thought the movie was going to be bad but it was actually amazing”negative_review = “I thought the movie was going to be amazing but it was actually bad”pos_bag = bag_of_words(positive_review)neg_bag = bag_of_words(negative_review)print(“Positive:”, pos_bag)print(“Negative:”, neg_bag)
```

我们可以看到，即使这些句子有着非常不同的含义，它们的编码方式却完全相同。显然，这是行不通的。再来看看其他一些方法。

# 整数编码

我们要看的下一个技术叫做**整数编码**。这包括将句子中的每个单词或字符表示为唯一的整数，并保持这些单词的顺序。这有望解决我们之前看到的单词顺序混乱的问题

```
word_encoding = 1def one_hot_encoding(text):global word_encodingwords = text.lower().split(“ “)encoding = []for word in words:if word in vocab:code = vocab[word]encoding.append(code)else:vocab[word] = word_encodingencoding.append(word_encoding)word_encoding += 1return encodingtext = “this is a test to see if this test will work is is test a a”encoding = one_hot_encoding(text)print(encoding)print(vocab)And now let’s have a look at one hot encoding on our movie reviews.positive_review = “I thought the movie was going to be bad but it was actually amazing”negative_review = “I thought the movie was going to be amazing but it was actually bad”pos_encode = one_hot_encoding(positive_review)neg_encode = one_hot_encoding(negative_review)print(“Positive:”, pos_encode)print(“Negative:”, neg_encode)
```

更好的是，现在我们跟踪单词的顺序，我们可以知道每个单词出现在哪里。但是这仍然有一些问题。理想情况下，当我们对单词进行编码时，我们希望相似的单词有相似的标签，而不同的单词有非常不同的标签。例如，快乐和喜悦这两个词应该有非常相似的标签，这样我们就可以确定它们是相似的。而像“可怕的”和“惊人的”这样的词应该有非常不同的标签。我们上面看到的方法不能为我们做这样的事情。这可能意味着模型将很难确定两个单词是否相似，这可能会导致一些非常严重的性能影响。

# 单词嵌入

幸运的是，有第三种方法要好得多，**单词嵌入**。这种方法保持单词的顺序不变，并且用非常相似的标签对相似的单词进行编码。它不仅试图对单词的频率和顺序进行编码，还试图对这些单词在句子中的含义进行编码。它将每个单词编码成一个密集的向量，代表它在句子中的上下文。

与之前的技术不同，单词嵌入是通过查看许多不同的训练示例来学习的。你可以将所谓的*嵌入层*添加到你的模型的开始，当你的模型训练你的嵌入层时，它将学习单词的正确嵌入。您也可以使用预训练的嵌入层。

这是我们将在示例中使用的技术，它的实现将在后面展示。

# 递归神经网络(RNN 氏)

既然我们已经了解了一些如何对文本进行编码的知识，那么是时候深入研究递归神经网络了。到目前为止，我们一直在使用一种叫做**前馈**神经网络的东西。这仅仅意味着我们所有的数据通过网络从左到右向前(一次全部)传送。这对于我们之前考虑的问题来说很好，但是对于处理文本来说就不太好了。毕竟，即使是我们(人类)也不会一次性处理文本。我们从左到右一个单词一个单词地读，并跟踪句子的当前意思，这样我们就能理解下一个单词的意思。这正是递归神经网络的设计目的。当我们说递归神经网络时，我们真正的意思是一个包含循环的网络。RNN 将一次处理一个单词，同时保持已经看到的内容的内部记忆。这将允许它根据单词在句子中的顺序来区别对待单词，并慢慢地建立对整个输入的理解，一次一个单词。

这就是为什么我们将文本数据视为一个序列！这样我们就可以一个单词一个单词地传递给 RNN。

让我们来看看循环层可能是什么样子。

![](img/441067dfefb7b290d94e5bddda5a78be.png)

【https://colah.github.io/posts/2015-08-Understanding-LSTMs/】来源:[](https://colah.github.io/posts/2015-08-Understanding-LSTMs/)

*在我们开始解释之前，让我们先定义一下所有这些变量代表什么。*

***ht** 在时间 t 的输出*

***xt** 在时间 t 输入*

***一个**循环层(loop)*

*该图试图说明的是，递归层一次处理一个单词或输入，并与前一次迭代的输出相结合。因此，随着我们在输入序列中的进一步发展，我们对整个文本建立了更复杂的理解。*

*我们刚刚看到的是一个简单的 RNN 层。对于简单的问题，它可以有效地处理较短的文本序列，但也有许多缺点。其中之一是，随着文本序列变得越来越长，网络越来越难以正确理解文本。*

# *LSTM*

*我们上面深入讨论的层被称为简单层。然而，确实存在一些比简单的 RNN 层工作得更好的其他循环层(包含循环的层)。我们将在这里谈论的一个叫做 LSTM(长短期记忆)。这一层的工作方式与 simpleRNN 层非常相似，但是增加了一种访问过去任何时间步长的输入的方式。而在我们简单的 RNN 层中，随着输入的深入，来自先前时间戳的输入逐渐消失。有了 LSTM，我们就有了一个长期的记忆数据结构来存储所有之前看到的输入以及我们看到它们的时间。这允许我们在任何时间点访问我们想要的任何先前的值。这增加了我们网络的复杂性，并允许它发现输入之间以及输入何时出现的更有用的关系。*

*出于本课程的目的，我们将不再深入这些层如何工作的数学或细节。*

# *LSTM*

*现在是时候看看一个循环神经网络的运行了。对于这个例子，我们要做一个叫做情感分析的东西。*

*维基百科对该术语的正式定义如下:*

**对一篇文章中表达的观点进行计算机识别和分类的过程，尤其是为了确定作者对特定主题、产品等的态度。是正的、负的或中性的。**

*我们在这里使用的例子是将电影评论分为正面、负面或中性。*

**本指南基于以下 tensorflow 教程:*[*https://www . tensor flow . org/tutorials/text/text _ classification _ rnn*](https://www.tensorflow.org/tutorials/text/text_classification_rnn)*

# *情感分析*

*我们从从 keras 加载 IMDB 电影评论数据集开始。该数据集包含来自 IMDB 的 25，000 条评论，其中每条评论都已经过预处理，并具有正面或负面的标签。每个评论都由整数编码，代表一个词在整个数据集中的常见程度。例如，由整数 3 编码的单词意味着它是数据集中第三个最常见的单词。*

```
*%tensorflow_version 2.x # this line is not required unless you are in a notebookfrom keras.datasets import imdbfrom keras.preprocessing import sequenceimport kerasimport tensorflow as tfimport osimport numpy as npVOCAB_SIZE = 88584MAXLEN = 250BATCH_SIZE = 64(train_data, train_labels), (test_data, test_labels) = imdb.load_data(num_words = VOCAB_SIZE)[ ]# Lets look at one reviewtrain_data[1]*
```

# *更多预处理*

*如果我们看一下我们的一些评论，我们会注意到它们的长度不同。这是一个问题。我们不能将不同长度的数据传递到我们的神经网络中。所以一定要让每次复习的长度一样。为此，我们将遵循以下程序:*

*   *如果评论超过 250 字，那么删掉多余的字*
*   *如果评论少于 250 字，则添加必要数量的 0，使其等于 250 字。*

*幸运的是，keras 有一个功能可以帮我们做到这一点:*

```
*train_data = sequence.pad_sequences(train_data, MAXLEN)test_data = sequence.pad_sequences(test_data, MAXLEN)*
```

# *创建模型*

*现在是创建模型的时候了。我们将使用一个单词嵌入层作为我们模型的第一层，然后添加一个 LSTM 层，该层进入一个密集的节点，以获得我们预测的情绪。*

*32 代表嵌入层生成的矢量的输出维数。如果我们愿意，我们可以改变这个值！*

```
*model = tf.keras.Sequential([tf.keras.layers.Embedding(VOCAB_SIZE, 32),tf.keras.layers.LSTM(32),tf.keras.layers.Dense(1, activation=”sigmoid”)])model.summary()*
```

# *培养*

*现在是编译和训练模型的时候了。*

```
*model.compile(loss=”binary_crossentropy”,optimizer=”rmsprop”,metrics=[‘acc’])history = model.fit(train_data, train_labels, epochs=10, validation_split=0.2)*
```

*我们将根据我们的训练数据评估该模型，看看它的表现如何。*

```
*results = model.evaluate(test_data, test_labels)print(results)*
```

*因此，我们的得分在 80%左右。对于一个简单的循环网络来说，这已经不错了。*

# *做预测*

*现在让我们利用我们的网络对我们自己的评论进行预测。*

*因为我们的评论是编码好的，所以需要将我们写的任何评论转换成那种形式，以便网络能够理解它。为此，从数据集中加载编码，并使用它们来编码我们自己的数据。*

```
*word_index = imdb.get_word_index()def encode_text(text):tokens = keras.preprocessing.text.text_to_word_sequence(text)tokens = [word_index[word] if word in word_index else 0 for word in tokens]return sequence.pad_sequences([tokens], MAXLEN)[0]text = “that movie was just amazing, so amazing”encoded = encode_text(text)print(encoded)*
```

*当我们在这里的时候，让我们做一个解码函数*

```
*reverse_word_index = {value: key for (key, value) in word_index.items()}def decode_integers(integers):PAD = 0text = “”for num in integers:if num != PAD:text += reverse_word_index[num] + “ “return text[:-1]print(decode_integers(encoded))*
```

*现在是做预测的时候了*

```
*def predict(text):encoded_text = encode_text(text)pred = np.zeros((1,250))pred[0] = encoded_textresult = model.predict(pred)print(result[0])positive_review = “That movie was! really loved it and would great watch it again because it was amazingly great”predict(positive_review)negative_review = “that movie really sucked. I hated it and wouldn’t watch it again. Was one of the worst things I’ve ever watched”predict(negative_review)*
```

# *RNN 游戏发生器*

*现在我们来看一个迄今为止最酷的例子。我们将使用 RNN 生成一个剧本。我们将简单地向 RNN 展示我们希望它重新创建的东西的一个例子，它将学习如何自己编写它的一个版本。我们将使用字符预测模型来实现这一点，该模型将可变长度序列作为输入，并预测下一个字符。我们可以连续多次使用该模型，将上一次预测的输出作为下一次调用的输入来生成序列。*

**本指南基于以下:*[](https://www.tensorflow.org/tutorials/text/text_generation)*

```
**%tensorflow_version 2.x # this line is not required unless you are in a notebookfrom keras.preprocessing import sequenceimport kerasimport tensorflow as tfimport osimport numpy as np**
```

# **资料组**

**对于这个例子，我们只需要一组训练数据。事实上，如果我们愿意，我们可以写我们自己的诗或剧本，并把它传给网络进行训练。然而，为了简单起见，我们将使用莎士比亚戏剧中的一段摘录。**

```
**path_to_file = tf.keras.utils.get_file(‘shakespeare.txt’, ‘https://storage.googleapis.com/download.tensorflow.org/data/shakespeare.txt')**
```

# **加载您自己的数据**

**要加载您自己的数据，您需要在 google colab 中上传一个文件。然后你需要按照上面的步骤，但是加载这个新文件。**

```
**from google.colab import filespath_to_file = list(files.upload().keys())[0]**
```

# **读取文件内容**

**让我们看看文件的内容。**

```
**# Read, then decode for py2 compat.text = open(path_to_file, ‘rb’).read().decode(encoding=’utf-8')# length of text is the number of characters in itprint (‘Length of text: {} characters’.format(len(text)))[ ]# Take a look at the first 250 characters in textprint(text[:250])**
```

# **编码**

**因为这个文本还没有被编码，我们需要自己来做。我们将把每个独特的字符编码成一个不同的整数。**

```
**vocab = sorted(set(text))# Creating a mapping from unique characters to indiceschar2idx = {u:i for i, u in enumerate(vocab)}idx2char = np.array(vocab)def text_to_int(text):return np.array([char2idx[c] for c in text])text_as_int = text_to_int(text)# lets look at how part of our text is encodedprint(“Text:”, text[:13])print(“Encoded:”, text_to_int(text[:13]))**
```

**这里我们将创建一个函数，可以将数值转换成文本。**

```
**def int_to_text(ints):try:ints = ints.numpy()except:passreturn ‘’.join(idx2char[ints])print(int_to_text(text_as_int[:13]))**
```

# **创建培训示例**

**记住我们的任务是给模型输入一个序列，并让它返回给我们下一个字符。这意味着我们需要将上面的文本数据分成许多更短的序列，我们可以将它们作为训练样本传递给模型。**

**我们将准备的训练示例将使用一个 *seq_length* 序列作为输入，一个 *seq_length* 序列作为输出，其中该序列是向右移动了一个字母的原始序列。例如:**

**`input: Hell | output: ello`**

**我们的第一步是从文本数据中创建一个字符流。**

```
**seq_length = 100 # length of sequence for a training exampleexamples_per_epoch = len(text)//(seq_length+1)# Create training examples / targetschar_dataset = tf.data.Dataset.from_tensor_slices(text_as_int)**
```

**接下来，我们可以使用 batch 方法将这个字符流转换成所需长度的批次。**

```
**sequences = char_dataset.batch(seq_length+1, drop_remainder=True)**
```

**现在我们需要使用这些长度为 101 的序列，并将它们分成输入和输出。**

```
**def split_input_target(chunk): # for the example: helloinput_text = chunk[:-1] # helltarget_text = chunk[1:] # elloreturn input_text, target_text # hell, ellodataset = sequences.map(split_input_target) # we use map to apply the above function to every entry[ ]for x, y in dataset.take(2):print(“\n\nEXAMPLE\n”)print(“INPUT”)print(int_to_text(x))print(“\nOUTPUT”)print(int_to_text(y))**
```

**最后，我们需要进行批量训练。**

```
**BATCH_SIZE = 64VOCAB_SIZE = len(vocab) # vocab is number of unique charactersEMBEDDING_DIM = 256RNN_UNITS = 1024# Buffer size to shuffle the dataset# (TF data is designed to work with possibly infinite sequences,# so it doesn’t attempt to shuffle the entire sequence in memory. Instead,# it maintains a buffer in which it shuffles elements).BUFFER_SIZE = 10000data = dataset.shuffle(BUFFER_SIZE).batch(BATCH_SIZE, drop_remainder=True)**
```

# **构建模型**

**现在是建立模型的时候了。我们将使用一个 LSTM 嵌入层和一个密集层，后者包含训练数据中每个唯一字符的节点。密集层将给出所有节点的概率分布。**

```
**def build_model(vocab_size, embedding_dim, rnn_units, batch_size):model = tf.keras.Sequential([tf.keras.layers.Embedding(vocab_size, embedding_dim,batch_input_shape=[batch_size, None]),tf.keras.layers.LSTM(rnn_units,return_sequences=True,stateful=True,recurrent_initializer=’glorot_uniform’),tf.keras.layers.Dense(vocab_size)])return modelmodel = build_model(VOCAB_SIZE,EMBEDDING_DIM, RNN_UNITS, BATCH_SIZE)model.summary()**
```

# **创建损失函数**

**现在我们要为这个问题创建我们自己的损失函数。这是因为我们的模型将输出(64，sequence_length，65)形状的张量，该张量表示批量中每个序列的每个时间步中每个字符的概率分布。**

**然而，在我们这样做之前，让我们来看看一个样本输入和来自我们的未训练模型的输出。这是为了让我们了解模型给了我们什么。**

```
**for input_example_batch, target_example_batch in data.take(1):example_batch_predictions = model(input_example_batch) # ask our model for a prediction on our first batch of training data (64 entries)print(example_batch_predictions.shape, “# (batch_size, sequence_length, vocab_size)”) # print out the output shape# we can see that the predicition is an array of 64 arrays, one for each entry in the batchprint(len(example_batch_predictions))print(example_batch_predictions)# lets examine one predictionpred = example_batch_predictions[0]print(len(pred))print(pred)**
```

**请注意，这是一个长度为 100 的 2d 数组，其中每个内部数组都是每个时间步长下一个字符的预测值**

```
**# and finally well look at a prediction at the first timesteptime_pred = pred[0]print(len(time_pred))print(time_pred)# and of course its 65 values representing the probabillity of each character occuring next**
```

**如果我们想确定预测的字符，我们需要对输出分布进行采样(根据概率选择一个值)**

```
**sampled_indices = tf.random.categorical(pred, num_samples=1)**
```

**现在我们可以改变数组的形状，将所有的整数转换成数字，以查看实际的字符**

```
**sampled_indices = np.reshape(sampled_indices, (1, -1))[0]predicted_chars = int_to_text(sampled_indices)predicted_chars # and this is what the model predicted for training sequence 1**
```

**因此，现在我们需要创建一个损失函数，将该输出与预期输出进行比较，并给出一些数值来表示两者有多接近。**

```
**def loss(labels, logits):return tf.keras.losses.sparse_categorical_crossentropy(labels, logits, from_logits=True)**
```

# **编译模型**

**在这一点上，我们可以认为我们的问题是一个分类问题，模型预测每个独特的字母出现的概率。**

```
**model.compile(optimizer=’adam’, loss=loss)**
```

# **创建检查点**

**现在，我们将设置和配置我们的模型，以便在训练时保存 checkpoinst。这将允许我们从检查点加载我们的模型，并继续训练它。**

```
**# Directory where the checkpoints will be savedcheckpoint_dir = ‘./training_checkpoints’# Name of the checkpoint filescheckpoint_prefix = os.path.join(checkpoint_dir, “ckpt_{epoch}”)checkpoint_callback=tf.keras.callbacks.ModelCheckpoint(filepath=checkpoint_prefix,save_weights_only=True)**
```

# **培养**

**最后，我们将开始训练模型。**

****如果这需要一段时间，转到运行时>更改运行时类型并选择硬件加速器下的“GPU”。****

```
**history = model.fit(data, epochs=50, callbacks=[checkpoint_callback])**
```

# **加载模型**

**我们将使用 batch _ size 1 从一个检查点重建模型，这样我们可以向模型提供一部分文本，并让它进行预测。**

```
**model = build_model(VOCAB_SIZE, EMBEDDING_DIM, RNN_UNITS, batch_size=1)**
```

**一旦模型完成训练，我们可以使用下面的代码找到存储模型权重的**最后一个检查点**。**

```
**model.load_weights(tf.train.latest_checkpoint(checkpoint_dir))model.build(tf.TensorShape([1, None]))**
```

**我们可以通过指定要加载的确切文件来加载**任何我们想要的检查点**。**

```
**checkpoint_num = 10model.load_weights(tf.train.load_checkpoint(“./training_checkpoints/ckpt_” + str(checkpoint_num)))model.build(tf.TensorShape([1, None]))**
```

# **生成文本**

**现在，我们可以使用 tensorflow 提供的可爱函数，使用我们喜欢的任何起始字符串来生成一些文本。**

```
**def generate_text(model, start_string):# Evaluation step (generating text using the learned model)# Number of characters to generatenum_generate = 800# Converting our start string to numbers (vectorizing)input_eval = [char2idx[s] for s in start_string]input_eval = tf.expand_dims(input_eval, 0)# Empty string to store our resultstext_generated = []# Low temperatures results in more predictable text.# Higher temperatures results in more surprising text.# Experiment to find the best setting.temperature = 1.0# Here batch size == 1model.reset_states()for i in range(num_generate):predictions = model(input_eval)# remove the batch dimensionpredictions = tf.squeeze(predictions, 0)# using a categorical distribution to predict the character returned by the modelpredictions = predictions / temperaturepredicted_id = tf.random.categorical(predictions, num_samples=1)[-1,0].numpy()# We pass the predicted character as the next input to the model# along with the previous hidden stateinput_eval = tf.expand_dims([predicted_id], 0)text_generated.append(idx2char[predicted_id])return (start_string + ‘’.join(text_generated))inp = input(“Type a starting string: “)print(generate_text(model, inp))**
```

***和*这就是本模块的全部内容！我强烈建议修改我们刚刚创建的模型，看看你能让它做些什么！**

# **来源**

*   **乔莱·françois.用 Python 进行深度学习。曼宁出版公司，2018。**
*   **"文本分类与 RNN:张量流核心."张量流，[www.tensorflow.org/tutorials/text/text_classification_rnn](http://www.tensorflow.org/tutorials/text/text_classification_rnn)。**
*   **"文本生成与 RNN:张量流核心."张量流，[www.tensorflow.org/tutorials/text/text_generation](http://www.tensorflow.org/tutorials/text/text_generation)。**
*   **“了解 LSTM 网络”了解 LSTM 网络——科拉的博客，[https://colah.github.io/posts/2015-08-Understanding-LSTMs/](https://colah.github.io/posts/2015-08-Understanding-LSTMs/)。**