# 本地开发调试指南

## 快速开始

```bash
# 1. 启动开发服务器（带热重载）
npm run dev
# 或直接: bundle exec jekyll serve --livereload

# 2. 浏览器访问
# → http://localhost:4000/

# 3. 修改内容后自动刷新，Ctrl+C 停止
```

## 常用命令

| 命令 | 说明 |
|------|------|
| `npm run dev` | 启动开发服务器，文件修改后自动重新生成 |
| `npm run build` | 生产构建，输出到 `_site/` |
| `bundle exec jekyll build` | 同 `npm run build` |
| `npm run lint:prettier` | 代码格式检查 |
| `npm run lint:style-contract` | 项目结构合规检查 |
| `bundle exec al-folio upgrade overrides audit` | 检查本地覆盖文件是否变动 |

## 环境依赖

| 工具 | 版本 | 来源 |
|------|------|------|
| Ruby | 3.4.10 | rbenv（`.ruby-version` 已锁定） |
| Bundler | 4.0.6 | rbenv shims |
| Node.js | v24.18.0 | `/usr/local/bin/node` |
| npm | 11.16.0 | `/usr/local/bin/npm` |
| Python | 3.8.3 | miniconda3 |

## 注意事项

### 代理问题

环境中设置了代理变量（`http_proxy`、`https_proxy`、`ALL_PROXY` 指向 `127.0.0.1:7890`），Ruby 的 HTTPS 请求通过 SOCKS5 代理可能出错。遇到 `SSL_connect` 或网络超时错误时，临时取消代理后构建：

```bash
unset http_proxy https_proxy ALL_PROXY
npm run build
```

### Jupyter Notebook 支持

`~/.local/bin` 不在 PATH 中，若构建提示 `jupyter-nbconvert` 找不到：

```bash
# 临时添加
export PATH="$HOME/.local/bin:$PATH"
npm run dev
```

建议将以下行添加到 `~/.zshrc` 末尾：

```bash
export PATH="$HOME/.local/bin:$PATH"
```

### macOS 13 兼容覆盖

`_sass/_themes.scss` 是本地覆盖文件，用于兼容 macOS 13（Ventura）。升级到 macOS 14+ 后可以：

1. 删除 `_sass/_themes.scss`
2. 删除 `.al-folio-overrides.yml` 中的对应条目
3. 编辑 `Gemfile`，将 `sass-embedded` 约束改回无限制
4. 运行 `bundle update sass-embedded`

### 外部 RSS 源

`_config.yml` 中默认的示例 RSS 源（Medium、Google Blog）已注释。如需添加自己的外部文章源，编辑 `_config.yml` 中 `external_sources` 部分。

## 目录结构

| 路径 | 用途 |
|------|------|
| `_pages/` | 页面内容（关于、简历等） |
| `_posts/` | 博客文章 |
| `_projects/` | 项目展示 |
| `_news/` | 新闻动态 |
| `_teachings/` | 教学相关 |
| `_books/` | 书籍相关 |
| `_bibliography/` | 参考文献 |
| `_config.yml` | 站点配置 |
| `_site/` | 构建输出（构建后生成） |
