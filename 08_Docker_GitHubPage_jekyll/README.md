# Purpose
Create personal GitHub Pages in github.io domain.
Setup up individual GitHub Pages site locally with Jekyll by Docker more quickly and easily.

# docker-compose.yml
```docker-compose.yml
version: "3"
services:
    site:
        image: jekyll/jekyll
        container_name: github-blog-local
        volumes:
            - /host_mnt/d/project/docker/github-pages/app:/usr/src/app
        command: /bin/sh -c 'jekyll serve -s /usr/src/app --watch'    
        ports:
            - 4000:4000
        tty: true
```

## build images
```
D:\project\docker\github-pages
λ  docker-compose --file docker-compose.yml up
Pulling site (jekyll/jekyll:)...
latest: Pulling from jekyll/jekyll
8e402f1a9c57: Pull complete
3cf24f050bbd: Pull complete
164ebf0a871b: Pull complete
781bcfdb7fef: Pull complete
102e0bff17f5: Pull complete
92058a7b0af8: Pull complete
Recreating github-blog-local ... done
Attaching to github-blog-local
github-blog-local | ruby 2.6.1p33 (2019-01-30 revision 66950) [x86_64-linux-musl]
github-blog-local | Configuration file: /usr/src/app/_config.yml
github-blog-local |        Deprecation: The 'gems' configuration option has been renamed to 'plugins'. Please update your config file accordingly.
github-blog-local |             Source: /usr/src/app
github-blog-local |        Destination: /srv/jekyll/_site
github-blog-local |  Incremental build: disabled. Enable with --incremental
github-blog-local |       Generating...
github-blog-local |                     done in 1.361 seconds.
github-blog-local |  Auto-regeneration: enabled for '/usr/src/app'
github-blog-local |     Server address: http://0.0.0.0:4000
github-blog-local |   Server running... press ctrl-c to stop.

D:\project\docker\github-pages
λ docker ps
CONTAINER ID        IMAGE                             COMMAND                  CREATED             STATUS              PORTS                          NAMES
725852bfc928        jekyll/jekyll       "/usr/jekyll/bin/ent…"   12 minutes ago      Up 5 minutes        0.0.0.0:4000->4000/tcp, 35729/tcp   github-blog-local
```

