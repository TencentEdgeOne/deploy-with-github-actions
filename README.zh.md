# Next.js 项目部署到 EdgeOne Pages

这是一个 Next.js 模板项目，使用 GitHub Actions 进行构建并部署到 [EdgeOne Pages](https://edgeone.ai/products/pages)。

## 项目概述

本项目提供了 Next.js 应用程序的启动模板，集成了 GitHub Actions 工作流和自动部署功能。

完整的 `.github/workflows/deploy.yml` 配置如下：

```yml
name: Build and Deploy

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
      
      - name: Install dependencies
        run: npm install
      
      - name: Build project
        run: npm run build
      
      - name: Deploy to EdgeOne Pages
        run: npx edgeone pages deploy ./out --name my-edgeone-pages-project --token ${{ secrets.EDGEONE_API_TOKEN }}
        env:
          EDGEONE_API_TOKEN: ${{ secrets.EDGEONE_API_TOKEN }}
```

## 步骤1：GitHub Actions 构建

本项目使用 [GitHub Actions](https://docs.github.com/zh/actions) 进行自动化构建。当代码推送到 main 分支时，会触发以下构建过程：

1. 检出代码仓库
2. 设置 Node.js 20 环境
3. 安装项目依赖
4. 使用 Next.js 构建项目

## 步骤2：EdgeOne Pages 部署

构建完成后，项目会通过以下过程自动部署到 [EdgeOne Pages](https://edgeone.ai/products/pages)：

1. 构建阶段生成 `./out` 目录
2. 使用 EdgeOne 命令行工具进行部署：
   ```bash
   npx edgeone pages deploy ./out --name my-edgeone-pages-project --token ${{ secrets.EDGEONE_API_TOKEN }}
   ```

## 设置仓库密钥

要部署到 EdgeOne Pages，你需要在 GitHub 中添加 `EDGEONE_API_TOKEN` 作为仓库密钥：

1. 导航到你的 GitHub 仓库
2. 进入 "Settings" > "Secrets and variables" > "Actions"
3. 点击 "New repository secret"
4. 名称：`EDGEONE_API_TOKEN`
5. 值：你的 EdgeOne API 令牌

从 EdgeOne Pages 获取你的 `EDGEONE_API_TOKEN`，访问：https://edgeone.ai/document/177158578324279296

# 参考资料

- EdgeOne Pages 文档：https://edgeone.ai/document/160427672992178176
- GitHub Actions 文档：https://docs.github.com/zh/actions 