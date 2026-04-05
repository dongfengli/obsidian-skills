# AGENTS.md

## Repository Structure

这是一个简单的技能库项目，包含多个独立的 `SKILL.md` 文件。没有构建系统、测试或 lint 配置。

- **skills/** — 所有技能目录，每个子目录包含一个 `SKILL.md`
- **skills/*/references/** — 可选的参考文档目录

## Skill 格式

每个 skill 是一个独立的功能单元，遵循 [Agent Skills 规范](https://agentskills.io/specification)。每个 skill 目录：
- 必须包含 `SKILL.md` 文件
- 可选包含 `references/` 子目录存放参考文档

## 创建或修改 Skill

1. 在 `skills/` 下创建新目录（如 `skills/my-skill/`）
2. 创建 `SKILL.md` 文件，遵循现有 skill 的结构和风格
3. 如有参考文档，放在 `references/` 子目录

## 提交规范

- 使用清晰简洁的提交信息，如 `Add web-to-obsidian skill` 或 `Update defuddle documentation`
- 推送到 `main` 分支