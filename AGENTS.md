# AGENTS.md

AI 代理在此仓库中提交代码时的行为规范。

## 语言规则

- 用户交互使用简体中文。
- 代码中注释、变量名、日志打印使用英语。
- Git 提交消息、分支名使用英语。

## 提交规范

### 提交信息格式

```
<type>(<scope>): <简短一句话描述>

<详细描述（可选）>

Signed-off-by: YuhengLi <expliyh@outlook.com>
```

- type: `feat` / `fix` / `docs` / `ci` / `chore` / `refactor` / `test`
- 提交时必须使用 `git commit -s` 添加 Signed-off-by。
- 遵循 [Conventional Commits](https://www.conventionalcommits.org/)。

### 分支命名

- 功能分支: `feat/<描述>`
- 修复分支: `fix/<描述>`

### 仓库 Fork 关系

- origin: `https://github.com/expliyh/cc-switch.git`（用户自己的 fork）
- upstream: `https://github.com/farion1231/cc-switch.git`（官方仓库）

### 分支策略

- **`main`**：仅用于同步上游，与 `upstream/main` 保持一致。**不要直接在 `main` 上做任何改动。**
- **`rel`**：所有 fork 自己的修改都在此分支上开发，包括功能、修复、CI 配置等。
### Tag 命名规范

- 同步上游版本时：`v<上游版本号>.0-expliyh`，例如 upstream `v3.18.0` → `v3.18.0.0-expliyh`。末尾的 `.0` 是在原版本号基础上追加的一位，作为 fork 自己迭代的起点。
- 自己开发的版本默认是预发布（rc），调试迭代时推送 `v<版本号>-rcX`，如 `v3.18.0.1-expliyh-rc1`、`v3.18.0.1-expliyh-rc2`。
- 用户确认达到要求后，才推送去掉 `-rcX` 的正式 tag，如 `v3.18.0.1-expliyh`。
- 版本号累加：在同步上游产生的 `.0` 基础上递增，正式版和 rc 版共享同一个版本号（只是有无 `-rcX` 后缀的区别）。
- Release 由推送 `v*` tag 触发。

## CI / Release

- Release workflow 由 `v*` tag 触发（`.github/workflows/release.yml`）。
- 若未配置 `TAURI_SIGNING_PRIVATE_KEY` secret，CI 自动跳过签名。
- `latest.json` 通过 `$GITHUB_REPOSITORY` 自动发布到对应仓库。

## 代码格式化修复

- 格式化/注释/简单重命名等机械性修复应当 **amend** 进对应的修改提交中（`git commit --amend`），保持提交历史整洁。
- 如果对应修改来自 upstream（即不是用户自己的改动），则**不需要** amend，因为这样会造成与 upstream 的冲突。
- 判断方法：`git log --format="%ae" <commit>` 查看作者邮箱，或者检查改动是否在 upstream/HEAD 中已存在。

## 禁止事项

- 不要尝试读取图片/截图文件（PNG、JPG、WebP 等），会导致错误。
- 不要在提交中包含私钥文件（`src-tauri/private.key`、`src-tauri/private.key.pub` 已在 `.gitignore` 中）。
- 不要修改 `src-tauri/tauri.conf.json` 中的 `pubkey`，除非同时生成了新的密钥对并更新了 GitHub secret。
