# Antigravity MacOS Guide

独立静态网页项目，提供 Antigravity 在 MacOS 平台上的深度配置与疑难解答指南。本项目采用单页应用架构，无需构建工具，可直接部署到 Vercel 或其他静态托管平台。

## 项目特点

- **零依赖构建**：纯静态 HTML/CSS/JavaScript，无需 Node.js、Webpack 或其他构建工具
- **自包含架构**：所有样式、脚本和图片资源均包含在项目内
- **响应式设计**：完美适配桌面端和移动端设备
- **Markdown 实时渲染**：基于原生 JavaScript 实现的 Markdown 解析器
- **目录导航**：自动生成文章目录，支持平滑滚动定位
- **代码高亮与复制**：代码块支持一键复制功能
- **现代化 UI**：采用渐变背景、毛玻璃效果等现代设计元素

## 文件结构

```
antigravity-macos-guide/
├── index.html              # 主页面（包含样式、渲染逻辑和 Markdown 解析器）
├── article.md              # 文章正文内容（Markdown 格式）
├── assets/
│   └── antigravity/        # 教程图片资源目录
│       ├── cover.png       # 封面图片
│       ├── macos/          # MacOS 平台专用截图
│       │   ├── image1.png
│       │   ├── image2.png
│       │   └── ...
│       └── ...
└── README.md               # 项目说明文档
```

## 核心文件说明

### index.html
项目的主入口文件，包含以下功能模块：
- **样式系统**：使用 CSS 变量和渐变背景，支持响应式布局
- **导航栏**：包含品牌标识、阅读全文链接和 Windows 指南跳转
- **Hero 区域**：展示封面图片、标题、描述和元数据
- **目录侧边栏**：自动从 Markdown 文件生成目录结构
- **文章渲染器**：自定义 Markdown 解析器，支持标题、列表、代码块、表格、图片等
- **交互功能**：代码块复制、平滑滚动、返回顶部按钮

### article.md
文章正文采用 Markdown 格式编写，包含：
- **Frontmatter**：文章元数据（标题、日期、标签、封面图）
- **章节结构**：六个主要章节，涵盖从准备到故障排查的完整流程
- **图片引用**：使用相对路径引用 assets 目录中的图片

## 本地开发

### 方法一：直接打开
直接用浏览器打开 `index.html` 文件即可预览。

### 方法二：使用本地服务器（推荐）
由于某些浏览器的 CORS 限制，建议使用本地服务器：

```bash
# 使用 Python 3
python -m http.server 8000

# 使用 Python 2
python -m SimpleHTTPServer 8000

# 使用 Node.js (需安装 http-server)
npx http-server -p 8000

# 使用 PHP
php -S localhost:8000
```

然后在浏览器访问 `http://localhost:8000`

### 编辑文章
直接编辑 `article.md` 文件，保存后刷新浏览器即可看到更新。

## 部署指南

### Vercel 部署（推荐）

1. **导入仓库**
   - 访问 [Vercel](https://vercel.com)
   - 点击 "Add New Project"
   - 选择 GitHub 并授权
   - 选择 `antigravity-macos-guide` 仓库

2. **配置项目**
   - Framework Preset: 选择 "Other" 或 "Static Site"
   - Root Directory: 保持默认 `./`
   - Build Command: 留空（无需构建）
   - Output Directory: 保持默认 `./`

3. **部署**
   - 点击 "Deploy" 按钮
   - 等待部署完成（通常 30-60 秒）
   - Vercel 会自动分配一个 `.vercel.app` 域名

4. **自定义域名（可选）**
   - 在项目设置中添加自定义域名
   - 配置 DNS 记录指向 Vercel

### GitHub Pages 部署

1. **启用 GitHub Pages**
   - 进入仓库 Settings
   - 找到 "Pages" 选项
   - Source 选择 "Deploy from a branch"
   - Branch 选择 `main`，目录选择 `/ (root)`

2. **访问网站**
   - 部署完成后访问 `https://yourusername.github.io/antigravity-macos-guide/`

### Netlify 部署

1. **导入站点**
   - 访问 [Netlify](https://netlify.com)
   - 点击 "Add new site" -> "Import an existing project"
   - 连接 GitHub 并选择仓库

2. **构建设置**
   - Build command: 留空
   - Publish directory: 留空或输入 `.`

3. **部署**
   - 点击 "Deploy site"

## 自定义配置

### 修改主题颜色
编辑 `index.html` 中的 CSS 变量：
```css
:root {
  --accent: #1a73e8;  /* 修改此值更改主题色 */
}
```

### 修改网站信息
编辑 `index.html` 中的 meta 标签：
```html
<meta name="description" content="Antigravity MacOS 深度配置与疑难解答指南。" />
<title>Antigravity MacOS 深度配置与疑难解答指南</title>
```

### 添加新图片
1. 将图片文件放入 `assets/antigravity/` 目录
2. 在 `article.md` 中使用相对路径引用：
```markdown
![图片描述](/articles/antigravity/your-image.png)
```
注意：路径会被自动替换为 `./assets/antigravity/your-image.png`

## 技术栈

- **HTML5**：语义化标签
- **CSS3**：现代 CSS 特性（Grid、Flexbox、渐变、backdrop-filter）
- **Vanilla JavaScript**：原生 JavaScript，无第三方依赖
- **Markdown**：文章内容格式

## 浏览器兼容性

- Chrome/Edge 90+
- Firefox 88+
- Safari 14+
- 移动端浏览器（iOS Safari, Chrome Mobile）

## 技术支持

如有任何问题或需要一对一技术协助，请扫描下方二维码加入售后群：

![售后群二维码](https://i.ibb.co/HLfKR3x2/20260507122135-1913-114.jpg)

## 许可证

本项目内容仅供参考学习使用。
