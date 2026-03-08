# Keti's Blog

使用 Hugo 搭建的个人博客，部署在 GitHub Pages。

## 开发

```bash
# 本地预览
hugo server -D

# 新建文章
hugo new content/post-name.md

# 构建网站
hugo
```

## 部署

推送到 main 分支后，GitHub Actions 会自动部署到 GitHub Pages。

博客地址：https://ketiblog.github.io/

## 配置

- 主题：[Paper](https://github.com/nanxiaobei/hugo-paper)
- 配置文件：`hugo.yaml`
- 内容目录：`content/`
