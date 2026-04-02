帮我生成一个极简个人投资分析系统，必须严格满足以下全部需求，不得多余功能：

整体架构极简：项目只有 agents/、data/、skills/ 三个文件夹 + .env + main.py + dashboard.py + config.py，总代码量尽量控制在 300 行以内，使用经典 CrewAI 写法，不要用 @CrewBase 装饰器。
Agent 定义全部走 Markdown：
所有 Agent（包括主 Manager）都在 agents/ 文件夹下用 .md 文件定义。
新建 Agent 只需要在 agents/ 新增一个 .md 文件，系统必须自动扫描并动态加载（不能硬编码任何 Agent 名称）。
frontmatter 中必须包含：name、role、goal、backstory、skills、github_claude_skills、llm_model。

模型配置：
使用 CrewAI 原生 LLM（from crewai import LLM）。
通过 .env 文件设置全局默认 LLM_MODEL（格式为 provider/model，例如 anthropic/claude-3-5-sonnet-20241022、xai/grok-3）。
每个 Agent 的 .md 文件中可用 llm_model 字段单独覆盖模型。
API Key 全部走环境变量（ANTHROPIC_API_KEY、OPENAI_API_KEY、GROK_API_KEY）。

Skill 支持：
复用我现有的 skill（放在 skills.py 或 skills/，直接 from skills import *）。
支持 github_claude_skills：frontmatter 里可填 GitHub claw-skill / claude-skill 仓库地址，系统自动 git clone 到 ./skills/github/ 并加载。

分析逻辑：
从 data/portfolio.json（持仓列表）和 data/watchlist.json（观察列表）读取股票。
观察列表股票：每个只给出简单结论（买/卖/观察）。
持仓列表股票：每个必须给出短期（1-7天）、中期（1-3月）、长期（3-12月）综合分析。
子 Agent 包括：Researcher、Fundamental Analyst、Sentiment Analyst、Report Writer（至少 4 个子 Agent，可动态扩展）。

主 Agent：必须有一个 Chief Analyst（主 Manager Agent），使用 Process.hierarchical，负责汇总所有子 Agent 输出，给出今日整体总结（明确区分 portfolio 和 watchlist）。
可观测性：Crew 必须设置 verbose=2，实时打印每个子 Agent 和主 Agent 的完整思考、工具调用、输出。
Web 看板：提供 dashboard.py 使用 Streamlit，实现：
“开始今日全量分析”按钮
实时显示整体进展和日志
自动读取 data/ 下所有 JSON，展示每日持股分析汇总表格和最终总结报告。

数据存储：全部只用 JSON 和 MD 文件，绝不使用任何数据库。每日分析结果保存为 data/{today}_summary.json。

请严格按照以下步骤输出完整可直接运行的代码（全部用中文注释）：

项目完整目录结构
.env 示例文件
config.py（CrewAI 原生 LLM 工厂函数）
skills.py（占位，复用现有 skill）
agents/ 目录下至少 5 个 .md 示例文件（包含 chief_analyst.md，并演示 llm_model 和 github_claude_skills）
main.py（包含动态 MD 加载、GitHub Skill clone、区分 portfolio/watchlist、hierarchical Crew、结果保存）
dashboard.py（带实时进展的 Streamlit 看板）
requirements.txt
data/portfolio.json 和 data/watchlist.json 示例

代码必须能直接复制到本地运行，保持极简、干净、易维护。直接开始输出，不要加任何解释。