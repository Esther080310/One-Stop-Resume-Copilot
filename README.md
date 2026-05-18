# Vibe CV Copilot: 你的专属 Headhunter 级简历智能体

> 本项目通过 Trae (Vibe Coding) 从 0 到 1 搭建，致力于解决 **“海投简历修改繁琐、排版易碎、面试经不起深挖”**的求职痛点。

## 🎯 痛点与破局：为什么 Vibe CV Copilot 值得你下载？

市面上已经有很多一键生成简历的 AI 工具，但对于真正需要**硬核排版**和**深度定制**的求职者来说，它们往往存在以下致命缺陷：

- ❌ **痛点 1：排版易碎 (Format Fragility)**
  *现象*：AI 网页版生成的简历排版死板，一旦想微调（加粗一个词、缩小一号字体），格式立刻崩盘；而 LaTeX 门槛过高，改一点就报错。
  *破局*：**无损 Docx 模板注入**。我们放弃了繁琐的 LaTeX，利用 Python (`python-docx`) 直接在底层修改 XML 文本节点。这意味着，你可以用你**手动精调过页边距和字体的完美 Word 简历**作为底稿（**不论是什么排版风格，只要是你喜欢的任意 `.docx` 文件都可以**），AI 只做精准的符合底稿格式要求的“填词游戏”，实现真正的所见即所得。

- ❌ **痛点 2：AI 幻觉与敷衍填词 (AI Hallucination)**
  *现象*：为了迎合 JD（岗位描述），AI 经常“无中生有”或者使用诸如“数据工程：为了夯实底层数据基座”这类空洞的废话修饰语。
  *破局*：**Human-in-the-loop (HITL) 人工审核闭环**。AI 重构后**不会**立刻生成最终简历，而是先在 IDE 中生成一份双栏对比的 Markdown 草稿 (`review_draft.md`)。AI 暂停运行，等待你手动删改确认后，再一键注入 Word。人类把控业务常识，AI 负责遣词造句。

- ❌ **痛点 3：面试见光死 (Shallow Keyword Stuffing)**
  *现象*：简历改得很漂亮，但面试官一问底层逻辑，立刻原形毕露，因为 AI 只是在表面堆砌关键词，而项目结束时间久远到面试时已经忘记了很多细节。
  *破局*：**全知视角与 Mock QnA 库**。本 Agent 并非只做文字润色，你可以把它挂载在你的每个本地项目代码库中，专门精修该项目经历。遇到关键数据时，它会查阅历史实验报告，甚至重跑脚本以还原真实数据。同时，它会自动为你提炼**简历钩子 (Hook)**，并生成【面试 Mock 问答库】，助你应对面试官的连环追问以及面试后复盘。

- ❌ **痛点 4：海投混乱，投后即忘 (Application Tracking Mess)**
  *现象*：海投了十几家公司，根本记不清哪家投了哪个岗位，用了哪版简历，甚至面试前连原始 JD 都找不到了。
  *破局*：**自动项目管理 (Tracker Integration)**。Agent 工作流强制绑定了 `Job_Application_Tracker.xlsx`。每次为你生成专属简历后，AI 会自动将目标公司、投递岗位、核心关键词、完整 JD 链接以及投递状态（甚至用哪份特定命名的简历草稿）自动登记到 Excel 表格中，实现求职全生命周期的自动化项目管理。

- ❌ **痛点 5：0 经验，连写在简历上的项目都没有 (0-to-1 Project Hatching)**
  *现象*：很多“鼠鼠”面临的终极困境是：连个像样的项目都没有，简历排版再好也没用。
  *破局*：**无中生有的项目孵化**。输入目标 JD，如果你没有项目经历，可以请 Agent 从 GitHub 筛选 2-3 个技术栈高度匹配、可跑通的开源项目。更绝的是，它会为你规划“防撞车改造点”（如加缓存、换微服务），让你在 1 天内“吃透”并魔改出一个高含金量的简历项目。

- ❌ **痛点 6：实习/项目做完了，不知道怎么写 (1-to-10 Retrospective)**
  *现象*：实习结束只剩下一堆代码和周报，不知道怎么把琐碎的业务逻辑包装成简历上的高价值成果。
  *破局*：**沉浸式成果榨取**。把你的代码仓库或工作周报扔给 Agent，它会自动扫描数据流和工程难点，降维提炼成 HR 视角的 STAR 法则要点，并反向生成面试官可能追问的“考点武器库”。

## ⚙️ 核心工作流 (The 9-Step Pipeline)

1. **输入阶段**：读取你的原始经历库 (`resources/projects/`) 和精调过的 Word 底稿。
2. **JD 拆解**：不仅提取关键词，而是动态提取适合该岗位的**四字小标题**（如：生态构建、算法协同）。
3. **XYZ 重构**：使用 XYZ/STAR 法则重写项目，优先展示数字与英文（视觉优先级原则：数字 >英文 > 中文），末尾不加句号。
4. **人工在环 (HITL)**：生成 `review_draft.md`，等待你的审批。
5. **无损注入**：执行 Python 脚本，将确认后的内容无损注入 `.docx` 底稿，自动重命名并输出到 `output/`。
6. **求职信生成 (Cover Letter)**：简历落定后，根据投递邮箱场景，自动生成去 AI 味、纯文本排版的高定版邮件正文。

## 📂 核心文件与目录指南

本开源仓库剥离了个人隐私信息，保留了最核心的产品逻辑框架：

