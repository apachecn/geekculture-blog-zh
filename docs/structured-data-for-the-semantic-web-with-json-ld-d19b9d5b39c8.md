# 基于 JSON-LD 的语义网结构化数据

> 原文：<https://medium.com/geekculture/structured-data-for-the-semantic-web-with-json-ld-d19b9d5b39c8?source=collection_archive---------52----------------------->

## 如何使用 JSON for Linking Data (JSON+LD)将语义数据添加到您的文章中，并让 Google 将它们显示在他们的搜索图库中。

🔔**本文最初发布在我的网站上，【MihaiBojin.com】[](https://MihaiBojin.com/personal-site/structured-semantic-data-with-json-ld?utm_source=Medium&utm_medium=organic&utm_campaign=rss)****。**🔔****

**![](img/58c6d55845ae44347955b553f656a4bf.png)**

**Photo by [Ferenc Almasi](https://unsplash.com/@flowforfrank?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/json-sematic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)**

**我今天花了一整天的时间学习用于链接语义网的数据和结构化数据的 JSON。**

**我从这篇很好的介绍文章开始[，然后去了](https://nystudio107.com/blog/json-ld-structured-data-and-erotica)[谷歌的高级搜索引擎优化页面](https://developers.google.com/search/docs/advanced/guidelines/get-started)。**

**我决定为我的站点文章实现这个功能，尽管我可以在稍后的中实现[其他类型。](https://developers.google.com/search/docs/guides/search-gallery)**

**为我的站点创建 JSON+LD 模式并不像我预期的那么容易。我找不到任何我想要的现成的 GatsbyJS 或 React 插件。最接近的一个是 [react-schemaorg](https://github.com/google/react-schemaorg) ,它似乎包含了`<script>`标签的生成——我不会用插件来做这个。**

**最后，我编写了代码来生成 JSON+LD 模式，这是基于这种类型所必需的属性。**

**在我写这篇文章的时候，我被谷歌提供的例子[弄糊涂了，具体来说，我应该使用哪个`@type`、](https://developers.google.com/search/docs/data-types/article)[文章](https://schema.org/Article)、[新闻文章](https://schema.org/NewsArticle)和[博客文章](https://schema.org/BlogPosting)。**

**据我所知，从 SEO 的角度来看，没有什么大的不同；我决定用通用的[条](https://schema.org/Article)型。**

**我最后得到了下面的助手代码(`src/components/article-schema.js`):**

```
function ArticleSchema({
  title,
  description,
  date,
  lastUpdated,
  tags,
  image,
  canonicalURL,
}) {
  // load metadata defined in gatsby-config.js
  const { siteMetadata } = useSiteMetadata();
  // load the site's logo as a file/childImageSharp by its relative path
  const { siteLogo } = useSiteLogo();

  const authorProfiles = [
    SITE_URL,
    `https://twitter.com/${siteMetadata.social.twitter}/`,
    `https://linkedin.com/in/${siteMetadata.social.linkedin}/`
    `https://github.com/${siteMetadata.social.github}`
  ];

  const img = image ? getImage(image.image) : null;
  const imgSrc = image ? getSrc(image.image) : null;

  const jsonData = {
    '@context': `https://schema.org/`,
    '@type': `Article`,

    // helper that generates `'@type': 'Person'` schema
    author: AuthorModel({
      name: siteMetadata.author.name,
      sameAs: authorProfiles,
    }),
    url: canonicalURL,
    headline: title,
    description: description,
    keywords: tags.join(','),
    datePublished: date,
    dateModified: lastUpdated || date,

    // helper that generates `'@type': 'ImageObject'` schema
    image: ImageModel({
      url: siteMetadata.siteUrl + imgSrc,
      width: img?.width,
      height: img?.height,
      description: image.imageAlt,
    }),

    // helper that generates `'@type': 'Organization'` schema
    publisher: PublisherModel({
      name: siteMetadata.title,

      // helper that generates `'@type': 'ImageObject'` schema
      logo: ImageModel({
        url: siteLogo.src,
        width: siteLogo.image.width,
        height: siteLogo.image.height,
      }),
    }),
    mainEntityOfPage: {
      '@type': `WebPage`,
      '@id': siteMetadata.siteUrl,
    },
  };

  return (
    <Helmet>
      <script type="application/ld+json">
        {JSON.stringify(jsonData, undefined, 4)}
      </script>
    </Helmet>
  );
}
ArticleSchema.defaultProps = {
  tags: [],
};

ArticleSchema.propTypes = {
  title: PropTypes.string.isRequired,
  description: PropTypes.string.isRequired,
  date: PropTypes.string,
  lastUpdated: PropTypes.string,
  tags: PropTypes.arrayOf(PropTypes.string),
  image: PropTypes.object,
  canonicalURL: PropTypes.string,
};

export default ArticleSchema;
```

**因为您可以多次调用 [react-helmet](https://www.npmjs.com/package/react-helmet) ，所以我选择直接从`blog-post.js`调用`<ArticleSchema ... />`，它是呈现我的博客帖子和文章的组件。**

**一旦一切都设置好了，我就在 Google 的 [rich results tester](https://search.google.com/test/rich-results) 中测试结果。**

**然后我意识到在 HTML 中包含一个`<article>`标签也被解释为语义标记，这与我的 JSON+LD 定义相冲突，duuuh！**

**我立即去掉了`<article>`、`<header>`和`<section>`标签。**

**由于 Google 没有在它的模式中定义`itemProp`，指定它是多余的，但是现在，我将文章的正文注释为:**

```
<div dangerouslySetInnerHTML= itemProp="articleBody" />
```

**我现在已经正确地配置了我所有的帖子，以显示在谷歌的[搜索库](https://developers.google.com/search/docs/guides/search-gallery)中，这将有望及时为我的文章带来更多的有机流量！**

**如果你喜欢这篇文章，并想阅读更多类似的文章，[请订阅我的简讯](https://motivated-founder-807.ck.page/db1cf284bf)；我每隔几周就发一封！**