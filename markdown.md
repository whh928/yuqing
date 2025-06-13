# 印巴冲突舆情分析报告生成流程文档

## 小组分工说明

### 林翔宇
**主要职责：**
- 负责利用大模型（如 ChatGPT）编写全篇舆情报告的文本内容，包括事件回顾、研究背景、技术路线、分析结论等章节的撰写；
- 构建章节结构框架，保证报告逻辑清晰、层次分明；
- 引导小组整体研究路径的内容方向与写作风格。

**主要成果：**
- 独立完成第一章 引言、第三章 情感倾向分析的文字撰写；
- 设计全篇结构逻辑，确保语言通顺、内容聚焦；
- 根据情感极性结果撰写结论分析和舆情导向建议；
- 主动查阅外部文献及政策背景资料，丰富报告理论深度。

### 吴昊
**主要职责：**
- 利用 Python 编程、情感分析 API、扣子平台等工具，进行数据抓取、清洗、建模与可视化操作；
- 实现舆情热度趋势图、词云图、情感倾向图、K-Means 聚类图等技术内容；
- 撰写所有代码及其注释，保证代码可运行、逻辑清晰。

**主要成果：**
- 编写并调试所有 API 请求代码（腾讯/百度情感分析接口）；
- 实现 TF-IDF 向量构建与 K-Means 聚类，并生成可视化结果图；
- 完成数据清洗、分词、向量化、PCA降维等数据处理步骤；
- 协助林翔宇完善部分技术段落的描述与图表说明。

### 薛政宇
**主要职责：**
- 负责全篇报告的语言润色、格式校对、图文统一排版；
- 根据指导教师与课程规范要求，检查段落格式、术语使用、图表编号等细节；
- 确保报告最终文档符合学术写作规范，达到作品提交标准。

**主要成果：**
- 全面审阅和修改第一至第五章文字表达及图表命名；
- 对引用格式、代码注释、附录等内容进行整理规范；
- 整合并美化报告结构，使排版清晰、便于阅读；
- 协助小组成员撰写分工说明和最终提交材料。

---

## 一、报告框架生成提示词

```prompt
你是一个舆情分析专家，需要撰写《印巴冲突舆情演化分析》的学术报告。要求包含以下章节：
1. 研究背景与意义（说明南亚地缘政治重要性）
2. 研究目的与内容（数据采集、情感分析、可视化）
3. 技术路径（扣子平台+Python生态工具）
4. 数据获取与预处理（来源+清洗流程）
5. 情感倾向分析（API接口实现）
6. 舆论主体聚类（K-Means算法）
7. 传播路径建模（SIR模型）
8. 研究结论与展望
请生成符合学术规范的完整报告框架，包含各章节三级标题。
## 三、数据采集流程

```markdown
## 2. 数据采集提示词（扣子平台）

```prompt
在扣子平台创建数据采集工作流：
1. 数据源：微博热搜/百度新闻/知乎/腾讯新闻/今日头条
2. 关键词："印巴冲突"+"克什米尔"+"印军"+"巴军"+"停火"
3. 时间范围：2024年5月1日至今
4. 字段提取：评论ID、时间、内容、点赞数
5. 自动去重：基于评论内容哈希值
## 四、情感分析实现

```markdown
## 3. 情感分析代码实现

```python
# 腾讯情感分析API调用
import requests
import hashlib
import time
import random

def get_tencent_sentiment(text, app_id, app_key):
    """
    调用腾讯AI情感分析API
    参数:
        text: 待分析文本
        app_id: 腾讯API应用ID
        app_key: 腾讯API应用密钥
    返回:
        JSON格式的API响应
    """
    url = 'https://api.ai.qq.com/fcgi-bin/nlp/nlp_textpolar'
    params = {
        'app_id': app_id,
        'time_stamp': int(time.time()),
        'nonce_str': ''.join(random.choices('abcdefghijklmnopqrstuvwxyz', k=10)),
        'text': text
    }
    
    # 签名生成
    param_str = '&'.join([f'{k}={v}' for k,v in sorted(params.items())])
    sign_str = f'{param_str}&app_key={app_key}'.encode('utf-8')
    params['sign'] = hashlib.md5(sign_str).hexdigest().upper()
    
    response = requests.post(url, data=params)
    return response.json()

# 示例调用
if __name__ == "__main__":
    # 从环境变量获取密钥
    import os
    APP_ID = os.getenv('TENCENT_APP_ID')
    APP_KEY = os.getenv('TENCENT_APP_KEY')
    
    result = get_tencent_sentiment("印巴冲突令人担忧", APP_ID, APP_KEY)
    print(f"情感极性: {result['data']['polar']}, 置信度: {result['data']['confd']}")
## 五、聚类分析实现

```markdown
## 4. 聚类分析实现

```prompt
你是一个数据科学家，请生成Python代码实现：
1. 使用jieba进行中文分词并移除停用词
2. 通过TF-IDF向量化文本
3. 用轮廓系数确定最佳K值
4. 对K-Means聚类结果进行可视化
要求输出聚类中心的关键词和主体类型判断
## 六、传播建模实现

```markdown
## 5. 传播建模（SIR模型）

```prompt
作为网络分析专家，请：
1. 解释SIR模型在舆情传播中的三个状态：S(易感人群)-I(传播人群)-R(移除人群)
2. 给出Python实现代码，包含参数：
   - β = 0.6 (传播率)
   - γ = 0.1 (恢复率)
   - 初始感染节点=3
3. 绘制传播曲线图并标注峰值日期
## 七、可视化实现

```markdown
## 6. 可视化实现

```prompt
你是一个数据可视化专家，请生成Python代码创建以下图表：
1. 情感分布饼图（负向/中性/正向）
2. 时间序列热度曲线（15天周期）
3. 聚类结果散点图（PCA降维）
4. 传播网络图（使用NetworkX）
要求：统一使用seaborn主题，添加标题和颜色注释
## 八、完整工作流程

```markdown
## 7. 完整工作流程

```mermaid
graph TD
    A[扣子平台数据采集] --> B[数据预处理]
    B --> C[情感分析API调用]
    C --> D[聚类分析]
    D --> E[传播建模]
    E --> F[可视化]
    F --> G[报告生成]
    G --> H[合规性检查]
## 九、最终文档结构

```markdown
## 8. GitHub文档结构
project-root/
│
├── India-Pakistan-Opinion-Analysis.md # 主文档
├── data/
│ ├── raw/ # 原始数据
│ └── processed/ # 处理后的数据
├── scripts/
│ ├── data_collection.py # 扣子平台集成
│ ├── sentiment_analysis.py # 情感分析
│ ├── clustering.py # 聚类分析
│ ├── sir_model.py # 传播建模
│ └── visualization.py # 可视化
├── media/ # 所有图表
│ ├── coze_data_collection.png
│ ├── sentiment_pie.png
│ ├── activity_timeline.png
│ ├── cluster_visualization.png
│ ├── diffusion_network.png
│ └── sir_model.png
├── requirements.txt # 依赖库
└── README.md # 项目说明