```text
Trae-Vibe-CV-Copilot/
  ├── SKILL.md                  # 整个 Agent 的大脑 (Prompt 规约与工作哲学)
  ├── Job_Application_Tracker.xlsx # 求职投递进度追踪与项目管理表
  ├── README.md                 # 项目介绍
  ├── CHANGELOG_AND_CREDITS.md  # 版本迭代记录与开源项目鸣谢
  ├── scripts/                  
  │   └── generate_tiktok_docx.py # 执行手臂：无损注入 Word 的自动化排版脚本
  ├── Career_Knowledge_Base/    
  │   ├── Interview_Mock_QnA_Example.md # 面试武器库：简历钩子拆解与深挖框架
  │   └── Cover_Letters_and_Pitches.md  # 投递求职信：高定版邮件正文与话术
  ├── resources/                # 占位目录：存放原始简历素材与 Word 底稿
  ├── work/                     # 占位目录：存放 HITL 人工审核 Markdown 草稿
  └── output/                   # 占位目录：存放最终生成的 Docx 简历
```

## 🚀 Get Started (如何使用)

1. **Clone 本仓库到本地**
   ```bash
   git clone https://github.com/你的用户名/Trae-Vibe-CV-Copilot.git
   cd Trae-Vibe-CV-Copilot
   ```

2. **准备你的个人素材**
   在 `resources/` 目录下：
   - 放入你手动排版好的 Word 简历底稿（命名为 `样板范例.docx`，如果你习惯用 LaTeX 当然也支持，只需调整 `SKILL.md` 的输出方式即可）。
   - 放入你的个人背景说明文件（如 `self_profile.md`）和过往项目笔记/Markdown 报告（放入 `projects/` 文件夹）。
   - **（可选进阶）**在 `Career_Knowledge_Base/` 目录下，你可以随时丢入平时看小红书博主分享、听招聘会宣讲得到的关于某公司/某岗位的笔记，AI 会自动将其吸收为行业认知库，在后续写简历和面试讲稿时为你提供精准的“行业黑话”。

3. **激活 Agent**
   - 将 `SKILL.md` 的全部内容复制到你的 IDE (推荐使用 Trae 或 Cursor) 的**系统提示词 (System Prompt)** 或自定义 Skill/Agent 设置中。
   - 在 IDE 中打开本项目的根目录。

4. **开始与 AI 共舞**
   - 直接在对话框中扔入你想投递的 JD（岗位描述），告诉 AI：“我要投递这个岗位，请帮我走一遍 Vibe CV Copilot 工作流”。
   - 接下来，只需根据提示完成 Human-in-the-loop 的 Markdown 确认，静待完美的 Word 简历与面试题库生成。

---

## 🌟 欢迎 Star & 共创鸣谢 (Credits & License)

如果这个工作流帮助你拿到了满意的 Offer，或者给你带来了灵感，请不要吝啬右上角的 **Star ⭐️**！这对我非常重要。

本项目会根据大模型能力的进化和实际面试反馈**持续迭代更新**，欢迎 Watch 关注最新动态。

### 🙏 特别鸣谢 (Acknowledgments)
本项目的诞生并非闭门造车，而是站在了开源社区诸多优秀前辈的肩膀上。在探索如何打造“最强简历 Agent”的过程中，我大量学习并吸收了以下开源项目的优秀设计理念（如 HITL 人工审核、0-to-1 项目孵化、1-to-10 实习复盘等）：

- [Li-Baichuan-James/cv-skill](https://github.com/Li-Baichuan-James/cv-skill) - 提供了防幻觉的 Claim-Source Map 设计理念
- [spontaneousai/job-hunt-copilot](https://github.com/spontaneousai/job-hunt-copilot) - 提供了 JD 动态匹配与素材库架构思路
- [LiuMengxuan04/shushu-internship-tool](https://github.com/LiuMengxuan04/shushu-internship-tool) - 提供了 0-to-1 项目孵化与防撞车改造理念
- [Sunanzhe2004/shushu-internship-resume-optimizer](https://github.com/Sunanzhe2004/shushu-internship-resume-optimizer) - 提供了 1-to-10 实习深度复盘工作流思路

> **详细的版本迭代历史与思路吸收过程，请参阅：[`CHANGELOG_AND_CREDITS.md`](./CHANGELOG_AND_CREDITS.md)**

### 📄 开源声明与共创
- **License**: 本项目采用 **CC BY-NC-SA 4.0 (知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议)** 开源。
  - **非商业性使用**：**严禁**任何人将本项目的代码、Prompt 逻辑或架构用于任何商业变现行为（如打包售卖、付费简历辅导等）。
  - **署名**：如果您在个人开源项目中使用了本项目的核心 Prompt（`SKILL.md`）或工作流架构，**必须在您的 README 中显著位置保留本项目的链接出处**。
  - **相同方式共享**：如果您基于本项目进行了修改或二次创作，您的衍生项目也必须采用相同的 CC BY-NC-SA 4.0 协议开源。

---

## 👩‍💻 关于作者

我是 **欣欣**，一名从文科生转专业到985统计学的 AI 产品探索者，GPA 3.9/4.0。
我热衷于分享统计学干货、效率工具测评，以及探讨如何利用最优的 AI 工作流解决复杂任务。

欢迎关注我的同名全网社交账号，一起交流成长：
自来卷的欣欣学姐 (ID: reliableXinxin)

> *“我相信，更多人类会从执行者变成设计规约与验证体系的控制工程师。与 AI 共舞，才是未来的解法。”*
