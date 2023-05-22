---
layout: post
title:  "文档编写注意事项"
date:   2023-05-22 10:18:00 +0800
categories: jekyll update
---


# 编写API文档指南

## 元数据格式

在编写API文档时，需要使用元数据来定义文档的布局、标题、日期和分类等信息。以下是一些常用的元数据字段及其格式：

layout: [布局类型]
title: [文档标题]
date: [日期和时间]
categories: [分类]

- `layout`：指定文档的布局类型，例如 `layout: page` 或 `layout: post`。
- `title`：指定文档的标题。
- `date`：指定文档的日期和时间，格式为 `YYYY-MM-DD HH:mm:ss +/-HHMM`。
- `categories`：指定文档的分类，可以是一个或多个分类，使用逗号分隔。

对于 `categories`，请从以下内容中选择：

- `getting started`：用于包含 API 的入门指南和快速开始的文档。
- `endpoints`：用于包含 API 的各种端点和接口的文档。
- `authentication`：用于包含有关 API 身份验证和授权的文档。
- `examples`：用于包含 API 的示例代码和使用案例的文档。
- `troubleshooting`：用于包含解决常见问题和故障排除的文档。

![IoT Development](/Page/assets/iot-development.jpg)