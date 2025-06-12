# 印巴冲突舆情演化分析—基于API数据采集的实证研究

## 一、大模型与API应用概述

本研究主要依赖以下大模型和API：
- **腾讯AI开放平台情感分析API**：用于识别评论内容的正负面情绪分布。
- **百度自然语言处理平台（Baidu NLP）API**：用于进一步的情感倾向分析。
- **扣子平台**：用于数据爬取与初步分析。

## 二、具体实现步骤与代码

###（一）数据获取与预处理

#### 1. 使用扣子平台爬取数据
在扣子平台中，我们设置关键词为“印巴冲突”、“克什米尔”、“印军”、“巴军”、“停火”等，并限定时间范围抓取数据。

#### 2. 数据预处理代码
```Python
导入pandas库，命名为pd
导入jieba
从sklearn.feature_extraction.text导入TfidfVectorizer

# 加载评论数据
df = pd.read_csv('india_pakistan_comments.csv')  # 假设你已有采集数据

# 自定义分词函数
def 分词(文本):
    返回 [word for word in jieba.cut(text) if word.strip()]

# 分词并处理停用词
停用词 = set(open('chinese_stopwords.txt', encoding='utf-8').read().splitlines())
df['tokens'] = df['comment'].apply(lambda x: [w for w in tokenize(x) if w not in stopwords])

# 转换为字符串用于TF-IDF
df['clean_text'] = df['tokens'].apply(lambda x: ' '.join(x))

# 提取TF-IDF向量特征
向量化器 = TfidfVectorizer()
X = vectorizer.fit_transform(df['clean_text'])
