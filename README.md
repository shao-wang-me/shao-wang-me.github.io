# shaowang.me

[![Build and deploy site](https://github.com/shao-wang-me/shao-wang-me.github.io/actions/workflows/main.yml/badge.svg)](https://github.com/shao-wang-me/shao-wang-me.github.io/actions/workflows/main.yml)

Shao Wang's personal website and public notes.

王邵的个人网站与公开笔记。

**Website / 网站:** [www.shaowang.me](https://www.shaowang.me/)

## Content / 内容

The site contains selected notes about software development, algorithms and other subjects.

网站主要收录软件开发、算法及其他主题的精选笔记。

## Development / 本地开发

The site uses [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) and [uv](https://docs.astral.sh/uv/).

```sh
uv sync --locked
uv run mkdocs serve
```

Build the production site locally with:

```sh
uv run --locked mkdocs build
```

## Publishing / 发布

Pull requests run a build check. Changes merged into `main` are built and deployed to GitHub Pages through GitHub Actions.

Pull Request 会运行构建检查；合并到 `main` 的内容会由 GitHub Actions 构建并发布到 GitHub Pages。
