---
category: 我的电脑
tags:
  - configures
date: 2023-4-16
title: nginx的配置
---

## nginx 安装
`brew install nginx`

安装成功
```zsh
==> Summary
🍺  /opt/homebrew/Cellar/nginx/1.23.4: 26 files, 2.2MB
==> Running `brew cleanup nginx`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
==> Caveats
==> nginx
Docroot is: /opt/homebrew/var/www

The default port has been set in /opt/homebrew/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.

nginx will load all files in /opt/homebrew/etc/nginx/servers/.

To restart nginx after an upgrade:
  brew services restart nginx
Or, if you don't want/need a background service you can just run:
  /opt/homebrew/opt/nginx/bin/nginx -g daemon off;
```

总结就是：
1. 服务器配置文件在 `/opt/homebrew/etc/nginx/nginx.conf`
2. `nginx will load all files in /opt/homebrew/etc/nginx/servers/.` 没理解这个servers的意思 
3. 默认的 站点根目录是：`/opt/homebrew/var/www`, 已经改为博客的地址`/User/wiu/Code/blog/dist`
4. nginx修改了配置文件后，需要运行`nginx -s reload`来重新加载
