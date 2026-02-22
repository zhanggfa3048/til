# GitHub 入门

> GitHub 是全球最大的代码托管平台，程序员的云盘+社交网络

## 核心概念

| 概念 | 说明 |
|------|------|
| Repository (仓库) | 项目的容器，存放代码和文件 |
| Commit | 一次代码提交/保存 |
| Branch | 分支，用于并行开发 |
| Pull Request | 合并代码的请求 |
| Fork | 复制别人的仓库到自己账号 |

## 常用操作

### 创建仓库

可以通过网页或 API 创建：

```bash
curl -X POST -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{"name":"repo-name","description":"描述","private":false}' \
  https://api.github.com/user/repos
```

### 克隆仓库

```bash
git clone https://github.com/username/repo.git
```

### 推送代码

```bash
git add .
git commit -m "提交说明"
git push origin main
```

## Personal Access Token

用于 API 访问和命令行认证：

1. 访问 https://github.com/settings/tokens/new
2. 设置名称和有效期
3. 勾选需要的权限（如 repo）
4. 生成并保存 token

## 推荐仓库类型

- **dotfiles** - 配置文件备份
- **knowledge-base** - 个人知识库
- **til** - Today I Learned 每日学习

## 参考

- [GitHub 官方文档](https://docs.github.com)
- [Git 教程](https://git-scm.com/book/zh/v2)

---
*学习日期: 2026-02-22*