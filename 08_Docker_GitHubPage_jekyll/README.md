# Purpose
Create personal GitHub Pages in github.io domain.
Setup up individual GitHub Pages site locally with Jekyll by Docker more quickly and easily.

# What kind of Docker images Selection
starefossen/github-pages - Docker Hub
https://hub.docker.com/r/starefossen/github-pages/

Dockerfile
https://hub.docker.com/r/starefossen/github-pages/

Cause download ranking of GitHub Pages is top.

# docker-compose.yml
```docker-compose.yml
version: "3"
services:
    site:
        image: starefossen/github-pages:latest
        volumes:
            - /host_mnt/d/project/docker/github-pages/app:/usr/src/app
        ports:
            - 80:4000
        tty: true
```

## build images
```
D:\project\docker\github-pages
λ  docker-compose --file docker-compose.yml up -d
Starting github-pages_site_1 ... done

D:\project\docker\github-pages
λ docker ps
CONTAINER ID        IMAGE                             COMMAND                  CREATED             STATUS              PORTS                          NAMES
34d8fdaec869        starefossen/github-pages:latest   "/bin/sh -c 'jekyll …"   16 hours ago        Up 18 seconds       80/tcp, 0.0.0.0:80->4000/tcp   github-pages_site_1
```


# Directory of Jekyll
```
/_data        データファイル設置場所
    test.yml
/_includes    インクルードファイル設置場所
/_layouts     レイアウトファイル設置場所
/assets
    /styles
        index.sass    sass、scss、coffeeファイルは任意の場所に設置可能
_config.yml   サイト変数ファイル
index.md      Markdownファイル自動変換。HTMLファイルをそのまま設置も可能
```

# Jekyll Variables

## Site variable
```_config.yml
site_title: example site
```

```index.md
---
---
# Hello World
site_title: {{ site.site_title }}
```
{{ 変数名 }} のように波括弧2つでくくると変数が使えます。
サイト変数はプレフィックス site が必要です。

```出力
 Hello World
site_title: example site   
```

## Page variable
```index.md
---
page_title: example page
---
# Hello World
site_title: {{ site.site_title }}  
page_title: {{ page.page_title }}
```
同じように、ページ変数はYAML Front formatterに記述します。
ページ変数はプレフィックス page が必要です。

```出力
Hello World
site_title: example site
page_title: example page
```

## Data Files
```_data/test.yml
data_test: example data
```

```index.md
---
page_title: example page
---
# Hello World
site_title: {{ site.site_title }}  
page_title: {{ page.page_title }}  
data_test: {{ site.data.test.data_test }}
```

```出力
Hello World
site_title: example site
page_title: example page
data_test: example data
```

# Layout
```_layouts/default.html
<!doctype html>
<html>
    <head>
        <title>Jekyll test</title>
    </head>
    <body>
        {{ content }}
    </body>
</html>
```
{{ content }} がページ内容の入る部分です。

```index.md
---
layout: default
page_title: example page
---
# Hello World
site_title: {{ site.site_title }}  
page_title: {{ page.page_title }}  
data_test: {{ site.data.test.data_test }}
```

これで _layouts/default.html に書いた {{ content }} の部分に index.md の内容が入ります。

ちなみに layout: default.html のように拡張子まで書いてしまうと、「なんで！表示されないやん！」と筆者のように意味の分からないところで無駄な時間を使うことになります。
気付いた瞬間どっと疲れが。。。


```出力（ソースコード）
<!doctype html>
<html>
    <head>
        <title>Jekyll test</title>
    </head>
    <body>
        <h1 id="hello-world">Hello World</h1>
<p>site_title: example site<br />
page_title: example page<br />
data_test: example data</p>

    </body>
</html>
```

# include
```_includes/html_open.html
<!doctype html>
<html>
    <head>
        <title>Jekyll test</title>
    </head>
    <body>
```

```_includes/html_close.html
    </body>
</html>
```

include は、変数で使った波括弧 {{ }} ではなく、{% %} を使います。
```_layouts/default.html
{% include html_open.html %}
{{ content }}
{% include html_close.html %}
```

