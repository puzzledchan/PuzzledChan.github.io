## 博客主题
博客由 [Hugo](https://gohugo.io/) 驱动，主题是 [MemE](https://github.com/reuixiy/hugo-theme-meme)。
## 搜索设置
搜索采用[Algolia](https://www.algolia.com/)搜索引擎，采用[github workflow](https://docs.github.com/en/actions/using-workflows)进行自动同步，
同步工具为开源[github action](https://github.com/iChochy/Algolia-Upload-Records)工具。
## 发布至github主页
采用[git](https://git-scm.com/)工具上传至个人主页 **username.github.io**，其中选择展示文件夹为 **/docs**。
## 同步到自有域名
由于国内访问github过慢，因此将博客同步到个人的国内域名下，通过[nginx](https://nginx.org/)配置，将所有的访问重定向到 *https*。