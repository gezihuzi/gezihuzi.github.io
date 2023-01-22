---
layout: posts
title:  "使用jekyll 安装和部署博客"
date:   2023-01-09 21:00:00 +0800
---
使用jekyll 安装和部署博客, 和解决安装时遇到的问题

## 环境准备

> 操作环境 macOS 12.6.2

安装jekyll 和相关的依赖需要使用`ruby`, `macOS` 系统自带的`ruby` 版本过低且使用非`root` 用户`gem` 无法安装第三方的依赖。使用[rbenv](https://github.com/rbenv/rbenv) 来安装和管理`ruby` 环境。

具体的安装和使用请参见[使用rbenv 管理ruby 环境](/2023/01/21/rbenv)

安装完成后重启终端并运行以下命令查看当前的Ruby 版本信息:

```bash
ruby -v
```

运行后获得以下信息:

```bash
ruby 3.0.0p0 (2020-12-25 revision 95aff21468) [x86_64-darwin21]
```

## 安装jekyll

前置的依赖环境已经安装完成, 接下来我们安装`jekyll`, `jekyll`可以帮助我们将`markdown` 等文件生成静态的`HTML` 文件供我们部署。并支持插件主题等更多特性, 具体的相关信息请参见[jekyll 官网](https://jekyllrb.com/)

使用以下命令安装Bundler 和jekyll:

```bash
gem install bundler jekyll
```

使用jekyll 创建一个名为的网站工程并切换至网站根目录:

```bash
jekyll new my-awesome-site
cd my-awesome-site
```

运行网站:

```bash
bundle exec jekyll serve
# 现在可以通过访问 http://localhost:4000 来查看网站信息
```

## 遇到的问题

安装`jekyll` 的过程中, 安装和编译特定的依赖`eventmachine ****`时会报错, 大致的内容如下:

```bash
make "DESTDIR="
compiling binder.cpp
In file included from binder.cpp:20:
./project.h:119:10: fatal error: 'openssl/ssl.h' file not found
#include <openssl/ssl.h>
         ^~~~~~~~~~~~~~~
1 error generated.
make: *** [binder.o] Error 1

make failed, exit code 2

Gem files will remain installed in /Users/chen/.rvm/gems/ruby-3.0.0/gems/eventmachine-1.2.7 for inspection.
```

从以上的错误信息中我们了解到, 代码引用了openssl 源码的内容, 但是编译的项目无法找到相关的头文件, 所以编译失败了。所以我们需要去解决这个问题。

目前我使用的电脑是有安装LibreSSL 2.8.3 版本的依赖, 但是经过测试还是无法编译成功。所以我搜索Homebrew 查询openssl 相关的依赖:

```bash
brew search openssl
```

搜索的结果如下:

```bash
==> Formulae
glib-openssl                                             opensc
openssl@1.1                                              open-sp
openssl@3 ✔                                              openfst
openslp                                                  opencsg
openssh                                                  openmsx
opensaml                                                 homebrew/portable-ruby/portable-openssl

==> Casks
openmsx
```

发现当前安装的版本是openssl@3 的版本, 可能因为当前安装的版本太高了, 整体的头文件信息已经发生不可调整的更改。所以我打算使用老版本的openssl@1.1 来尝试是否可行。运行一下命令安装openssl@1.1 依赖:

```bash
brew install openssl@1.1
```

使用命令获取当前已经安装的openssl@1.1 的路径:

```bash
brew --prefix openssl@1.1
```

获取的路径为:

```bash
/opt/homebrew/opt/openssl@1.1
```

安装成功后使用以下的命令附加额外的--with-openssl-dir 参数并拼接获取的openssl@1.1 路径信息后重新安装eventmachine 依赖:

```bash
gem install eventmachine -- --with-openssl-dir=/opt/homebrew/opt/openssl@1.1
```

安装成功且未继续报错, 然后重新使用安装命令安装jekyll:

```bash
gem install bundler jekyll
```

安装成功
