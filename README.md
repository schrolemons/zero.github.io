# ZERO Hexo 源码

本仓库的 `source` 分支保存 Hexo 源码，包括文章、配置、主题、脚手架和依赖清单。`main` 分支保存 Hexo 生成的网站，由原有的 `hexo d` 流程维护。

## 更新源码

在项目根目录执行，保持当前分支为 `source`：

```powershell
git add -A
git commit -m "更新源码"
git push origin source
```

`git push` 只推送已提交的修改，因此修改文件后需要先执行 `git add` 和 `git commit`。

## 发布网站

继续使用原来的 Hexo 命令。例如，重新生成并发布：

```powershell
hexo g
hexo d
```

根目录的 Git 仓库负责源码；Hexo 使用独立的 `.deploy_git` 仓库将生成内容推送到 `_config.yml` 中配置的 `main` 分支。发布网站时保持根目录在 `source` 分支即可，无需切换到 `main`，也无需合并这两个分支。

源码推送与网站发布分别执行：推送 `source` 不会发布网站，`hexo d` 不会备份源码。

`source` 分支根目录的 `vercel.json` 使用 `git.deploymentEnabled.source: false`，禁止 Vercel 的 Git 集成为该分支自动创建预览部署。此配置应保留在源码分支；`main` 的网站发布仍由原有 Hexo 流程维护。配置依据：[Vercel Git Configuration](https://vercel.com/docs/project-configuration/git-configuration#gitdeploymentenabled)。

## 从源码恢复

```powershell
git clone --branch source --single-branch git@github.com:schrolemons/zero.github.io.git zero
cd zero
npm ci
npx hexo g
```

主题以完整文件保存，包含当前的本地修改，克隆后无需额外初始化子模块。`node_modules`、`public`、`.deploy_git`、`db.json` 和本地编辑器缓存不纳入源码版本管理。
