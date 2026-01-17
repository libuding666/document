### AI

##### 关键词

Prompt  ReAct (Reason+Act):AI智能体

##### 前沿领域

LLM/Agent/多模态/MLOps方向

1. 核心工具链
   - **LLM框架**: **LangChain** 或 **LlamaIndex** (二选一，LangChain生态更全，LlamaIndex专注RAG)。CrewAI
   - **文本嵌入模型**: 学习使用OpenAI `text-embedding-3-small`或开源模型（如Hugging Face上的Sentence Transformers）。
   - **向量数据库**: 掌握至少一个，如 **ChromaDB** (本地易用) 或 **Pinecone** (云端强大)。
2. **高级技巧**: 学习如何优化切分策略、进行多轮检索、重排序等。



````
Claude Code Templates
 https://github.com/davila7/claude-code-templates
 https://www.aitmpl.com/agents
 
PandaWiki - 开源的在线 AI 知识库 https://pandawiki.docs.baizhi.cloud/
 
简单几行代码 LangChain 帮我们把 split、embedding、入库全部都做了，再看一下 TiDB Vector 中生成的表结构：

```sql
CREATE TABLE `semantic_embeddings` (
`id` varchar(36) NOT NULL,
`embedding` vector<float>(1536) NOT NULL COMMENT 'hnsw(distance=cosine)',
`document` text DEFAULT NULL,
`meta` json DEFAULT NULL,
`create_time` datetime DEFAULT CURRENT_TIMESTAMP,
`update_time` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (`id`) /*T![clustered_index] CLUSTERED */
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin
```

这里做 chunk 用最简单粗暴的形式，每页文档为一个 chunk，chunk 的文本内容存入 `document`字段，向量化后的内容存入

````



#### 名词

就构成了 RAG 的灵魂：检索（Retrieval）、增强（Augmented）、生成（Generation）



##### 教程

Streamlit 极速搭建AI智能对话助手 https://www.bilibili.com/video/BV1w3ddYuETz/

秋芝2046 https://space.bilibili.com/385670211?spm_id_from=333.788.upinfo.detail.click

【隔壁的程序员老王】 原来写一个 AI Agent 这么简单 https://www.bilibili.com/video/BV1UMVKzEESL

AI时代下6大应用方向，分享给在寻找创业赚钱机会的人 https://www.bilibili.com/video/BV122cweVEbB

```
我想做一个ios app，是一个ai自动记账app。
这个app可以随意输入自然语言或者语音输入自己的消费，然后由gemini 2.5 pro模型的ai解析我的语音并且自动分类和帮我存储我的账单。
登录和数据存储都使用supabase。
语音功能则需要买会员，这个会员服务的购买使用stripe。
我希望页面漂亮炫酷，是黑客帝国风格的感觉。

##注意
你只需要完成项目的生成，而不需要执行测试和构建的工作
使用ios18.5,我会用iphone 16pro作为模拟器
gemini、supabase和stripe都已经配置好
除了要求的功能其他尽量保持简单。

用AI开发上线App全流程！全网最全新版TRAE教程 https://www.youtube.com/watch?v=1c5gc7admh8
```

#### RAG

RAG 简介 https://github.com/datawhalechina/all-in-rag/blob/main/docs/chapter1/01_RAG_intro.md

一文彻底搞懂大模型 - RAG（检索、增强、生成） https://blog.csdn.net/a2875254060/article/details/142468037

RAG企业实战：从原理到落地，构建企业智能知识库 https://www.woshipm.com/ai/6323870.html

最全梳理：一文搞懂RAG技术的5种范式！ https://segmentfault.com/a/1190000046138023

![一图看懂RAG 的基本原理](./img/2VHl2lEHORGRYMb3DQZy.webp)

![RAG简单流程与总览](./img/1060453206-390b091220301e5d_fix732.webp)

#### SOP

标准化操作程序（Standard Operating Procedure, SOP） https://www.processon.com/knowledge/sopflowchart



#### 数据可视化

使用Matplotlib、Seaborn、Power BI或Tableau进行数据可视化

人工智能AI学习从零开始 ：详尽路线图+实战案例 https://zhuanlan.zhihu.com/p/654555097

小白变大神！2025年人工智能（AI）初学者学习路线图，轻松从入门到高手！ https://zhuanlan.zhihu.com/p/1919059787930407510

2026年AI学习完整指南：从入门到进阶的12个月通关路线图 https://cloud.tencent.com/developer/article/2610818

PlanetScale https://www.cnblogs.com/vpofeng/articles/19124239

####  Agent Skills
AI 编程工具都在用 Agent Skills https://www.bilibili.com/video/BV1tRr6BAErJ/



需求

实现“AI 搜索花卉并匹配 MySQL 数据库商品”的核心技术路径是 **向量搜索（Vector Search）**。

