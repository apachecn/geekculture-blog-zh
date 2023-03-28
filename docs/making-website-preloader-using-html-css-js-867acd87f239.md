# 用 HTML CSS JS 制作网站预装程序

> 原文：<https://medium.com/geekculture/making-website-preloader-using-html-css-js-867acd87f239?source=collection_archive---------2----------------------->

![](img/67cd00b1afd526b4154519b5134a087b.png)

你好，我的朋友们，这次我们将讨论网站页面为 muabah 时的 Preloader。这在朋友创建网站时非常有用。我们在这里使用来自 jQuery，CSS 和 HTML 的代码，我们可以在我们的网站页面上创建一个预加载程序。

在我看来，在网站页面上使用预加载器的好处是非常好的，因为它使我们在看到我们的网站时不会先随机搜索，因为所有的代码都没有完全加载，使用预加载器，网站页面将被预加载器完全关闭，当所有的代码都被浏览器加载后，预加载器将自动从访问者面前消失。

在网站页面上使用预加载程序的缺点，在我看来，当访问者的互联网连接很慢，并且看到我们显示的加载动画只是旋转，而如果网站页面完全加载，预加载程序就会消失时，对访问者来说可能会很无聊。

逐步地

1.用最新版本准备好 [jQuery](http://jquery.com/download) 。

2.用 HTML 格式创建新文档。在这里，我将所有的编码放在一个文件中。

3.在文档中编写 HTML 结构。

4.在标签中，像这样调用 jQuery 文件:

```
<script src="http://code.jquery.com/jquery-2.2.1.min.js"></script>
```

5.然后在同一个标签中也写几行 CSS 代码，即标签。

```
<style type="text/css">
.wrapperanimate{
    width:200px;
    height:60px;
    position: absolute;
    left:50%;
    top:50%;
    transform: translate(-50%, -50%);
}
.circle{
    width:20px;
    height:20px;
    position: absolute;
    border-radius: 50%;
    background-color: #fff;
    left:15%;
    transform-origin: 50%;
    animation: circle .5s alternate infinite ease;
}

@keyframes circle{
    0%{
        top:60px;
        height:5px;
        border-radius: 50px 50px 25px 25px;
        transform: scaleX(1.7);
    }
    40%{
        height:20px;
        border-radius: 50%;
        transform: scaleX(1);
    }
    100%{
        top:0%;
    }
}
.circle:nth-child(2){
    left:45%;
    animation-delay: .2s;
}
.circle:nth-child(3){
    left:auto;
    right:15%;
    animation-delay: .3s;
}
.shadow{
    width:20px;
    height:4px;
    border-radius: 50%;
    background-color: rgba(0,0,0,.5);
    position: absolute;
    top:62px;
    transform-origin: 50%;
    z-index: -1;
    left:15%;
    filter: blur(1px);
    animation: shadow .5s alternate infinite ease;
}

@keyframes shadow{
    0%{
        transform: scaleX(1.5);
    }
    40%{
        transform: scaleX(1);
        opacity: .7;
    }
    100%{
        transform: scaleX(.2);
        opacity: .4;
    }
}
.shadow:nth-child(4){
    left: 45%;
    animation-delay: .2s
}
.shadow:nth-child(5){
    left:auto;
    right:15%;
    animation-delay: .3s;
}
.wrapperanimate span{
    position: absolute;
    top:75px;
    font-family: 'Lato';
    font-size: 20px;
    letter-spacing: 12px;
    color: #fff;
    left:15%;
}
</style>
```

6.在标签中显示预装程序。

```
<div class="wrapperanimate">
        <div class="circle"></div>
        <div class="circle"></div>
        <div class="circle"></div>
        <div class="shadow"></div>
        <div class="shadow"></div>
        <div class="shadow"></div>
        <span>Loading</span>
    </div>
```

7.请在浏览器中运行 HTML 文档，如果合适，它会出现如下。

![](img/d5fa7c7ed3fa4581aa3b5d26b3e384e6.png)

8.为什么装货不停？请耐心等待，因为我们还没有给出让预加载程序自动关闭的 jQuery 命令。那怎么做呢？很简单，在标签中添加下面一行代码，我建议放在代码的底部调用 jQuery 文件。

```
<script>
$(document).ready(*function*(){
$(".preloader").fadeOut();
})
</script>
```

9.请在浏览器中再次运行，为什么加载会立即消失？这是因为我们在自己的电脑上运行它，而不是在互联网上。

那是我这次做的教程，希望有用。

谢了。