# claude-code-project-workflows

Claude Code 项目开发工作流 Skill 集合，提供从需求到交付的完整开发流水线。

## 工作流程

```
/requirements-discuss → /requirements-review → /arch-design → /arch-review → /dev-plan → /dev-plan-review → /dev-execute
```

## Skill 列表

| Skill | 说明 | 产出文档 |
|-------|------|----------|
| `/requirements-discuss` | 与用户多轮对话探讨需求，逐步明确功能点和验收标准 | `project_plan/requirements.md` |
| `/requirements-review` | 百分制审核需求文档（90 分通过），检测反模式并评估成熟度 | 审核报告 + 文档状态更新 |
| `/arch-design [--lite]` | 基于需求设计项目架构。默认完整模式（架构风格、技术栈、模块、数据、接口、部署、风险、ADR）；`--lite` 轻量模式仅产出技术栈+模块+安全门控，适合小项目 | `project_plan/architecture.md` |
| `/arch-review` | 百分制审核架构文档（完整模式 85 分 / 轻量模式 70 分通过），检查需求覆盖、架构合理性、反模式等 | 审核报告 + 文档状态更新 |
| `/dev-plan` | 基于需求和架构文档制定分阶段开发方案，拆分 S/M 粒度工作项 | `project_plan/development-plan.md` |
| `/dev-plan-review` | 百分制审核开发方案（85 分通过），检查工作项粒度、测试交织、集成门控等 | 审核报告 + 文档状态更新 |
| `/dev-execute [--auto]` | 按阶段逐工作项执行开发，通过安全门控和集成门控验证 | 代码 + `project_plan/learnings.md` |

### 旁路 Skill（不在主流水线内）

| Skill | 说明 | 适用场景 |
|-------|------|----------|
| `/test-write [目标路径]` | 为已有代码补齐 Smoke + 异常/边界测试，遵循与主流水线相同的 6 类异常场景框架 | 遗留代码补测、紧急 patch 补测、覆盖率提升 |
| `/bug-fix [bug 描述]` | 规范化修 bug：复现 → 定位根因 → 先写失败的回归测试 → 修复 → 跑全量测试 → 记录 learnings → 提交 | 交付后手工测发现的 bug、用户报告问题、线上问题修复 |

## 核心机制

- **测试交织**：每个功能工作项后紧跟 E2E 测试，不堆积到阶段末尾
- **集成门控**：每 3-5 个工作项插入一个集成验证点
- **安全门控**：每个工作项完成后必须通过 lint/check/test
- **工作项粒度**：仅允许 S/M，禁止 L/XL
- **需求成熟度**：RS0-RS5 六级分级，P0 需求须达 RS3+
- **一次一个问题**：讨论环节每次只问一个问题，避免信息过载
- **分支保护**：执行开发时自动创建 `dev/<项目名>` 分支，保持主分支干净
- **自动模式**：`/dev-execute --auto` 一次确认后全自动完成所有阶段，每阶段自动提交
- **架构轻量模式**：`/arch-design --lite` 跳过 ADR/部署/风险矩阵等重型章节，小项目不被流程拖累

## 安装

```bash
# 克隆到 ~/.claude/skills/ 目录
cd ~/.claude/skills/
git clone <repo-url> claude-code-project-workflows

# 安装符号链接
cd claude-code-project-workflows
./manage.sh install
```

## 管理

```bash
./manage.sh install    # 安装（创建符号链接）
./manage.sh uninstall  # 卸载（删除符号链接）
./manage.sh update     # 更新（重建符号链接）
./manage.sh status     # 查看安装状态
```
