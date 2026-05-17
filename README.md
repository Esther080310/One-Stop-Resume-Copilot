# Vibe CV Copilot

这是我为了准备 TikTok AI 产品实习生面试，使用 Trae (Vibe Coding) 从 0 到 1 搭建的【简历生成与面试辅导 Agent】。

## 核心实现
- **基于 JD 的动态特征提取**：跳出传统的静态关键词匹配，根据具体岗位动态提取四字核心技能。
- **Human-in-the-loop (HITL) 人工审核机制**：避免 AI 幻觉，在生成最终文档前通过 Markdown 草稿进行人工校验。
- **防 AI 幻觉的 Word 自动化排版注入**：使用 Python `python-docx` 库直接注入 Word 模板，完美继承所有格式（包括页边距、中英文字体隔离），实现了所见即所得。

## 核心文件指南
- `SKILL.md`: Agent 的核心逻辑与产品工作流（Prompt 规约）。
- `scripts/generate_tiktok_docx.py`: 实现无损注入 Word 的自动化排版脚本。
- `docs/Interview_Mock_QnA_枢宇科技.md`: 针对量化 Agent 实习经历整理的高强度面试 Mock 库，展示对业务场景的深度拆解。
