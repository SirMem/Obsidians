---
Project: "[[SpringBoot]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 
Connected:
---
## 前置要求

在开始之前，你需要具备以下工具和账户：

1. **Node.js 和 npm**：Hexo 需要 Node.js 和 npm 来运行。你可以从 [Node.js 官方网站](https://nodejs.org/) 下载并安装。
2. **Git**：用于版本控制和部署。你可以从 [Git 官方网站](https://git-scm.com/) 下载并安装。
3. **GitHub 账户**：用于托管你的博客代码和网站。你可以从 [GitHub](https://github.com/) 免费注册一个账户。
4. **CloudFlare 账户**：用于内容分发网络 (CDN) 和 DNS 服务管理。你可以从 [CloudFlare](https://www.cloudflare.com/) 免费注册一个账户。
5. **Namecheap 账户**：用于购买和管理域名。你可以从 [Namecheap](https://www.namecheap.com/) 注册一个账户并购买域名。

## 步骤 1：设置 GitHub

1. **创建 GitHub 仓库**：
   - 登录到你的 GitHub 账户。
   - 点击右上角的 “+” 图标并选择 “New repository”。
   - 仓库名称格式应为：`<username>.github.io`，其中 `<username>` 是你的 GitHub 用户名。
   - 设为 “Public” 并点击 “Create repository”。

2. **生成 GitHub 访问令牌**：
   - 在 GitHub 的设置页面中，选择 “Developer settings” > “Personal access tokens”。
   - 生成一个新的 token，并确保选择 `repo` 范围的权限。
   - 复制这个 token，因为稍后在 Hexo 配置以及图床创建时需要用到。

## 步骤 2：设置 Hexo
1. **安装 Hexo**：
   打开命令行工具并运行以下命令：

   ```bash
   npm install -g hexo-cli
   ```

2. **创建 Hexo 项目**：
   - 选择一个文件夹来存储你的博客项目。
   - 运行以下命令来初始化 Hexo 项目：

     ```bash
     hexo init my-blog
     cd my-blog
     npm install
     ```

3. **添加主题**
- 在[Hexo Themes](https://hexo.io/themes/)添加主题，使用Git克隆主题至Themes文件夹
- 修改根目录_config.yml文件的theme为对应主题名字

## 步骤 3：将博客部署到 GitHub

1. **安装 Hexo 部署工具**：
   - 在你的 Hexo 项目根目录下，运行以下命令：

     ```bash
     npm install hexo-deployer-git --save
     ```

2. **配置 `_config.yml` 文件**：
   - 打开 `my-blog` 文件夹中的 `_config.yml` 文件。
   - 找到 `deploy` 部分并将其更新为：

     ```yaml
     deploy:
       type: git
       repo: https://github.com/<username>/<username>.github.io.git
       branch: main
     ```

     替换 `<username>` 为你的 GitHub 用户名。

3. **生成和部署博客**：
   - 运行以下命令生成静态文件：

     ```bash
     hexo generate
     ```

   - 然后运行以下命令将博客部署到 GitHub：

     ```bash
     hexo deploy
     ```

   - 部署完成后，你可以在浏览器中访问 `https://<username>.github.io` 查看你的博客。

## 步骤 4：配置 CloudFlare

1. **添加站点到 CloudFlare**：
   - 登录到你的 CloudFlare 账户并点击 “Add a Site”。
   - 输入你的 GitHub Pages 域名（例如：`<username>.github.io`），然后点击 “Add site”。
   
2. **选择计划**：
   - 选择免费的计划并点击 “Continue”。

3. **更改 DNS 服务器**：
   - CloudFlare 会为你的域名提供新的 DNS 服务器地址。
   - 登录到你的 Namecheap 账户，选择你的域名，找到 DNS 设置，将 Namecheap 的默认 DNS 更改为 CloudFlare 提供的 DNS。

4. **配置 Cloudflare Pages**
- 登录到你的 Cloudflare 账户。
- 在左侧菜单中，选择 “Pages”。
- 点击 “Create a project” 按钮。
- 连接Github仓库，选择相应网站文件仓库和分支


## 步骤 5：markdown笔记优化
1. **图床设置**
- 使用PicGo填入Github信息
- 使用Obsidian安装Image auto upload Plugin自动上传本地markdown图片

2. **图片链接优化**
- 在Hexo的themes所使用的主题下，找到scripts文件夹，添加filter.ejs文件
```ejs
hexo.extend.filter.register('before_post_render', function(data){

    // 使用正则表达式匹配 `![内容](link)` 的形式，并将内容替换为空

    data.content = data.content.replace(/!\[.*?\]\(/g, '![](');

    return data;

});
```


## 总结

通过上述步骤，你现在已经成功地使用 Hexo、GitHub、CloudFlare 和 Namecheap 创建了一个自定义域名的博客。这个博客使用 Hexo 生成静态网站，托管在 GitHub 上，通过 CloudFlare 提供 CDN 和 HTTPS 支持，并使用 Namecheap 管理域名。

可以通过修改 Hexo 项目的 `_config.yml` 文件和 `source` 文件夹中的内容来进一步定制你的博客。每次更改后，运行 `hexo clean`，`hexo generate` 和 `hexo deploy` 以更新你的 GitHub Pages 网站。