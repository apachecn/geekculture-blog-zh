# 使用 selenium 的动态网页抓取:抓取受保护的网站

> 原文：<https://medium.com/geekculture/dynamic-web-scraping-using-selenium-scrape-protected-websites-401de3497939?source=collection_archive---------2----------------------->

![](img/bb202319cab1345a965173a5854b9c43.png)

Designed by upklyak

## 但是！我们上次已经讨论过了！😤

在我的上一篇文章中，我们使用 beautiful soup 从网页中抓取信息，我们可能已经覆盖了几乎每个重要的方面和方法，包括包括所有不同的类型，爬行，僵尸保护，自动化，通过 NLP 抓取，但是！美汤只能处理静态网页，不能处理动态*麦克风掉落*🎤

> 如果你是刮擦的新手，或者需要了解一些高级概念，可以看看这篇文章

[https://medium . com/geek culture/get-started-with-web-scraping-for-data-analysis-Fe 55 cc 0 b 8d 61](/geekculture/getting-started-with-web-scraping-for-data-analysis-fe55cc0b8d61)

## 🥲，静态和动态网页有什么区别

你问了一个很棒的问题！静态网页使用直接嵌入你代码中的内容，而在动态网页中，**从 javascript 生成**数据，并将它们放入 DOM 元素中。保持简单

## 好的场景时间🫣

考虑你正在抓取使用分页元素的网站，但是你猜对了，当我们点击链接时，url 没有更新。这意味着我们不能使用爬行！😣也就是说，我们试着用美丽的汤做每一件可能的事情，但它不起作用！😖这意味着您没有找到根据请求加载数据的 HTML 片段！😫也就是说没有 Nutella 了😩…等等，纳特拉是爱

## 为什么是硒呢🤔

很高兴你问了🤗不仅仅是硒，还有硒+美丽的汤*旧爱仍然有一些踢它*硒在简单的术语中模拟用户与浏览器的交互。更广泛地说，selenium 是一个框架，它可以运行和执行脚本，并通过向连接您的浏览器和 Selenium 的 web 驱动程序发送和接收方法调用和数据来控制您的 Web 浏览器。所以是的，我必须安装 Chrome，但是是的，如果你没有 Chrome 或者 Firefox，现在就安装它们。有趣的是，你知道的各种自动化应用程序都使用 selenium。

## 开始前的测验时间😌

如果你去过地球，你可能知道澳大利亚，那里有一个拥有仓库的化学家。恭喜你知道我们正在刮的网站！🥳

## 安装先决条件🛠

```
# Run the commands below once 
!pip install beautifulsoup4
!pip install requests
!pip install selenium
!pip install webdriver-manager
```

## 获得我们的进口😌

```
import requests
from bs4 import BeautifulSoup
import pandas
import time
from selenium import webdriver
#dynamic scraping using selenium and beautiful soup
options = webdriver.ChromeOptions()
options.add_argument('--ignore-certificate-errors')
options.add_argument('--incognito')
options.add_argument('--headless')
driver = webdriver.Chrome("*insert_your_directory*/chromedriver", chrome_options=options)
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.common.by import By
driver = webdriver.Chrome(ChromeDriverManager().install())
```

请确保您拥有正确的 webdriver 路径。我更喜欢从网上手动下载网络驱动程序并保存在 pwd 中。

## 召唤 3 硒让我做到了这一点👻

我们将让浏览器点击下一步按钮，通过 selenium 从 **Selenium 获取源代码，而不是像上次那样请求**。这应该可行，因为我们正在模拟浏览器，就像如果每页有 63 页，我们将单击“下一步”按钮“抓取”!*java 脚本加载内容*刮！java 脚本加载内容再次刮！重复好硒将开始使用您的浏览器，现在它的拥有

```
driver.get("The_link_that_you_found_via_quiz")
src = driver.page_source
soup = BeautifulSoup(src, 'lxml')
final_list = []
df = []
for i in range(62):
    src = driver.page_source
    soup = BeautifulSoup(src, 'lxml')
    product = soup.find_all('div', class_='product')
    l = []
    for item in product:
        d={}
        d["Name"] = (item.find('div', class_ ='product__title').text.replace("[","").replace("]",""))
        d["Price"] = (item.find('span', class_ ='product__price-current').text.replace("[","").replace("]",""))
        d["Discount"] = (item.find('em', class_ ='product__price-discount'))
        l.append(d)

    final_list.extend(l)

    next_button = driver.find_elements(by=By.CLASS_NAME, value="pager__button--next")try:
        if next_button[0].is_displayed():
            driver.execute_script("arguments[0].click();",next_button[0])
            time.sleep(1)
    except:
        pass

df = pandas.DataFrame(final_list)df.to_csv("xoxo.csv")
df.to_excel("xoxo.xlsx")
```

## 再见👋

总结一下，没有太多要解释的了，但这只是深入了解 selenium 的开始，因为 selenium 不仅仅限于 web 抓取，还记得每次 GPU 在 1 秒内售罄吗？甚至是 NFT 氏症？硒你弄脏了我的朋友。但是每天都是学习新东西和快乐编码🫡的机会