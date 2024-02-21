---
title: 同步 obsidian 文档到 astro 的博客
author: pen
type: post
pubDatetime: 2024-02-20T01:04:48+08:00
lastmod: 2024-02-20T01:05:20+08:00
url: /2024/02/20/同步-obsidian-文档到-astro-的博客
description: 手把手教你如何将 obsidian 中的文章同步到 astro 的博客仓库中
draft: false
featured: false
postSlug: obsidian-astro-blog
tags:
  - obsidian
  - astro
  - blog
  - 博客
  - cloudflare
  - github
  - publisher
---
![image.png](https://blog-1256302330.cos.ap-beijing.myqcloud.com//test20240221150847.png)

## 背景
最近把笔记系统从 ：Logseq 迁移到 Obsidian 上了， 发现 obsidian 的能力比 logseq 要强大很多，对 markdown 的支持也非常友好。大概花了两天的时间研究各种各样的插件（过两天单独写一篇文章介绍），因为之前自己也搭建了一个博客，博客的技术栈是采用的 `Astro` `Github` `Cloudflare`，博客模板地址如下 [satnaing/astro-paper: A minimal, accessible and SEO-friendly Astro blog theme (github.com)](https://github.com/satnaing/astro-paper)，因为 Obsidian 和 Astro 的目录都是使用 MD 作为文件操作，所以我就打算搭建一个 Obsidian 编写文章，一键发布到 Github，然后同步发布到 Cloudflare 的能力。  

## 前置工作
1. **安装插件**：首先，需要给Obsidian安装Github Publisher插件，这是实现将Obsidian文章上传到GitHub仓库的关键。这个插件的使用可以让你在上传前指定文件目录、自定义内容替换等操作
2. **配置GitHub config**：在使用Github Publisher插件之前，需要对GitHub的配置进行一些设置。例如，生成的token不应该放在GitHub的公共仓库中，因为检测到该token后会失效。此外，Text & link converters的配置也会影响上传文章后的内容结构
3. **选择发布方式**：根据个人偏好，可以决定将Obsidian的笔记库公开或私密。
4. **自动同步与拉取**：为了确保文章能够自动同步到GitHub，可以利用Obsidian-Git插件将文件同步到GitHub。同时，还可以通过Cloudflare自动拉取GitHub上的文章，以提高发布效率和速度
5. **Markdown内容替换**：在上传文章时，可能需要对Markdown内容进行替换。建议关闭Markdown hard line break功能，因为开启它可能会导致空行变多，影响代码美观度和表格渲染效果

## 方案/流程

![image.png](https://blog-1256302330.cos.ap-beijing.myqcloud.com//test20240220160809.png)


## 配置手册
### Github publisher 配置
![image.png](https://blog-1256302330.cos.ap-beijing.myqcloud.com//test20240220161504.png)
| 序号 | 解释                                                                                                                                  |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | 填写博客地址 github 用户名称                                                                                                          |
| 2    | 填写博客地址的仓库名称                                                                                                                |
| 3    | 申请 github 的 token，这个 token 可以用来发布内容 [Personal Access Tokens (Classic) (github.com)](https://github.com/settings/tokens) |
| 4    | 选择发布的分支                                                                                                                        |
| 5    |  pr 是否自动合并，勾选为是 |

![image.png](https://blog-1256302330.cos.ap-beijing.myqcloud.com//test20240220162338.png)
| 序号 | 解释 |
| ----- | --- |
| 1 |  发布的博客中需要放置 md 的目录，这里 src/content 就是这个 Astro 博客中文章的地方  |

![image.png](https://blog-1256302330.cos.ap-beijing.myqcloud.com//test20240220162550.png)
| 序号 | 解释 |
| --- | --- |
| 1 |  增加发布按钮 |

