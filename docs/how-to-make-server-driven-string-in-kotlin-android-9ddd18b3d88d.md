# 如何在 kotlin- Android 中做一个字符串格式化服务器驱动？

> 原文：<https://medium.com/geekculture/how-to-make-server-driven-string-in-kotlin-android-9ddd18b3d88d?source=collection_archive---------24----------------------->

让我们假设我们想在 kotlin 后端响应的基础上改变一个字符串的颜色，并想制作一个字符串 URL 的一些单词，单击它将重定向到一个网页。

![](img/c5c32cac2c6f9f9658f947193071f279.png)

让我们假设我们想要实现像这样的^，你可以看到一些文本有链接和一些粗体字(如:-细节)，现在这整个字符串也来自后端，
让我们假设我们的响应是这样的:-

```
{
  "string": "Read more about %s and %s in $",
  "text": [
    {
      "title": "TnC",
      "href": "[https://www.google.com](https://www.google.com)"
    },
    {
      "title": "Product Information",
      "href": "[https://www.linkedin.com](https://www.linkedin.com)"
    },
    {
      "title": "Detail",
      "href": null
    }
  ]
}
```

如果你看到我们的后端响应，我们已经给了字符串一些特殊的字符，我们希望在本地更改或格式化这些字符串，我们还创建了一个包含标题和链接的 href 的文本数组，这样我们就可以在字符串中点击一些单词，并在点击后打开一个网页，如果 href 为空，我们将假设我们必须在字符串中以粗体显示该单词。你们可以根据你们的要求做出回应，我刚刚给出了一个例子，我们如何在字符串中使用特殊字符来实现这一点。

是时候写前端科特林代码✌️了

```
**val** footerString = SpannableStringBuilder(**"Read more about %s and %s in $"**)
addFormattingToFooterString(footerString, footer)

*with*(**binding**.contentView.tvTermsAndConditions) **{** *text* = footerString
    *linksClickable* = **true** *movementMethod* = LinkMovementMethod.getInstance()
**}**
```

正如你在上面的代码中看到的，我们已经创建了一个 spannable 字符串，当我们想给字符串中的一些特定单词添加一些样式或跨度时，spannable 字符串非常有用。
之后我们调用了给字符串添加格式的方法来给字符串添加样式和链接。
下面，我们使我们的整个文本视图可点击，并添加了链接移动，所以每当我们添加任何 href(url)到我们的字符串中的任何单词，点击它后，将打开相应的网页。

```
**private fun** addFormattingToFooterString(
    footerString: SpannableStringBuilder,
    footer: Footer
) {
    **var** linkListIndex = 0
    footerString.*forEachIndexed* **{** index, char **->
        if**(char == **'$'**){
            footerString.setSpan(
                StyleSpan(Typeface.*BOLD*),
                index, index+1, Spannable.*SPAN_INCLUSIVE_INCLUSIVE* )
            footerString.replace(index, index+1, **"Detail"**)
            linkListIndex++ }
        **if** (char == **'%'**) {
            footerString.setSpan(
                StyleSpan(Typeface.*BOLD*),
                index,
                index + 2,
                Spannable.*SPAN_INCLUSIVE_INCLUSIVE* )
            **val** footerTermsAndConditionString = footer.**link**[linkListIndex].**text
            val** replaceableString =
                **"<a href='"** + footer.**link**[linkListIndex].**url** + **"'><u>$**footerTermsAndConditionString**</u></a>"** footerString.replace(
                index,
                index + 2,
                **if** (Build.VERSION.*SDK_INT* >= Build.VERSION_CODES.*N*) {
                    Html.fromHtml(replaceableString, Html.*FROM_HTML_MODE_LEGACY*)
                } **else** {
                    Html.fromHtml(replaceableString)
                }
            )
            linkListIndex++
        }
    **}** }
```

在上面的方法中，我们做了很多事情，让我们一个一个地理解所有这些事情。
1。首先，我们创建了一个 listIndex 变量，用于获取上面创建的文本数组中标题的索引(在后端响应中)。

2.让我们遍历我们的 footerString，并使用`$`字符检查粗体字符串，如果它出现在响应字符串中，我们将使用后端字符串替换它，并将其设为粗体。首先，我们将为此设置跨度，并将当前索引和当前索引+1 之间的字符串设为粗体(因为我们现在只想替换长度为 1 的单词(当前索引+1-currentIndex，` $ `)。

3.现在，在这之后，我们用我们想要使用后端响应加粗的单词替换字符。现在，因为我们在这里使用了 **SPAN_INCLUSIVE_INCLUSIVE** ，所以这个 SPAN 将扩展到包括字符串中任何新插入的字符，并将它们加粗，根据文档，它的定义是:-

```
*/**
 * Spans of type SPAN_INCLUSIVE_INCLUSIVE expand
 * to include text inserted at either their starting or ending point.
 */*
```

4.现在下一步是我们添加网址的话与%s 和取代%s 的文字来在后端的反应，并添加网址给他们。因为我们已经维护了一个列表索引变量来查找文本数组中被替换字符串的索引，所以我们将把它用作被替换的字符串。

5.现在我们使用 href 的 anchor 标签将 URL 添加到特定的单词中。

6.最后，我们将用 Html 字符串替换我们的%s，这样就完成了。

因此，正如你们可以看到我们如何设计我们的后端合同，使像样式，字符串后端驱动的单词的 URL 这样的事情。这有很多好处，将来我们可以在前端改变尽可能多的单词(改变它们的样式，给一些单词添加 URL)，而无需任何应用程序发布。对于我们的用例，我们也可以使用[clickables span](https://developer.android.com/reference/android/text/style/ClickableSpan)，但是它创建了一些额外的类和一些额外的代码，所以我用了这个。

感谢您阅读这篇文章，如果您有任何疑问或反馈，可以通过 [Linkedin](https://www.linkedin.com/in/vikas-bajpayee-4a17aa106/) 联系我。