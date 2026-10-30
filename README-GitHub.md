# GitHub Pages 部署说明（高项备考工作台）

GitHub 和 GitLab 的静态发布方式不同：GitLab 用 `.gitlab-ci.yml`，GitHub 用 **GitHub Actions**。这个包已经带好 `.github/workflows/pages.yml`，不需要自己写 YAML。

## 一、需要准备的资料

```text
GITHUB_USER=你的GitHub用户名
REPO=仓库名（例如 gaoxiang-workbench）
BRANCH=main
TOKEN=github_pat_...  或  ghp_...
```

另外确认：

- [ ] 仓库已创建，默认分支 `main`；
- [ ] 仓库可以 **Public**（免费版 Pages 最省事）；
- [ ] 如果仓库是 Private，GitHub Free 不能开 Pages，需要 Pro/Team/Enterprise；
- [ ] GitHub Actions 已启用；
- [ ] 仓库 **Settings → Pages → Build and deployment → Source** 选择 **GitHub Actions**。

## 二、Token 权限

推荐用 **Fine-grained personal access token**，只选这个仓库：

- Repository access：Only select repositories → 选中该仓库
- Permissions：
  - **Contents: Read and write**（提交文件）
  - **Pages: Read and write**（开启/读取 Pages）
  - **Actions: Read and write**（读取/触发 workflow）
  - **Metadata: Read**（必选）
  - 如果提交 `.github/workflows/pages.yml` 时提示 workflow 权限不足，再加 **Workflows: Read and write**；或改用 classic token。

Classic token 也可以：

- `repo`
- `workflow`

> 不要发 GitHub 登录密码，也不要发 SSH 私钥。Token 用完请立即 revoke。

## 三、上传方式

### 方式 A：我帮你用 API 推送

把上面 4 项发我，我会：

1. 验证仓库和 Token；
2. 用 Git Data API 一次提交 `public/` 和 `.github/workflows/pages.yml`；
3. 检查 Actions 运行状态；
4. 给你 `https://<用户名>.github.io/<仓库名>/` 网址。

### 方式 B：你自己用 Git 推

```bash
cd github-deploy
git init --initial-branch=main
git add .
git commit -m "deploy 高项备考工作台"
git remote add origin https://github.com/<用户名>/<仓库名>.git
git push -u origin main
```

HTTPS 推送时用户名填 GitHub 用户名，密码填 **Token**（不是登录密码）。

## 四、开启 Pages

推送后：

1. 进入仓库 → **Settings → Pages**；
2. **Build and deployment → Source** 选 **GitHub Actions**；
3. 进入 **Actions**，等待 `Deploy 高项备考工作台 to GitHub Pages` 成功；
4. 回到 Pages 页面，访问：

```text
https://<用户名>.github.io/<仓库名>/
```

如果仓库名是 `<用户名>.github.io`，网址就是 `https://<用户名>.github.io/`。

## 五、手机安装

- Android：Chrome / Edge 打开 Pages 网址 → 菜单 → **安装应用 / 添加到主屏幕**；
- iPhone：Safari 打开 → 分享 → **添加到主屏幕**；
- 微信内：只能浮窗/收藏，不支持安装 PWA；
- ZIP 不是 APK/IPA，不能直接安装。

## 六、常见失败

| 现象 | 原因 | 处理 |
|---|---|---|
| Actions 里没有 workflow | `.github/workflows/pages.yml` 没上传 | 确认路径和文件名完全一致 |
| `Get Pages site failed` | Pages 没设为 GitHub Actions | Settings → Pages → Source 选 GitHub Actions |
| `Resource not accessible by integration` | workflow 权限不足 | 仓库 Settings → Actions → General → Workflow permissions 设为 Read and write |
| 404 | Pages 还没部署完 / 仓库私有但无 Pages 权限 | 等 Actions 成功；检查仓库可见性和 Pages 设置 |
| 手机打不开 | 网址是 GitLab/局域网地址 | 用 GitHub Pages 的 `github.io` 网址 |
