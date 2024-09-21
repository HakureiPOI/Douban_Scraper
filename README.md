### ***HakureiPOI https://github.com/HakureiPOI/Douban_Scraper.git***

---

## 大致说一下内容 


### 1. **AnimaIndex.ipynb**

获取豆瓣搜索页面中搜索 “动漫” 获得的动漫的序号

豆瓣的单个电影页面是相当好爬取的但是这个豆瓣搜索真是莫名其妙

本来以为是动态加载搜索结果列表，所以尝试使用selenium进行爬取，但是获得的内容也不是想要的

又 F12 查了很久 Network 硬是没有找到一个地方藏了搜索结果的数据

后来在网上找了很久发现有大佬提到搜索结果被藏到了 html 的这个标签里

>  <script type="text/javascript">
>    window.__DATA__ = {"count": 15, "error_info": "", "items": [...]
>    window.__USER__ = { }
>  </script>

window.\__DATA\__["items"] 的内容乍一看看不懂，其实就是搜索结果的电影名称作者啥的信息，只是被加密了

当然反过来破解应该是相当有难度的，不过有趣的是，这里面数字是没有进行加密的，也就是可以看到一些熟悉的内容比如年份 2022 啊之类的

这个就意味着，***我们可以不破解这个加密的手段，直接获得搜索结果的序号***

因此就有了这个第一部分的程序，先获得所有待爬取动漫页的序号，得到了 **anima.csv** 等文件



### 2. **AnimaComment.ipynb**

这个就比较简单了，从动漫页获取动漫的 title 等需要的信息之后，跳转剧评、短评两种评论页面，直接爬取内容就行了

不过这里主要的问题就是爬太快就会给 IP 封了，我设置了比较长的封禁后等待时间结果给我 kaggle 笔记本跑超时了

所以现在获得的 **comments.xlsx** 只有1w+ 行，也只去爬取了 100 个动漫的评论（肯定还有因为等待时间太长被我直接 skip 的）

如果能准备多点 IP 应该能解决这个问题



### 3. **Preprocess.ipynb**

对上述的 **comments.xlsx** 进行简单的处理

使用 ***jieba***， ***nltk*** 对评论内容进行分词处理

使用了 ***https://github.com/goto456/stopwords.git*** 这个仓库中的 ***hit_stopwords.txt*** 作为了中文停用词库

删除了标点符号，最后获得 **filtered_text.txt**
