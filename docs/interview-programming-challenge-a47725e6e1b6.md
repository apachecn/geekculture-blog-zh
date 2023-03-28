# 面试编程挑战

> 原文：<https://medium.com/geekculture/interview-programming-challenge-a47725e6e1b6?source=collection_archive---------5----------------------->

![](img/268f7b0a9418b1bce311234773756ac3.png)

一周前，我第一次参加了一家公司的技术面试，这家公司让我非常兴奋。我以前只参加过模拟面试，所以即将到来的真正面试既令人兴奋又充满压力。当这一天到来时，我意识到这实际上是一次非常有趣的经历。我学到了很多其他公司的招聘流程。最棒的是，我得到了一个新的编程挑战，从中获得乐趣！在这篇文章中，我将向你展示挑战是什么，以及我想出的解决方案。

挑战在于创建一个自动纠正字符串的函数。规则很简单。

*   只有第一个单词的第一个字母需要大写。
*   第二个单词的第一个字母可以大写或小写。
*   单词第一个字母之后的所有字母都必须小写。

我决定用 Ruby 编程语言来解决这个问题。我的想法是首先将输入的句子分成一个单词数组，然后创建一个空数组来存储最终的结果。

```
def auto_correct(str)
  arr = str.split(' ')
  nu_arr = []
end
```

好了，现在我知道了如何处理原始字符串，我想我需要遍历每个单词。我需要一种方法来识别第一个单词，以便它总是大写。为了做到这一点我用了。每个都有索引。如果索引是 0，那么我可以将这个单词的第一个字母大写。在这个块中，我还需要将单词拆分成单独的字母，并为处理后的单词设置一个变量。这是我的代码到目前为止的样子。

```
def auto_correct(str)
  arr = str.split(' ')
  nu_arr = [] arr.each_with_index do |word, index|
    split_word = word.split('')
    nu_word = ''
  end
end
```

现在，我需要检查每个字母，并确定它是大写字母还是小写字母。我决定再做一次。each_with_index 块遍历每个字母，改变大小写，并将其连接到 nu_word。如果单词的索引是 0，字母的索引是 0，那么这个字母必须大写。否则，如果字母的索引是零而不是第一个单词，则该字母可以保持原样，然后连接到 nu_word。否则字母是小写的。我写了 if 语句来说明这一点。在 if 语句之后，我会把 nu_word 铲到 nu_arr。这就是它的样子。

```
def auto_correct(str)
  arr = str.split(' ')
  nu_arr = [] arr.each_with_index do |word, index|
    split_word = word.split('')
    nu_word = ''
    split_word.each_with_index do |letter, i|
      if index == 0 && i == 0
        nu_word += letter.upcase
      elsif i == 0
        nu_word += letter
      else
        nu_word += letter.downcase
      end
    end
    nu_arr << nu_word
  end
end
```

很好，所以现在我们的 nu_arr 应该包含所有根据规则正确处理的单词。现在唯一要做的就是将单词连接成一个字符串，并将结果输出到控制台。让我们调用方法，看看我们的结果

```
def auto_correct(str)
  arr = str.split(' ')
  nu_arr = [] arr.each_with_index do |word, index|
    split_word = word.split('')
    nu_word = ''
    split_word.each_with_index do |letter, i|
      if index == 0 && i == 0
        nu_word += letter.upcase
      elsif i == 0
        nu_word += letter
      else
        nu_word += letter.downcase
      end
    end
    nu_arr << nu_word
  end
  puts nu_arr.join(' ')
endauto_correct("HeLlO WoRLD!")
```

在我们运行代码后，结果看起来像这样“Hello World！”。代码运行完美！

这个问题很有趣，这是我第一次面试，所以这是一次很棒的经历。也许这不是最好或最有效的答案，但我很自豪我能够解决这个问题。事实上，试图解决这个问题并不那么简单。我经历了一段时间的反复试验，花了一些时间让它工作起来。不管怎样，我觉得面试进行得很顺利，我真的很喜欢获得一些面试经验。让我知道你的解决方案是什么样的。我很想看看其他人有什么想法。感谢阅读和快乐编码！😎