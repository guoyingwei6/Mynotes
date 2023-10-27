---
date: 2023-10-07
categories:
  - Tutorial
slug: encrypt
password: password
---

## mkdocs encrypt

本文介绍通过调用mkdocs-encryptcontent-plugin插件进行mkdocs界面的加密

### Install

`pip install mkdocs-encryptcontent-plugin`

在`mkdocs.yml`中添加以下配置：

```
plugins:
    - search: {}
    - encryptcontent: {}
```

### Usuage

Add an meta tag `password: secret_password` in your markdown files to protect them.

Add `global_password: your_password` in plugin configuration variable, to protect all pages with this password by default

```
  - encryptcontent: 
      password_button: True
      global_password: 'password'
```

If the password meta tag is defined in a markdown file, it will ALWAYS override the global password.