```出力（ソースコード）
<!doctype html>
<html>
    <head>
        <title>Jekyll test</title>
    </head>
    <body>

<h1 id="hello-world">Hello World</h1>
<p>site_title: example site<br />
page_title: example page<br />
data_test: example data</p>

    </body>
</html>
```

# Asset （css, JavaScript）
任意のディレクトリを作成し、その中に .sass、 .scss、 .coffee などの適切な拡張子を持ったファイルを作成すると、任意のディレクトリと同じ相対URLでファイルを読めるようになります。

これらのファイルも、空の YAML Front-Matter をファイル先頭に置くことで変換してくれるようになります。
```assets/styles/index.sass
---
---
h1
    font-size: 1em
```

```_includes/html_open.html
---
---<!doctype html>
<html>
    <head>
        <title>Jekyll test</title>
        <link rel="stylesheet" href="assets/styles/index.css">
    </head>
    <body>
```

# テーマ

Jekyllにはテーマ機能があるので、テーマを選択するだけで見た目を大きく変えることが出来ます。
GitHub Pagesにも実装されており、作ったリポジトリのSettingsページにあるGitHub Pages設定から、Choose a themeボタンでテーマを選択することが出来ます。

レイアウトやinclude、アセット等を使うと、選択したテーマを一部上書きすることが出来るので、テーマを使いつつ更にカスタマイズを行うことも出来ます。

# base tag of href
Jekyllはコンソールから利用する静的サイトジェネレータなので、webアクセスがあった時に利用出来るホストネーム等の取得が出来ません。
_config.ymlに url: localhost のような指定をすればローカルでは動作させることが出来ますが、これを「書かないと」github.ioでは自動で username.github.io を設定してくれます。
これではローカルとgithub.ioにアップロードした時とでbase hrefの共通化が出来ないので、一考して以下の対応としました。
```
{%if site.url == 'https://mkgask.github.io'%}{%assign site_url = site.url%}{% else %}{%assign site_url = '//localhost'%}{% endif %}
<base href="{{ site_url }}">
```
静的サイトジェネレータなので使える変数はすべて静的なものですが、テンプレートエンジンのように if を使うことができ、assign で変数を生成することも出来ます。
ので、_config.yml には記述なしとし、site.url がgithub.ioのドメインであればそのまま出力、そうでなければlocalhostにするようにしました。