# Jekyll Bootstrap
* [利用Jekyll Bootstrap 快速建立Blog 2014-01-12](https://wcc723.github.io/jekyll/2014/01/12/jekyll-bootstrap/ )

## Download and Install Jekyll Bootstrap

### Download Zip file of Jekyll Bootstrap
![alt tag](https://wcc723.github.io/images/2014-01-02_170007.png)

### Unzip Zip file of Jekyll Bootstrap and copy all files under foloder app
![alt tag](https://wcc723.github.io/images/2014-01-02_170143.png)

### docker run "starefossen/github-pages" image
```
d:\project\docker\github-pages
(MoneyHunter) λ docker run -t --rm -v %cd%/app:/usr/src/app -p "80:4000" starefossen/github-pages
Configuration file: none
            Source: /usr/src/app
       Destination: /_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.087 seconds.
 Auto-regeneration: enabled for '/usr/src/app'
    Server address: http://0.0.0.0:4000
  Server running... press ctrl-c to stop.
      Regenerating: 2 file(s) changed at 2019-03-09 09:42:02
                    _drafts/jekyll-introduction-draft.md
                    _includes/JB/analytics
                    ...done in 0.0124364 seconds.
~~省略~~
[2019-03-09 09:42:15] ERROR `/assets/themes/bootstrap-3/bootstrap/css/bootstrap.min.css' not found.
[2019-03-09 09:42:15] ERROR `/assets/themes/bootstrap-3/bootstrap/css/bootstrap-theme.min.css' not found.
[2019-03-09 09:42:15] ERROR `/assets/themes/bootstrap-3/bootstrap/js/bootstrap.min.js' not found.
[2019-03-09 09:42:15] ERROR `/assets/themes/bootstrap-3/css/style.css' not found.
[2019-03-09 09:42:15] ERROR `/assets/themes/bootstrap-3/bootstrap/css/bs-sticky-footer.css' not found.
[2019-03-09 09:42:15] ERROR `/assets/themes/bootstrap-3/bootstrap/js/bootstrap.min.js' not found. 

[2019-03-09 10:14:41] ERROR `/assets/themes/bootstrap-3/css/style.css' not found.
[2019-03-09 10:14:41] ERROR `/favicon.ico' not found.
[2019-03-09 10:17:03] ERROR `/assets/themes/bootstrap-3/css/style.css' not found.
[2019-03-09 10:17:04] ERROR `/assets/themes/bootstrap-3/css/style.css' not found.
[2019-03-09 10:17:06] ERROR `/assets/themes/bootstrap-3/css/style.css' not found.
[2019-03-09 10:17:07] ERROR `/assets/themes/bootstrap-3/css/style.css' not found.
```

### Create subfolder bootstrap-3 under /assets/themes and put folders bootstrap and css under subfolder bootstrap-3
![alt tag](https://i.imgur.com/9ko4GUD.jpg)

```
      Regenerating: 8 file(s) changed at 2019-03-09 10:13:51
                    assets/themes/bootstrap-3/bootstrap/css/bootstrap-theme.min.css
                    assets/themes/bootstrap-3/bootstrap/css/bootstrap.min.css
                    assets/themes/bootstrap-3/bootstrap/css/bs-sticky-footer.css
                    assets/themes/bootstrap-3/bootstrap/fonts/glyphicons-halflings-regular.eot
                    assets/themes/bootstrap-3/bootstrap/fonts/glyphicons-halflings-regular.svg
                    assets/themes/bootstrap-3/bootstrap/fonts/glyphicons-halflings-regular.ttf
                    assets/themes/bootstrap-3/bootstrap/fonts/glyphicons-halflings-regular.woff
                    assets/themes/bootstrap-3/bootstrap/js/bootstrap.min.js
                    ...done in 1.6939761 seconds.

      Regenerating: 8 file(s) changed at 2019-03-09 10:13:52
                    assets/themes/bootstrap/css/bootstrap-theme.min.css
                    assets/themes/bootstrap/css/bootstrap.min.css
                    assets/themes/bootstrap/css/bs-sticky-footer.css
                    assets/themes/bootstrap/fonts/glyphicons-halflings-regular.eot
                    assets/themes/bootstrap/fonts/glyphicons-halflings-regular.svg
                    assets/themes/bootstrap/fonts/glyphicons-halflings-regular.ttf
                    assets/themes/bootstrap/fonts/glyphicons-halflings-regular.woff
                    assets/themes/bootstrap/js/bootstrap.min.js
                    ...done in 0.6529467 seconds.

      Regenerating: 1 file(s) changed at 2019-03-09 10:18:27
                    assets/themes/bootstrap-3/css/style.css
                    ...done in 0.5091941 seconds.

      Regenerating: 1 file(s) changed at 2019-03-09 10:18:27
                    assets/themes/css/style.css
                    ...done in 0.5934166 seconds.                    
```

### Open Browser to keyin http://localhost:4000/
![alt tag](https://i.imgur.com/l11EzEe.jpg)

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

## ERROR `/assets/themes/bootstrap-3/bootstrap/css/bootstrap.min.css' not found
* [ERROR `/assets/themes/bootstrap/css/bootstrap.min.css' not found 22 Mar 2016](https://github.com/plusjade/jekyll-bootstrap/issues/302)
```
Thank u!
I create a folder bootstrap-3 and put bootstrap in there. Now it works well.
```
* [fixes bad path to old CSS/JS files (#302) #303](https://github.com/plusjade/jekyll-bootstrap/pull/303/commits/097e3305ec288aea14c324082351fbf54d2e5ef4)

##  Could not find gem 'pygments.rb' in any of the gem sources listed in your Gemfile. (Bundler::GemNotFound)
* [Could not find gem 'jekyll-sitemap' in any of the gem sources listed in your Gemfile or available on this machine. (Bundler::GemNotFound) #4972 1 Jun 2016](https://github.com/jekyll/jekyll/issues/4972)
```
$ sudo gem install jekyll-sitemap
$ sudo gem install pygments.rb
$ gem install bundler
$ bundle install
```


```
/usr/src/app # gem install pygments.rb
Fetching: multi_json-1.13.1.gem (100%)
Successfully installed multi_json-1.13.1
Fetching: pygments.rb-1.2.1.gem (100%)
Successfully installed pygments.rb-1.2.1
2 gems installed
```

## bootstrap 4.2.1 `bundle install` error occurred while installing ffi (1.10.0)
* [bootstrap 4.2.1 `bundle install` error occurred while installing ffi (1.10.0) Jan 13,2019](https://stackoverflow.com/questions/54165136/bootstrap-4-2-1-bundle-install-error-occurred-while-installing-ffi-1-10-0)
```
The suggestion by vladhadzhiyski to run brew reinstall libffi worked for me:
```

# Reference
* [docker-compose（dockerで十分）でGitHub Pagesローカル開発環境 2018-02-25](https://qiita.com/mkgask/items/7578bb0f9c646dbb68d0)
* [30分のチュートリアルでJekyllを理解する 13 May 2012](http://melborne.github.io/2012/05/13/first-step-of-jekyll/)
* [Setting up your GitHub Pages site locally with Jekyll - User Documentation](https://help.github.com/en/articles/setting-up-your-github-pages-site-locally-with-jekyll)

* [jekyllのローカル開発環境(プレビュー) 2017-11-11](https://qiita.com/t_732_twit/items/be50b8bded0d157ad5c9)
```docker-compose.yml

version: '2'
services:

  api:
    image: jekyll/jekyll
    container_name: github-blog-local
    volumes:
      - ./:/usr/src/app
    working_dir: /usr/src/app
    command: sh -c 'jekyll serve -s /usr/src/app --watch'
    ports:
      - "4000:4000"
```
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
```
![alt tag](https://wcc723.github.io/images/2014-01-12_114728.png)

* [簡單筆記一下 RubyGem, Gem, RVM, Gemfile, bundler December 15, 2014](http://yulin-learn-web-dev.logdown.com/posts/246089-brief-notes-of-rubygem-gem-rvm-gemfile-bundler)
```
Gemfile

Gemfile 在每個 Rails 新專案用 rails new 這個指令時會跟著產生，裡頭定義這個 rails app 用了哪些 gem 和指定版本
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

* [Jekyll serve suddenly switched from localhost to 0.0.0.0 - any fix? Jun 8 '14 ](https://stackoverflow.com/questions/24108302/jekyll-serve-suddenly-switched-from-localhost-to-0-0-0-0-any-fix)

* [如何更換jekyll 主題 2016-10-31](https://blog.jxtsai.info/2016/10/31/jekyll-theme/)

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
