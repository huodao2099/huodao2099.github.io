# 博客搭建与部署指南

本指南帮助你把这套 Hugo 博客完整跑起来，实现 **免费 + 大厂背书 + 稳定运行10年以上** 的目标。

## 一、整体架构

```
你在手机/电脑写 Markdown
        │  git push
        ▼
GitHub 仓库 (源文件 + 图片)
        │  GitHub Actions 自动构建
        ▼
GitHub Pages (静态站点，绑定专属域名)
        │
        ├── 读者访问 yourname.com
        ├── RSS 订阅 (豆瓣/知乎/公众号等)
        └── Giscus 评论 (基于 GitHub Discussions)
```

## 二、一次性初始化（约30分钟）

### 1. 准备 GitHub 仓库

建议建立两个仓库：

| 仓库 | 用途 | 示例 |
|------|------|------|
| `yourname.github.io` | 博客源文件 + 自动部署 | 公开 |
| `yourname-images` | 图床（也可公开，仅存图） | 公开 |

> 命名规则：用户名 + `.github.io` 是 GitHub Pages 的保留格式，会自动获得 `https://yourname.github.io` 的默认地址。

### 2. 启用 GitHub Pages

1. 进入仓库 → **Settings → Pages**
2. Source 选择 **GitHub Actions**
3. 等待约1分钟，站点即上线

### 3. 绑定专属域名

1. 在域名商（如 Cloudflare / 阿里云）添加两条 CNAME 记录：
   ```
   www.yourname.com  →  yourname.github.io
   yourname.com      →  yourname.github.io
   ```
2. 在项目根目录创建 `static/CNAME` 文件，内容为：
   ```
   www.yourname.com
   ```
3. 仓库 Settings → Pages → Custom domain 填入 `www.yourname.com`，勾选 Enforce HTTPS
4. 等待 DNS 生效（通常几分钟到几小时）

### 4. 安装 Hugo（本地预览用，非必须）

```bash
# macOS
brew install hugo
# Windows (scoop)
scoop install hugo-extended
# Linux
snap install hugo

# 本地预览
hugo server -D
# 浏览器打开 http://localhost:1313
```

> 即使本地不装 Hugo，GitHub Actions 也会在每次 push 时自动构建，所以本地 Hugo 只是方便实时预览。

## 三、日常写文章流程

### 电脑端

```bash
git clone https://github.com/yourname/yourname.github.io.git
cd yourname.github.io
# 新建一篇文章（或用任意编辑器创建 .md）
hugo new posts/my-new-post.md
# 编辑 content/posts/my-new-post.md ...
git add .
git commit -m "add: 新文章"
git push   # 推送后自动部署，约30秒上线
```

### 手机端（重点需求）

**iOS 推荐：Working Copy（Git 客户端）+ 任意 Markdown 编辑器**
1. 用 Working Copy 克隆仓库
2. 用 iA Writer / 1Writer 编辑 `content/posts/xxx.md`
3. 回到 Working Copy，commit + push
4. 几秒后文章自动发布 ✅

**Android 推荐：Markor / 1Writer + Termux（Git）**
- 或用 **GitHub 官方 App** 直接编辑文件并提交

> 手机写稿体验的核心：仓库即数据库，Markdown 即格式，`git push` 即发布。

## 四、图床配置（PicGo + GitHub）

每周图片不多，追求 **稳定免费自动上传**：

### 方案：私有 GitHub 仓库作为图床

1. 在图床仓库生成一个 **Personal Access Token (Classic)**：
   - Settings → Developer settings → Personal access tokens → Tokens (classic)
   - 勾选 `repo` 权限
   - 保存 token（只显示一次）

2. 安装 [PicGo](https://picgo.github.io/PicGo-Doc/)（支持 Win/Mac/Linux）

3. 配置 GitHub 图床：
   ```
   Repo:           yourname/yourname-images
   Branch:         main
   Token:          ghp_xxxxxxxxxxxxxxxxxxxx
   Path:           images
   Custom domain:  https://cdn.jsdelivr.net/gh/yourname/yourname-images
   ```
   > 使用 jsDelivr CDN 加速，国内访问稳定快速。

4. 使用：写文章时截图/选图，PicGo 自动上传并返回 Markdown 链接，直接粘贴即可。

### 备选图床（若 GitHub 图床不够用）

- **腾讯云 COS**、**七牛云**：有免费额度，国内速度快
- **SM.MS**：免费图床，开箱即用
- 原则：**图片链接用稳定 CDN 域名，避免图床迁移导致图片失效**

## 五、评论系统（Giscus）

Giscus 基于 GitHub Discussions，免费、支持 Markdown、无广告，评论直接显示在文章下方。

### 接入步骤

1. 安装 [Giscus App](https://github.com/apps/giscus) 到你的博客仓库
2. 开启仓库的 **Discussions** 功能（Settings → Features → Discussions 勾选）
3. 到 [giscus.app](https://giscus.app) 生成配置代码，填入 `config.yaml` 的 `params.giscus`：
   ```yaml
   giscus:
     repo: "yourname/yourname.github.io"
     repoID: "R_xxx"          # 在 giscus.app 自动生成
     category: "Announcements"
     categoryID: "DIC_xxx"
     mapping: "pathname"
   ```
4. PaperMod 主题原生支持 Giscus，配置即生效

## 六、多渠道分发（半自动）

由于小红书/豆瓣等平台 API 不开放，采用 **RSS + 半自动复制** 策略：

| 平台 | 方式 | 自动化程度 |
|------|------|-----------|
| 博客本身 | Git push 自动部署 | ✅ 全自动 |
| RSS 订阅 | 自动生成 RSS Feed | ✅ 全自动 |
| 公众号 | 用 **墨滴** 一键复制排版 | 🔶 半自动（点一下） |
| 知乎/豆瓣 | 复制 Markdown 粘贴 | 🔶 半自动 |
| 小红书 | 手动搬运（图文） | 🖐️ 手动 |

> 墨滴（https://md.editor.mdnice.com）：粘贴 Markdown，一键复制适配公众号的排版，带样式。

## 七、保证"10年稳定运行"的 4 条铁律

1. **零依赖付费服务**：全部基于 GitHub 免费额度，无按月收费组件
2. **数据自持**：文章（Markdown）+ 图片全部在你自己的 GitHub 仓库，随时可迁移
3. **静态优先**：无数据库、无服务端，杜绝平台关停/服务过期风险
4. **域名自动续费**：开启域名商的自动续费，避免忘记续费丢失域名

## 八、目录结构

```
yourname.github.io/
├── .github/workflows/deploy.yml   # 自动部署工作流
├── content/posts/                 # 文章（Markdown）
│   └── hello-world.md
├── static/                        # 静态资源（图片/CNAME等）
│   └── CNAME
├── docs/                          # 本文档
├── config.yaml                    # 站点配置
└── themes/PaperMod/               # 主题（git submodule）
```

---

✅ 完成以上步骤后，你就拥有了一个 **免费、大厂背书、稳定运行十年以上** 的个人博客。
