---
layout: default
title: 项目总结：make-knowledge-cards
permalink: /projects/make-knowledge-cards/
---

<style>
  .page-header {
    background: linear-gradient(120deg, #ffe4ec, #ffb6c1) !important;
  }
  .page-header .project-name {
    color: #d6336c !important;
  }
  .page-header .project-tagline {
    color: #ffe4ec !important;
  }
</style>

# 项目总结：make-knowledge-cards

## 项目概述

**项目名称**：make-knowledge-cards

**一句话介绍**：一个将文章自动转换为结构化知识卡片的命令行工具。

**技术栈**：Python + DeepSeek API + Claude（AI 辅助编程）

**项目地址**：[github.com/mlele7582-code/make-knowledge-cards](https://github.com/mlele7582-code/make-knowledge-cards)

**PyPI 安装**：`pip install make-knowledge-cards`

## 一、项目背景与需求

### 为什么做这个项目？

- 需要一个把长文章快速提炼成知识卡片的工具
- 想学习完整的开源项目开发流程
- 验证"用 AI 辅助编程"的可行性

### 核心功能需求

| 功能 | 描述 |
|------|------|
| 输入方式 | 粘贴文本 或 读取本地 `.md`/`.txt` 文件 |
| 输出内容 | 5-8 张知识卡片，每张含标题、核心知识、解释、例子/问题 |
| 处理规则 | 提取真正重要的知识，一卡一点，删除重复，不编造信息 |
| 支持模式 | AI 模式（DeepSeek API）和简单模式（无 API 时） |

### 不支持的功能（暂定）

- 网页抓取
- PDF 文件
- Anki 导出
- 图形界面

## 二、技术选型与决策

| 组件 | 选择 | 原因 |
|------|------|------|
| 编程语言 | Python | 简单易学，适合新手 |
| AI API | DeepSeek | 便宜（1元/百万Token），兼容 OpenAI 接口，新用户有免费额度 |
| AI 辅助编程 | Claude | 全程参与代码编写、调试和优化 |
| 包管理 | PyPI | Python 官方仓库，用户一键安装 |
| 版本控制 | GitHub | 开源标准平台 |
| 主题系统 | Jekyll | GitHub Pages 原生支持 |

## 三、开发过程

### 阶段一：项目初始化

在桌面创建 `First-skill` 文件夹，通过 Claude 生成初始代码结构。

### 阶段二：遇到第一个问题——标题太差

**问题**：`--no-ai` 模式下，标题是直接截取的句子片段，如：

- ❌ "抗内卷率=抗内卷的人数-总人数"
- ❌ "实际生活中可能是越靠右边就越抗…"

**解决**：让 Claude 修改 `extractor.py` 中的标题生成逻辑，使用关键词组合 + Markdown 标题提取，生成更有意义的标题。

**效果**：

- ✅ "内卷函数的定义"
- ✅ "可替代性与内卷的关系"
- ✅ "强制跨维度比较的危害"

### 阶段三：集成 DeepSeek API

切换到 AI 模式后，质量有了质的飞跃：

| 对比 | 简单模式（--no-ai） | AI 模式（DeepSeek） |
|------|---------------------|---------------------|
| 标题 | 截取的句子片段 ❌ | 精炼的知识概括 ✅ |
| 解释 | 空白的 ❌ | 详细易懂 ✅ |
| 例子 | 通用问题 ❌ | 针对性设计 ✅ |

### 阶段四：支持 .env 文件

让项目自动读取 `.env` 文件中的 API Key，用户不需要每次手动设置环境变量。

```env
OPENAI_API_KEY=sk-xxx
OPENAI_API_BASE=https://api.deepseek.com/v1
OPENAI_MODEL=deepseek-chat
```

### 阶段五：解决依赖和版本问题

- 用 `python -m pip` 代替 `pip`（Windows PowerShell 兼容问题）
- 更新 `pyproject.toml` 版本号才能让 PyPI 更新
- 用本地开发模式 `pip install -e .` 让命令立即生效

## 四、发布流程

### 步骤一：发布到 GitHub

```bash
git init
git add .
git commit -m "feat: 初始版本"
git remote add origin https://github.com/mlele7582-code/make-knowledge-cards.git
git push -u origin main
```

### 步骤二：发布到 PyPI

1. 创建 `pyproject.toml`
2. 注册 PyPI 账号 + 开启两步验证（Google Authenticator）
3. 生成 API Token
4. 构建并上传

```bash
python -m build
python -m twine upload dist/*
```

### 步骤三：用户安装使用

```bash
pip install make-knowledge-cards
knowledge-cards --file 文章.md
```

## 五、踩坑记录

### 坑 1：pip 找不到命令

| 错误 | 原因 | 解决 |
|------|------|------|
| `pip: The term 'pip' is not recognized` | PowerShell 没有将 pip 加入 PATH | 用 `python -m pip install xxx` |

### 坑 2：PyPI 两步验证

| 错误 | 原因 | 解决 |
|------|------|------|
| 要求扫描二维码 | PyPI 强制开启 2FA | 下载 Google Authenticator 绑定 |

### 坑 3：标题生成不理想

| 错误 | 原因 | 解决 |
|------|------|------|
| 标题是截断的句子 | 简单模式只做了字符串截取 | 让 Claude 改代码，用关键词组合生成 |

### 坑 4：版本号没更新

| 错误 | 原因 | 解决 |
|------|------|------|
| `knowledge-cards` 命令还是旧版本 | 修改代码后没改 `pyproject.toml` 的 `version` | 更新版本号，重新构建上传 |

### 坑 5：命令名记错

| 错误 | 原因 | 解决 |
|------|------|------|
| `knowledge_cards` 报错 | 命令是短横线不是下划线 | 正确命令是 `knowledge-cards` |

### 坑 6：环境变量每次都要重设

| 错误 | 原因 | 解决 |
|------|------|------|
| 每次新窗口都要设 API Key | 没有持久化环境变量 | 用 `python-dotenv` 读取 `.env` 文件 |

### 坑 7：Git 推送冲突

| 错误 | 原因 | 解决 |
|------|------|------|
| `failed to push` 或 `non-fast-forward` | 远程有本地没有的更新 | `git pull` → 解决冲突 → `git push` |

### 坑 8：文件编码问题

| 错误 | 原因 | 解决 |
|------|------|------|
| `type index.md` 显示乱码 | PowerShell 默认编码不是 UTF-8 | 在 GitHub 网页编辑保存，或指定 UTF-8 编码 |

### 坑 9：反斜杠导致页面异常

| 错误 | 原因 | 解决 |
|------|------|------|
| Markdown 符号显示为文本 | 复制内容时格式被转义（`\##` 而不是 `##`） | 用纯文本重新粘贴，去除反斜杠 |

### 坑 10：GitHub Pages 不渲染

| 错误 | 原因 | 解决 |
|------|------|------|
| 网站显示 404 或源码 | 存在 `.nojekyll` 文件 | 删除 `.nojekyll`，让 Jekyll 正常构建 |

## 六、最终成果

### 安装与使用

```bash
# 安装
pip install make-knowledge-cards

# 配置 API Key（在 .env 文件中）
OPENAI_API_KEY=sk-xxx
OPENAI_API_BASE=https://api.deepseek.com/v1
OPENAI_MODEL=deepseek-chat

# 运行
knowledge-cards --file 文章.md
```

### 运行效果

```
==================================================
  📚 知识卡片生成器
==================================================

📄 已读取文件：test-article.txt（5666 个字符）
🤖 正在使用 AI 模型提取知识点...（模型：deepseek-chat）

✅ 成功提取 5 张知识卡片：
   1. 内卷函数的定义
   2. 可替代性与内卷的关系
   3. 强制跨维度比较的危害
   4. 内卷的长期后果：劝退效应
   5. 抗内卷率与维度提升

🎉 完成！
```

### 生成的卡片示例

```markdown
## 卡片 1：内卷函数的定义

**核心知识**
内卷是指投入增加但收益递减的现象。

**解释**
当所有人都在同一个维度上竞争时，每个人需要投入更多才能保持现有位置，但总体收益并没有增加。

**自测问题**
你能举一个生活中内卷的例子吗？
```

## 七、心得体会

### 1. AI 是很好的编程助手
不需要从零开始写代码，学会**提出好问题**比写代码更重要。

### 2. 先跑通流程，再追求完美
第一版不完美很正常，先让项目能跑起来，然后迭代优化。

### 3. 开源不只是写代码
还包括：README、LICENSE、发布流程、用户文档。但这些都不难。

### 4. 踩坑是正常的
每一个报错都是一次学习机会。记录下来，下次就懂了。

### 5. 从用户角度思考
最终用户不关心你用了什么技术，只关心能不能解决问题。

### 6. 版本控制很重要
Git 回退功能救了网站好几次。

## 八、项目链接

- **GitHub**：[github.com/mlele7582-code/make-knowledge-cards](https://github.com/mlele7582-code/make-knowledge-cards)
- **PyPI**：[pypi.org/project/make-knowledge-cards/](https://pypi.org/project/make-knowledge-cards/)
- **个人网站**：[mlele7582-code.github.io](https://mlele7582-code.github.io/)

## 九、后续计划

- [ ] 支持 PDF 文件
- [ ] 支持网页抓取
- [ ] 支持 Anki 导出
- [ ] 做成 Web 应用（Streamlit）
- [ ] 支持更多国产大模型（通义、智谱等）
- [ ] 写一篇完整的技术博客

## 十、致谢

- **Claude**：全程参与代码编写、调试和优化
- **DeepSeek**：提供便宜好用的 API
- **PyPI**：让发布变得简单
- **GitHub**：让开源成为可能
- **自己的耐心**：熬过了无数个报错

---

*这是第一个开源项目，但一定不是最后一个。* 🚀
