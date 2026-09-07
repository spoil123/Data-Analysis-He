# 电商用户价值分层与精准营销（RFM-I 优化模型）

一个端到端的数据分析项目：在经典 **RFM** 模型基础上，引入 **意向深度（Intent）** 等扩展维度，构建 **RFM-I 优化模型**，对电商用户进行细粒度价值分层，并给出有量化 ROI 支撑的精准营销策略。

本项目完整覆盖数据分析工作流：**数据质量检查 → 特征工程 → 探索性数据分析（EDA）→ 用户分层 → 分层画像 → 营销 ROI 测算**。

---

## 目录

- [项目概述](#项目概述)
- [核心方法：RFM-I 模型](#核心方法rfm-i-模型)
- [技术栈](#技术栈)
- [项目结构](#项目结构)
- [如何运行](#如何运行)
- [关键发现](#关键发现)
- [数据来源与隐私](#数据来源与隐私)
- [License](#license)

---

## 项目概述

用户价值分层是电商营销领域的基础问题。与其对所有用户一视同仁，企业必须回答两个问题：**谁是真正高价值的用户**，以及 **营销预算应该投给谁** 才能获得最大回报。

本项目基于一份标准电商数据集（1000 名用户 × 14 个特征），完成以下工作：

1. 数据质量审计（缺失值、异常值检查）；
2. 构建 **RFM-I 扩展特征集**，在经典 RFM 之外引入用户 *意向深度（Intent）*、*转化摩擦（Friction）*、*活跃连接度（Loyalty）* 与 *购买力水平（Income）* 等维度；
3. 通过探索性数据分析理解用户行为；
4. 将用户细分为具有不同营销价值的人群；
5. 用雷达图和分布柱状图刻画各人群特征画像；
6. 通过 ROI 模拟对比 **传统 RFM 策略** 与 **优化 RFM-I 策略** 的营销效果。

结果表明：优化后的 RFM-I 策略在边际 ROI 上**显著优于传统方案（33.5% vs 4.0%）**，同时花费更少。

---

## 核心方法：RFM-I 模型

### 经典 RFM

| 维度 | 含义 |
| --- | --- |
| **R**（Recency） | 用户最近一次购买的间隔 |
| **F**（Frequency） | 用户购买频率 |
| **M**（Monetary） | 用户累计消费金额 |

### 扩展特征（RFM-I）

为捕捉经典 RFM 遗漏的行为信号，额外构建以下特征：

| 特征 | 含义 | 构造方式 |
| --- | --- | --- |
| **I_Score** | 意向深度——用户参与/购买意向强度 | `0.5 * Time_Spent_Norm + 0.5 * Pages_Viewed_Norm`（min-max 归一化） |
| **Friction** | 转化摩擦——购买路径上的阻力 | `Pages_Viewed / (Purchase_Frequency + 1)` |
| **L_Score** | 活跃连接度 | 基于邮件订阅状态与最近登录间隔的规则打分（1~3 分） |
| **Income_Level** | 购买力背景 | 按收入三分位划分（Low / Medium / High，33% 与 66% 分位） |
| **Interest_Match** | 用户兴趣与购买品类匹配度 | `Interests == Product_Category_Preference` |

将上述维度综合打分后，对用户进行**价值分层**，并为每一类人群匹配针对性的营销策略。

---

## 技术栈

- **Python 3**（Jupyter Notebook 环境）
- **pandas** — 数据加载、清洗与特征工程
- **numpy** — 数值计算
- **matplotlib** — EDA 可视化、雷达图、分布图
- **openpyxl** — 读取 `.xlsx` 输入数据

---

## 项目结构

```
Data-Analysis-He/
├── Rfm_User_Value_Segmentation.ipynb              # 项目主版 Notebook（完整流水线，参考实现）
├── Rfm_User_Value_Segmentation_Handcrafted.ipynb  # 独立手写重实现（交叉验证）
├── data/schema.md                                 # 数据字段字典（数据 schema 说明）
├── figures/                                       # 分析图表输出（EDA / 雷达图 / ROI）
├── Project_Deep_Dive.docx                         # 项目深度解读文档（辅助材料）
├── README.md                                      # 本文件
├── requirements.txt                               # Python 依赖清单
├── .gitignore                                     # 忽略数据 / 临时文件，保护隐私
└── LICENSE                                        # MIT License
```

### 文件说明

| 文件 | 说明 |
| --- | --- |
| `Rfm_User_Value_Segmentation.ipynb` | **主版 Notebook** —— 完整分析流水线：质量检查 → 特征工程 → EDA → 分层 → 画像 → ROI 测算 |
| `Rfm_User_Value_Segmentation_Handcrafted.ipynb` | **手写版 Notebook** —— 对同一方法论的独立从零重实现 |
| `data/schema.md` | 数据字段字典（字段类型、含义、RFM-I 映射关系） |
| `figures/` | 分析图表：EDA 分布、相关性矩阵、分层分布、分层雷达、ROI 对比 |
| `Project_Deep_Dive.docx` | 深度解读文档：业务背景、方法依据与结果说明 |

### 应该看哪个 Notebook？

本仓库**有意提供两份并行的 Notebook**，从不同角度覆盖同一方法论——它们互为补充，而非重复：

| Notebook | 定位 |
| --- | --- |
| `Rfm_User_Value_Segmentation.ipynb` | **主版 / 参考实现**。完整、注释清晰的分析流水线，讲述从质量审计 → 特征工程 → EDA → 分层 → 画像 → ROI 的完整故事。读取 `data/user_personalized_features.xlsx`。 |
| `Rfm_User_Value_Segmentation_Handcrafted.ipynb` | **从零重实现**。不参照主版独立编写，用于交叉验证方法论，并展示可复现的编码能力。读取 `data/user_personalized_features.csv`。 |

两份 Notebook 得出等价的分层结论，彼此印证。想按步骤跟进分析，先从主版开始；想用一份独立干净的实现交叉校验方法，请对照手写版。

---

## 如何运行

### 1. 环境准备

```bash
# （推荐）创建并激活虚拟环境
python -m venv venv
source venv/bin/activate        # Linux/macOS
# venv\Scripts\activate       # Windows

# 安装依赖
pip install -r requirements.txt
```

### 2. 准备数据

> **注意：** 出于隐私保护，原始数据**未包含**在本仓库中（详见 [数据来源与隐私](#数据来源与隐私)）。

| Notebook | 预期数据文件（相对仓库根目录） |
| --- | --- |
| 主版（`Rfm_User_Value_Segmentation.ipynb`） | `data/user_personalized_features.xlsx` |
| 手写版（`Rfm_User_Value_Segmentation_Handcrafted.ipynb`） | `data/user_personalized_features.csv` |

将数据文件放入 `data/` 目录并命名为预期文件名即可——两份 Notebook 均通过**相对路径自动加载，无需修改代码**。预期字段（1000 行 × 14 列）见 [data/schema.md](data/schema.md)：

`User_ID, Age, Gender, Location, Income, Interests, Last_Login_Days_Ago, Purchase_Frequency, Average_Order_Value, Total_Spending, Product_Category_Preference, Time_Spent_on_Site_Minutes, Pages_Viewed, Newsletter_Subscription`

### 3. 运行

在 Jupyter 中打开 `Rfm_User_Value_Segmentation.ipynb` 并依次执行所有单元格，图表将输出到 `figures/` 目录。

---

## 关键发现

1. **数据干净规范。** 数据集（1000 行 × 14 列）无缺失值、无年龄异常值，无需大量清洗即可直接分析。

2. **用户行为差异显著。** I_Score（意向深度）取值 0.00~98.83，Friction（转化摩擦）0.10~49.00，不同用户的购买行为差异明显。

3. **价值分层清晰。** 用户被细分为 15 类人群（核心价值用户 / 潜力用户 / 低价值用户等），每类人群拥有独特的 RFM-I 雷达画像，可针对性施策。

4. **人货匹配存在缺口。** 初始 `Interest_Match`（用户兴趣与购买品类重叠度）仅为约 0%，提示个性化推荐存在巨大优化空间。

5. **优化 RFM-I 策略 ROI 显著优于经典 RFM。** 在 10000 元预算下的模拟投放结果：

   | 策略 | 目标用户数 | 成本 | 边际 ROI |
   | --- | --- | --- | --- |
   | A：传统 RFM 前 20% | 200 | 2000 元 | **4.0%** |
   | B：优化 RFM-I | 159（含 89 名新挖掘的潜力用户） | 1590 元（预算使用率 15.9%） | **33.5%** |

   优化策略**既降低了成本**，又**激活了经典 RFM 遗漏的高潜力用户**。

---

## 数据来源与隐私

- 原始数据文件（`user_personalized_features.xlsx`）及任何原始 CSV 数据**均不上传**本仓库，以保护用户隐私与业务数据；
- 分析中的个人标识均做了**脱敏处理**（用户以 `#1`、`#2`... 形式指代）；
- 如需复现，请使用本地相同字段结构的数据集，或自行生成等价合成数据。

---

## License

本项目采用 [MIT License](LICENSE) 开源许可。Copyright (c) 2026 **He Langjie**。