# Troubleshooting
## mkdir /host_mnt/d: file exists
![alt tag](https://i.imgur.com/jM5k4IW.jpg)

# Reference
* [docker-compose（dockerで十分）でGitHub Pagesローカル開発環境 2018-02-25](https://qiita.com/mkgask/items/7578bb0f9c646dbb68d0)
* [30分のチュートリアルでJekyllを理解する 13 May 2012](http://melborne.github.io/2012/05/13/first-step-of-jekyll/)
* [Setting up your GitHub Pages site locally with Jekyll - User Documentation](https://help.github.com/en/articles/setting-up-your-github-pages-site-locally-with-jekyll)

* [jekyllのローカル開発環境(プレビュー) 2017-11-11](https://qiita.com/t_732_twit/items/be50b8bded0d157ad5c9)
* [Creating an Engineering Blog with GitHub Pages 29 Nov 2016 ](https://cardano.github.io/blog/2016/11/29/creating-an-engineering-blog)
* [Creating and Hosting a Personal Site on GitHub](http://jmcglone.com/guides/github-pages/)
* [Build A Blog With Jekyll And GitHub Pages August 1, 2014](https://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/)
* [https://help.github.com/en/articles/basic-writing-and-formatting-syntax](https://help.github.com/en/articles/basic-writing-and-formatting-syntax)

* [RubyをインストールせずにdockerでGitHub Pagesのプレビュー環境を作るメモ 2016-01-25](https://qiita.com/yamamoto-febc/items/8c5b50acb4b6075ee15d)
* [GitHub Pages (Jekyll) のローカルプレビュー環境を Docker で手軽に実現しよう (Macの場合)  2016-02-16](https://qiita.com/zacky1972/items/2f616963279eff0f33ad)
* [【個人メモ】buildingを使って気軽にDocker containerをつくろう 2014-04-03](https://qiita.com/futoase/items/21167e9d064b0e336e8f)
* [Markdownで執筆できるブログをJekyll + Github Pagesで公開する 2019年1月13日](https://tech.fusic.co.jp/ruby/jekyll-githubpages/)
```
Themeを変更する

Jekyll Themesから気に入ったテーマを選定して変更してみます。
今回は「HCZ Material」を使うことにしました。

$ wget https://github.com/ashutosh2k12/hcz-jekyll-blog/archive/master.zip
$ unzip master.zip
$ rm -rf hcz-jekyll-blog-master/_posts hcz-jekyll-blog-master/asset hcz-jekyll-blog-master/_config.yml
$ cp -a hcz-jekyll-blog-master/* ./
$ rm -rf hcz-jekyll-blog-master master.zip
$ bundle install
$ bundle exec jekyll s
```

* [使用 GitHub 免費製作個人網站](https://gitbook.tw/chapters/github/using-github-pages.html)
* [[架站] GitHub Pages 也可拿來架設HTML靜態網站，與綁定網域名稱](https://www.minwt.com/website/server/18522.html)
* [不用懂git也能用GitHub Pages架設靜態網站並綁定網域](https://medium.com/@NorthBei/%E4%B8%8D%E7%94%A8%E6%87%82git%E4%B9%9F%E8%83%BD%E7%94%A8github-pages%E6%9E%B6%E8%A8%AD%E9%9D%9C%E6%85%8B%E7%B6%B2%E7%AB%99%E4%B8%A6%E7%B6%81%E5%AE%9A%E7%B6%B2%E5%9F%9F-c60c02bc470c)
* [在 GitHub 上使用 Jekyll 建立自己的部落格 2017-07-28](https://frank198978104.github.io/2017/07/28/welcome-to-jekyll/)
* [Jekyll 更換主題及安裝插件 2017-08-08](https://frank198978104.github.io/2017/08/08/choose-your-jekyll-theme/)
* [使用 Jekyll 建立自己的 Github Page Blog 05 Feb 2017](https://nk910216.github.io/2017/02/05/HowToSetupBlog/)
* [利用Jekyll Bootstrap 快速建立Blog 2014-01-12](https://wcc723.github.io/jekyll/2014/01/12/jekyll-bootstrap/ )
```
這時候可能會遇到的錯誤

如果github page在此時如果編譯錯誤，他會發一封mail到用戶的信箱，如下圖，這是沒有驗證過的信箱…，要驗證後的信箱他才會正常的編譯。
![alt tag](https://wcc723.github.io/images/2014-01-12_114728.png)
```

* [在Winodws上運作jekyll 2014-01-13](https://wcc723.github.io/jekyll/2014/01/13/windows-jekyll-server/)
```
Windows 特有錯誤

在我安裝過那麼多次，都一定會遇到的錯誤，就是Windows版 jekyll中文會有編碼的問題，只要輸入jekyll server就會出現以下錯誤。
這時候就要到Ruby裡的jekyll資料夾修改一些設定檔
在windows的Ruby安裝路徑找到convertible.rb這個檔案打開

C:\Ruby200-x64\lib\ruby\gems\2.0.0\gems\jekyll-1.4.2\lib\jekyll\convertible.rb

將convertible.rb裡的值如下修改，就可以修正這個問題

原
self.content = File.read_with_options(File.join(base, name),
                                  merged_file_read_opts(opts))
改為
self.content = File.read(File.join(base, name),:encoding=>"utf-8")

接下來再輸入一次jekyll server試試看吧，這問題就會解決了。
```
* [在Jekyll設定自己的留言板 2014-01-14](https://wcc723.github.io/jekyll/2014/01/14/jekyll-disqus/)
* [在Jekyll中調整屬於自己的Template 2014-01-15](https://wcc723.github.io/jekyll/2014/01/15/jekyll-define-page/)



* []()
![alt tag]()

# h1 size

## h2 size

### h3 size

#### h4 size

##### h5 size

*strong*strong  
**strong**strong  

> quote  
> quote

- [ ] checklist1
- [x] checklist2

* 1
* 2
* 3

- 1
- 2
- 3
