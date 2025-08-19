# Lausen's Blog

🌟 一个基于Jekyll的个人技术博客，专注于AI、机器学习、深度学习等技术主题。

## ✨ 特性

- 🎨 **现代化设计** - 仿照Lil'Log的简洁优雅风格
- 🌙 **深色模式** - 支持明暗主题切换
- 📱 **响应式布局** - 完美适配各种设备
- 🔍 **搜索功能** - 内置文章搜索
- 🏷️ **标签系统** - 文章分类管理
- 📚 **归档页面** - 按年份浏览所有文章
- ❓ **FAQ页面** - 常见问题解答

## 🚀 本地运行

### 环境要求

- Ruby 2.7.0+
- Jekyll 3.8.6+
- Linux/macOS/Windows

### 快速开始

1. **克隆仓库**
   ```bash
   git clone https://github.com/lausen001/lausen001.github.io.git
   cd lausen001.github.io
   ```

2. **安装Jekyll**
   
   **Ubuntu/Debian:**
   ```bash
   sudo apt install jekyll -y
   ```
   
   **CentOS/RHEL:**
   ```bash
   sudo yum install jekyll
   ```
   
   **macOS:**
   ```bash
   gem install jekyll bundler
   ```
   
   **Windows:**
   ```bash
   gem install jekyll bundler
   ```

3. **准备运行环境**
   
   为了避免依赖问题，建议暂时移动Gemfile：
   ```bash
   mv Gemfile Gemfile.bak 2>/dev/null || true
   ```

4. **启动本地服务器**
   ```bash
   jekyll serve --config _config_local.yml --host=localhost --port=4000
   ```

5. **访问网站**
   
   打开浏览器访问：`http://localhost:4000`

### 开发模式

如果需要实时查看更改：

```bash
pkill -f jekyll
jekyll serve --config _config_local.yml --host=localhost --port=4000 --livereload
```

### 生产环境构建

```bash
jekyll build --config _config.yml
```

## 📁 项目结构

```
.
├── _config.yml          # 主配置文件
├── _config_local.yml    # 本地开发配置
├── _layouts/            # 页面布局模板
│   ├── default.html     # 基础布局
│   ├── home.html        # 首页布局
│   ├── post.html        # 文章页布局
│   └── page.html        # 静态页布局
├── _posts/              # 博客文章
├── assets/              # 静态资源
│   └── css/
│       └── style.css    # 主样式文件
├── archive.html         # 归档页面
├── search.html          # 搜索页面
├── tags.html            # 标签页面
├── faq.md              # FAQ页面
└── index.html          # 首页
```

## ✍️ 写作指南

### 创建新文章

1. 在`_posts/`目录下创建新文件，文件名格式：`YYYY-MM-DD-title.md`
2. 文件开头添加Front Matter：

```yaml
---
layout: post
title: "文章标题"
date: 2024-12-20 10:00:00 +0800
tags: [AI, 机器学习]
author: Lausen
reading_time: 5
excerpt: "文章摘要..."
---

文章内容...
```

### 主题切换

网站支持明暗主题切换：
- 点击header右上角的🌙/☀️图标即可切换
- 主题偏好会自动保存到浏览器localStorage中

## 🎨 自定义

### 修改配置

编辑`_config.yml`文件来修改网站基本信息：

```yaml
title: "Your Blog Name"
description: "Your blog description"
url: "https://yourusername.github.io"
github_username: yourusername
```

### 修改样式

主要样式文件位于`assets/css/style.css`，支持：
- 亮色/暗色主题
- 响应式设计
- 自定义配色方案

### 添加新页面

1. 创建新的`.html`或`.md`文件
2. 添加适当的Front Matter
3. 在`_config.yml`的navigation中添加链接

## 🚀 部署到GitHub Pages

1. **推送代码到GitHub**
   ```bash
   git add .
   git commit -m "Update blog"
   git push origin main
   ```

2. **启用GitHub Pages**
   - 进入仓库Settings
   - 找到Pages选项
   - 选择Source为"Deploy from a branch"
   - 选择分支为"main"和文件夹为"/ (root)"

3. **访问网站**
   
   几分钟后即可通过`https://yourusername.github.io`访问

## 🛠️ 故障排除

### 常见问题

**1. CSS样式不生效**
```bash
# 清理缓存并重启
rm -rf _site .jekyll-cache
jekyll serve --config _config_local.yml --host=localhost --port=4000
```

**2. 依赖冲突**
```bash
# 暂时移动Gemfile
mv Gemfile Gemfile.bak
```

**3. 权限问题**
```bash
# Ubuntu/Debian安装Jekyll
sudo apt install jekyll -y
```

**4. 端口被占用**
```bash
# 使用不同端口
jekyll serve --config _config_local.yml --host=localhost --port=4001
```

## 📝 更新日志

- **2024-12-20**: 初始版本发布
  - 基础Jekyll博客结构
  - 响应式设计
  - 深色模式支持
  - 搜索和标签功能

## 📄 许可证

MIT License - 详见[LICENSE](LICENSE)文件

## 🤝 贡献

欢迎提交Issue和Pull Request！

## 📞 联系方式

- GitHub: [@lausen001](https://github.com/lausen001)
- Email: your-email@example.com

---

⭐ 如果这个项目对你有帮助，请给个star！