传统的搜索是靠“关键词”匹配（比如搜“玫瑰”），而 AI 搜索是靠“语义/特征”匹配（比如搜“红色的、代表爱情的花”，或者上传一张花卉图片）。

以下是实现这一功能的四个关键步骤：

------

### 1. 数据预处理：将花卉转化为“向量”

AI 无法直接对比 MySQL 中的文本或图片，必须先通过 **Embedding 模型**（如 OpenAI 的 `text-embedding-3` 或开源的 `CLIP` 模型）将花卉信息转化为一串数字（向量）。

- **操作：** 遍历你数据库中现有的花卉商品，提取它们的名称、描述，甚至图片。
- **转化：** 将这些信息输入 Embedding 模型，得到对应的特征向量（例如一个 512 维的数组）。

### 2. 数据库升级：存储向量

普通的 MySQL 版本不支持直接进行高效的向量运算。你有两种方案：

- **方案 A（推荐）：使用支持向量的 MySQL 扩展**
  - **MySQL 9.0+ / HeatWave：** 原生支持 `VECTOR` 数据类型。
  - **Cloud 服务：** 如阿里云 PolarDB、Google Cloud SQL 或 TiDB，它们都集成了向量搜索能力。
- **方案 B：外挂向量数据库**
  - 如果不想动现有数据库，可以引入专门的向量数据库（如 Milvus, Pinecone, 或 Chroma）。在 MySQL 里存商品基本信息，在向量库里存 `商品ID + 向量`。

### 3. 实现匹配逻辑（核心流程）

当用户输入搜索请求时，流程如下：

1. **用户输入：** 用户输入“适合送给母亲的粉色花”或上传照片。

2. **实时转换：** 使用**同一个** Embedding 模型，将用户的输入转化为一个“查询向量”。

3. **相似度计算：** 在数据库中使用 SQL 语句计算“查询向量”与“库存向量”之间的距离（常用 **Cosine Similarity 余弦相似度**）。

   SQL

   ```
   -- 示例 SQL (以支持向量扩展的 MySQL 为例)
   SELECT id, name, price 
   FROM flowers 
   ORDER BY VECTOR_DISTANCE(embedding_column, [用户查询的向量], 'COSINE') ASC 
   LIMIT 5;
   ```

4. **返回结果：** 数据库会返回最接近的 5 个商品。

### 4. 方案对比表

| **方案**                 | **优点**                   | **缺点**                     | **适用场景**         |
| ------------------------ | -------------------------- | ---------------------------- | -------------------- |
| **原生 MySQL (9.0+)**    | 架构简单，无需同步数据     | 需要升级数据库版本           | 数据量中等，追求稳定 |
| **云数据库 (PolarDB等)** | 性能强，开箱即用           | 有额外的云服务成本           | 企业级应用，快速上线 |
| **MySQL + Milvus**       | 极其适合海量数据（千万级） | 系统复杂度增加，需维护两个库 | 大规模电商平台       |

使用 TiDB Vector 搭建 RAG 应用 https://tidb.net/blog/7a8862d5

polardbx https://doc.polardbx.com/zh/quickstart/topics/quickstart.html#%E5%9C%A8-ubuntu-%E4%B8%8A%E5%87%86%E5%A4%87%E6%B5%8B%E8%AF%95%E7%8E%AF%E5%A2%83

需要配置三个模型：

**嵌入模型（Embedding Model）：**通过对文本的语义理解，将文字信息映射到向量空间中，使语义相似的文本在向量空间中的距离更近。例如，“如何预防感冒？” 和 “感冒的预防方法有哪些？” 会被转化为距离较近的向量，从而为后续的检索匹配提供基础。
**重排模型（Reranker Model）：**基于文本与问题的语义相关性、信息完整性、准确性等深层特征，重新调整候选文本的顺序，过滤噪音信息（如无关内容、低质量回答），确保更相关的文本被优先送入生成模型。例如，在检索 “机器学习算法” 时，重排模型会将详细讲解算法原理的文档排在泛泛而谈的内容之前。
**文本模型（生成模型，如 LLM）：**基于检索到的相关信息和用户问题，生成最终的回答。
RAG 通过嵌入模型将用户问题与知识库文本转化为语义向量并检索相关信息，经重排模型优化排序后，由文本模型结合检索结果生成准确且有依据的回答，实现 “检索增强生成” 的闭环。

这里我们发现，默认的嵌入模型和重排模型已经帮我们配置好了，这里应该是 “百智云” 给这个产品提供了赞助，默认提供了一些免费额度。不过嵌入模型和重排模型其实在整个 RAG 检索的过程中 Token 消耗量是非常小的，所以这里我们用默认的免费额度就可以用非常久了。


