---
title: 使用 Nginx 的小提示
date: 2026-01-11 20:11:52
tags:
  - [tips]
excerpt: 在 Arch Linux 上 Nginx 的配置。
---

## Nginx 的配置

nginx 在 arch linux 中的配置，主要是注意两个路径：

1. `/etc/nginx`里面的`nginx.conf`文件用来配置 root，root 指向项目文件夹（是项目的 index.html 所在的文件夹，而不是 nginx 默认的欢迎页面 index.html 所在的文件夹。前者是后者的子目录）
2. 需要把项目文件夹放在`/usr/share/nginx/html`这个文件夹下。如前所述，最终 root 指向的是`/usr/share/nginx/html/project`

## 启动 Nginx

- 设置 nginx 开机自启动：`systemctl enable nginx`；
- 验证是否成功开启：`systemctl is-enabled nginx`
- 手动开/关/重载：`systemctl start/stop/reload nginx`
- 查看状态：`systemctl status nginx`

## 补充

1. 浏览器中的请求 URL 中的`api`来标识这是一个发向 tomcat 服务的请求。所以一般来说，对于静态资源的请求是没有`api`标识的，直接加载。
