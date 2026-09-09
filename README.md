# 我的博客

一个 **免费、大厂背书、稳定运行10年以上** 的个人博客。

## 技术栈

- **Hugo** (PaperMod 主题) — 静态生成器
- **GitHub Pages** — 托管（微软大厂、免费）
- **GitHub Actions** — 推送即自动部署
- **PicGo + GitHub 仓库** — 免费图床（jsDelivr 加速）
- **Giscus** — 评论系统（基于 GitHub Discussions）
- **RSS** — 多渠道分发

## 快速开始

详见 [docs/setup.md](docs/setup.md)。

```bash
git clone https://github.com/yourname/yourname.github.io.git
cd yourname.github.io
hugo new posts/my-new-post.md
# 编辑 content/posts/my-new-post.md
git add . && git commit -m "add: 新文章" && git push
```

推送后约30秒自动部署上线。

## 目录结构

```
yourname.github.io/
├── .github/workflows/deploy.yml   # 自动部署工作流
├── content/posts/                 # 文章（Markdown）
├── static/                        # 静态资源（CNAME 等）
├── docs/setup.md                  # 完整搭建指南
├── config.yaml                    # 站点配置
└── themes/PaperMod/               # 主题
```

## 成本

- 网站托管 + 图床：**¥0**
- 专属域名：≈ **¥60/年**
