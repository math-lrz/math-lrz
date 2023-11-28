demo site now [mirrored](https://weathered-bread-8229.on.fleek.co/) in [IPFS](https://github.com/ipfs/ipfs#quick-summary)!

# Jekyll theme: Adam Blog 2.0


## **第四步，jekyll的目录结构**

我们只需要关注几个核心的目录结构如下（可以自己创建）：

* `_layouts` （存放页面模板，md或html文件的内容会填充模板）
* `_sass`（存放样式表）
* `_includes` （可以复用在其它页面被include的html页面）
* `_posts`（博客文章页面）
* `assets`（原生的资源文件）
  * `js`
  * `css`
  * `image`
* `_config.yml` （全局配置文件）
* `index.html, index.md, README.md` （首页index.html优先级最高，如果没有index，默认启用README.md文件）
* `自定义文件和目录`

更多更详细的目录结构参看jekyll官网：[https://**jekyllrb.com/docs/struc**ture](https://jekyllrb.com/docs/structure)

## **第五步，jekyll的模板编程语言Liquid的使用**

* 变量 `{{ variable }}` 被嵌入在页面中，会在静态页面生成的时候被替换成具体的数值。常用的全局变量对象有：`site` 和 `page`。这两个对象有很多默认自带的属性，比如：`{{ site.time }}`，`{{ page.url }}`。更多的默认值参看：[https://**jekyllrb.com/docs/varia**bles](https://jekyllrb.com/docs/variables)。
* `site`对象对应的就是网站范围，自定义变量放在 `_config.yml`中，比如 `title:自定义标题`使用 `{{ site.title }}`访问。
* `page`对象对应的是单个页面，自定义变量放在每个页面的最开头，比如：

```text
---
myNum:100  
myStr:我是字符串
---
```

使用 `{{ page.myNum }}` 和 `{{ page.myStr }}` 访问。

* 条件判断语句，更多详见：[https://**shopify.github.io/liqui**d/tags/control-flow](https://shopify.github.io/liquid/tags/control-flow)

```php
{% if site.title == 'Awesome Shoes' %}   
   These shoes are awesome! 
{% endif %}  

{% if site.name == 'kevin' %}   
    Hey Kevin! 
{% elsif site.name == 'anonymous' %}   
    Hey Anonymous! 
{% else %}   
    Hi Stranger! 
{% endif %}
```

* 循环迭代，更多详见：[https://**shopify.github.io/liqui**d/tags/iteration](https://shopify.github.io/liquid/tags/iteration)

```php
{% for product in collection.products %}  
 {{ product.title }} 
{% endfor %}
```

* 默认函数，可以对变量进行一些处理，比如大小写转化、数学运算、格式化、排序等等，在Liquid中叫做Filters。比如 `{{ "Hello World!" | downcase }}`转换字符串为小写。更多内置函数详见：[https://**jekyllrb.com/docs/liqui**d/filters](https://jekyllrb.com/docs/liquid/filters)

## **第六步，使用_config.yml文件设置jekyll**

如果不是fork别人的仓库，就需要自己创建一个这个文件。然后，我们就可以配置一些默认的属性来控制jekyll的编译过程。更多详细的内置属性详见：[https://**jekyllrb.com/docs/confi**guration/default](https://jekyllrb.com/docs/configuration/default)

同时我们可以自定变量，会自动绑定到 `site`对象上，比如我们可以把导航配置到_config.yml中：

```php
nav:
- name: Home
  link: /
- name: About
  link: /about.html

// 以下嵌入页面，page.url以 "/" 开头
<nav>
  {% for item in site.nav %}
    <a href="{{ item.link }}" 
      {% if page.url == item.link %} style="color: red;" {% endif %}
    >
      {{ item.name }}
    </a>
  {% endfor %}
</nav>
```

当然，我们也可以把一些数据单独放入一个 `yml`文件，然后放在固定的数据文件夹 `_data`下，比如 `_data/navigation.yml`，这样访问这个文件的数据对象就是 `site.data.navigation`。

## **第七步，_layouts模板配置**

`_layouts`文件夹存放的是页面模板，默认需要一个 `default.html`，什么意思？就是说，layout提供一个页面的布局框架，这是固定的模式，包括样式、结构、布局、脚本控制等等。然后，我们在用其它md或html文件去动态填充这个框架，这样就形成了一个完整的页面。比如我的 `default.html`页面如下：

```html
<!doctype html>
<html lang="{{ page.lang | default: site.lang }}">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">

    {% seo %}

    <link rel="icon" href="https://avatars0.githubusercontent.com/u/1797320" type="image/x-icon" title="scottcgi">
    <link rel="stylesheet" href="{{ '/assets/css/style.css?v=' | append: site.github.build_revision | relative_url }}">
  
    <script src="{{ '/assets/js/scale.fix.js' | relative_url }}"></script>
    <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">

  </head>
  <body>
    <div class="wrapper">
      <header {% unless site.description or site.github.project_tagline %} class="without-description" {% endunless %}>
        <h1>{{ site.title | default: site.github.repository_name }}</h1> 
        <p>{{ page.description | default: site.description }}</p>
     
        <ul>
        {% for item in site.nav %}
          {% if page.url == item.link %}
            <li style="background-color:#069">
              <a href="{{ item.link }}">
                <strong>{{ item.name }}</strong>
              </a>
            </li>
          {% else %}
            <li><a href="{{ item.link }}">{{ item.name }}</a></li>
          {% endif %}
        {% endfor %}
        </ul>
      </header>
      <section>

      {{ content }}

      </section>
    </div>
  
    <footer>
      <p>{{ site.copyright | default: "copyright not found in _config.yml" }}</p>
    </footer>
  </body>
</html>
```

* `{% seo %}` 是jekyll的一个插件提供的seo优化，详情在这里：[https://**github.com/jekyll/jekyl**l-seo-tag](https://github.com/jekyll/jekyll-seo-tag)
* 核心在于 `{{ content }}` 这个变量是内置的，会用我们的md或html页面填充这部分内容。
* 其它的，我们看到会大量使用变量和流程控制代码，来填充模板的方方面面。
* 于是，填充模板的内容，一方面是来自读取配置文件的变量，一方面是来自 `_includes`的页面，还有就是来自 `{{ content }}` 对应的页面。

当然，我们也可以不使用 `{{ content }}` 来填充模板，而是使用 `_includes`的页面来代替 `{{ content }}` ，但这样不够灵活，因为使用 `{{ content }}`，我们可以在每个页面单独设置对应的 `layout`模板。

## **第八步，md和html页面编写**

站点内容页面，可以使用markdown或html来编写，但markdown编写的md文件，在浏览器地址访问的时候依然使用html文件后缀。推荐使用markdown来书写内容，语法参见：[Github md 示例](https://help.github.com/articles/basic-writing-and-formatting-syntax)和 [Github md 教程](https://guides.github.com/features/mastering-markdown)。比如下面这个About.md页面：

```text
---
layout: default
title: About
---
# About page

This page tells you a little bit about me.
```

`layout: default` 就是告诉jekyll这个页面使用哪个模板，即这个页面会放入哪个模板的 `{{ content }}`。当然，我们可以在 `_layouts`文件夹下提供多个不同的模板，然后根据需要不同的页面使用不同的 `layout`。

页面可以放在任意位置和目录，访问的时候从站点域名开始，带上目录名称，再次注意需要使用html结尾。如果想要自定义浏览器的访问路径，可以参看详细设置：[permalinks](https://jekyllrb.com/docs/permalinks)。

**md和html页面的区别：**

* md有自己的语法，可以使用少量的html标签，最终会编译成html，侧重于内容编写。
* html可以随意使用html标签，可以使用liquid模板语言，侧重于页面模板和功能控制。

至此，我们就可以在github上，新建md文件然后编辑提交，等待几分钟编译生成之后，就可以在浏览器里看到页面内容了。

## **第九步，博客文章编写和管理**

我们自然可以新建目录，提交文章，然后添加一个文章列表页面。但我们也可以把这些交给jekyll的内置机制来完成，因为它提供了一些方便的内置文章管理功能。

_posts文件夹是内置的放置文章的目录，我们可以将固定格式year-moth-day-name.md名称的md文件放到这里。比如新建一篇md的博客文章放到 `_posts`目录下：

```text
---
layout: post
--- 

这是一篇博客文章。
```

* 接下来我们需要添加一个 `post`的模板页面到 `_layouts`文件夹下面。

```html
---
layout: default
---

<h1>{{ page.title }}</h1>
<p>{{ page.date | date_to_string }} - {{ page.author }}</p>

{{ content }}
```

可见，模板页面本身也可以使用模板，这里 `post`使用了 `default`模板，而这里 `{{ content }}` 就会填充 `_posts`下面编写的页面（如果页面使用了 `layout: post`模板）。

* 最后，我们还需要编写一个博客文章列表的页面，用来展示所有的文章。比如在根目录新建 `blog.html`页面：

```html
---
layout: default
title: Blog
---

<h1>Latest Posts</h1>

<ul>
  {% for post in site.posts %}
    <li>
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      <p>{{ post.excerpt }}</p>
    </li>
  {% endfor %}
</ul>
```

* `site.posts` jekyll会自动生成 `_posts`目录的对象。
* `post.url`jekyll会自动会设置在 `_posts`目录下的页面url。
* `post.title` 默认是md文件名称，但也可以在文章页面自定义 `title: 我的文章自定义名称`。
* `post.excerpt` 默认是文章第一段的摘要文字。

## **第十步，Github Pages的限制**

Github Pages 并不是无限存储和无限流量的静态站点服务，一些限制如下：

* 内容存储不能超过1GB。
* 每个月100GB流量带宽。
* 每小时编译构建次数不超过10次。（在线修改重新编译并未发现这个限制）
* 更多参看官方说明：[usage-limits](https://help.github.com/articles/what-is-github-pages/#usage-limits)。


by [Armando Maynez](https://github.com/amaynez) based on [V1.0](https://github.com/artemsheludko/adam-blog) by [Artem Sheludko](https://github.com/artemsheludko).

Adam Blog 2.0 is a Jekyll theme that was built to be 100% compatible with [GitHub Pages](https://pages.github.com/). If you are unfamiliar with GitHub Pages, you can check out [their documentation](https://help.github.com/categories/github-pages-basics/) for more information. [Jonathan McGlone&#39;s guide](http://jmcglone.com/guides/github-pages/) on creating and hosting a personal site on GitHub is also a good resource.

### What is Jekyll?

Jekyll is a simple, blog-aware, static site generator for personal, project, or organization sites. Basically, Jekyll takes your page content along with template files and produces a complete website. For more information, visit the [official Jekyll site](https://jekyllrb.com/docs/home/) for their documentation. Codecademy also offers a great course on [how to deploy a Jekyll site](https://www.codecademy.com/learn/deploy-a-website) for complete beginners.

### Never Used Jekyll Before?

The beauty of hosting your website on GitHub is that you don't have to actually have Jekyll installed on your computer. Everything can be done through the GitHub code editor, with minimal knowledge of how to use Jekyll or the command line. All you have to do is add your posts to the `_posts` directory and edit the `_config.yml` file to change the site settings. With some rudimentary knowledge of HTML and CSS, you can even modify the site to your liking. This can all be done through the GitHub code editor, which acts like a content management system (CMS).

## Features of v2.0:

- SEO meta tags
- Dark mode ([configurable in _config.yml file](https://github.com/the-mvm/the-mvm.github.io/blob/a8d4f781bfbc4107b4842433701d28f5bbf1c520/_config.yml#L10))
- automatic [sitemap.xml](http://the-mvm.github.io/sitemap.xml)
- automatic [archive page](http://the-mvm.github.io/archive/) with infinite scrolling capability
- [new page](https://the-mvm.github.io/tag/?tag=Coding) of posts filtered by a single tag (without needing autopages from paginator V2), also with infinite scrolling
- click to tweet functionality (just add a `<tweet> </tweet>` tag in your markdown.
- custom and responsive [404 page](https://the-mvm.github.io/404.html)
- responsive and automatic Table of Contents (optional per post)
- read time per post automatically calculated
- responsive post tags and social share icons (sticky or inline)
- included linkedin, reddit and bandcamp icons
- *copy link to clipboard* sharing option (and icon)
- view on github link button (optional per post)
- MathJax support (optional per post)
- tag cloud in the home page
- 'back to top' button
- comments 'courtain' to mask the disqus interface until the user clicks on it ([configurable in _config.yml](https://github.com/the-mvm/the-mvm.github.io/blob/d4a67258912e411b639bf5acd470441c4c219544/_config.yml#L13))
- [CSS variables](https://github.com/the-mvm/the-mvm.github.io/blob/d4a67258912e411b639bf5acd470441c4c219544/assets/css/main.css#L8) to make it easy to customize all colors and fonts
- added several themes for code syntax highlight [configurable from the _config.yml file](https://github.com/the-mvm/the-mvm.github.io/blob/e146070e9348c2e8f46cb90e3f0c6eb7b59c041a/_config.yml#L44).
- responsive footer menu and footer logo ([if setup in the config file](https://github.com/the-mvm/the-mvm.github.io/blob/d4a67258912e411b639bf5acd470441c4c219544/_config.yml#L7))
- search shows results based on full post content, not just the description
- smoother menu animations

## Features preserved from v1.0

- [Google Fonts](https://fonts.google.com/)
- [Font Awesome icons](http://fontawesome.io/)
- [Disqus](https://disqus.com/)
- [MailChimp](https://mailchimp.com/)
- [Analytics](https://analytics.google.com/analytics/web/)
- [Search](https://github.com/christian-fei/Simple-Jekyll-Search)

## Demo

[Check the theme in action](https://the-mvm.github.io/)

The main page looks like this:

<img width="640px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/homepage-responsive.jpg?raw=true">

Dark mode selector in main menu:

<img width="560px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/light-toggle.png?raw=true">

The post page looks like:

<img width="540px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/post.jpg?raw=true">
<img width="540px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/post_bottom.jpg?raw=true">

Custom responsive 404:

<img width="640px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/404-responsive.jpg?raw=true">

Dark mode looks like this:

<img width="640px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/homepage-dark.png?raw=true">

<img width="640px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/post-dark.png?raw=true">
<img width="640px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/post_bottom-dark.png?raw=true">

# Installation

## Local Installation

For a full local installation of Adam Blog 2.0, [download your own copy of Adam Blog 2.0](https://github.com/the-mvm/the-mvm.github.io/archive/refs/heads/main.zip) and unzip it into it's own directory. From there, open up your favorite command line tool, enter `bundle install`, and then enter `jekyll serve`. Your site should be up and running locally at [http://localhost:4000](http://localhost:4000).

If you're completely new to Jekyll, I recommend checking out the documentation at [https://jekyllrb.com/](https://jekyllrb.com/) or there's a tutorial by [Smashing Magazine](https://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/).

If you are hosting your site on GitHub Pages, then committing a change to the `_config.yml` file (or any other file) will force a rebuild of your site with Jekyll. Any changes made should be viewable soon after. If you are hosting your site locally, then you must run `jekyll serve` again for the changes to take place.

Head over to the `_posts` directory to view all the posts that are currently on the website, and to see examples of what post files generally look like. You can simply just duplicate the template post and start adding your own content.

## GitHub Pages Installation

### **STEP 1.**

[Fork this repository](https://github.com/the-mvm/the-mvm.github.io/fork/) into your own account.

#### Using Github Pages

You can host your Jekyll site for free with Github Pages. [Click here](https://pages.github.com/) for more information.

 When forking, if you use as destination a repository named ``USERNAME.github.io`` then your url will be ``https://USERNAME.github.io/``, else ``https://USERNAME.github.io/REPONAME/``) and your site will be published to the gh-pages branch. Note: if you are hosting several sites under the same GitHub username, then you will have to use [Project Pages instead of User Pages](https://help.github.com/articles/user-organization-and-project-pages/) - just change the repository name to something other than 'http://USERNAME.github.io'.

##### A configuration tweak if you're using a gh-pages branch

In addition to your github-username.github.io repo that maps to the root url, you can serve up sites by using a gh-pages branch for other repos so they're available at github-username.github.io/repo-name.

This will require you to modify the `_config.yml` like so:

```yml
# Site settings
title: Repo Name
email: your_email@example.com
author: Your Name
description: "Repo description"
baseurl: "/repo-name"
url: "https://github-username.github.io"
```

This will ensure that the the correct relative path is constructed for your assets and posts.

### **STEP 2.**

Modify ``_config.yml`` file, located in the root directory, with your data.

```YAML
# Site settings
title: The Title for Your Website
description: 'A description of your blog'
permalink: ':title:output_ext' # how the permalinks will behave
baseurl: "/" # the subpath of your site, e.g. /blog
url: "" # the base hostname & protocol for your site, e.g. http://example.com
logo: "" # the logo for your site
logo-icon: "" # a smaller logo, typically squared
logo-icon-SEO: "" # must be a non SVG file, could be the same as the logo-icon

# Night/Dark mode default mode is "auto", "auto" is for auto nightshift (19:00 - 07:00), "manual" is for manual toggle, and "on/off" is for default on/off. Whatever the user's choice is, it will supersede the default setting of the site and be kept during the visit (session). Only the dark mode setting is "manual", it will be always kept on every visit (i.e. no matter the browser is closed or not)
night_mode: "auto"
logo-dark: "/assets/img/branding/MVM-logo-full-dark.svg" #if you want to display a different logo when in dark mode
highlight_theme: syntax-base16.monokai.dark # select a dark theme for the code highlighter if needed


# Author settings
author: Your Name # add your name
author-pic: '' # a picture of you
about-author: '' # a brief description of you

# Contact links
email: your@email.com # Add your Email address
phone: # Add your Phone number
website:  # Add your website
linkedin:  # Add your Linkedin handle
github:  # Add your Github handle
twitter:  # Add your Twitter handle
bandcamp:  # Add your Bandcamp username

# Tracker
analytics: # Google Analytics tag ID
fbadmin: # Facebook ID admin

# Paginate
paginate: 6 # number of items to show in the main page
paginate_path: 'page:num'
words_per_minute: 200 # default words per minute to be considered when calculating the read time of the blog posts
```

### **STEP 3.**

To configure the newsletter, please create an account in https://mailchimp.com, set up a web signup form and paste the link from the embed signup form in the `config.yml` file:

```YAML
# Newsletter
mailchimp: "https://github.us1.list-manage.com/subscribe/post?u=8ece198b3eb260e6838461a60&id=397d90b5f4"
```

### **STEP 4.**

To configure Disqus, set up a [Disqus site](https://disqus.com/admin/create/) with the same name as your site. Then, in `_config.yml`, edit the `disqus_identifier` value to enable.

```YAML
# Disqus
discus_identifier:  # Add your discus identifier
comments_curtain: yes # leave empty to show the disqus embed directly
```

More information on [how to set up Disqus](http://www.perfectlyrandom.org/2014/06/29/adding-disqus-to-your-jekyll-powered-github-pages/).

### **STEP 5.**

Customize the site colors. Modify `/assets/css/main.css` as follows:

```CSS
html {
  --shadow:       rgba(32,30,30,.3);
  --accent:       #DB504A;    /* accent */
  --accent-dark:  #4e3e51;    /* accent 2 (dark) */
  --main:         #326273;    /* main color */
  --main-dim:     #879dab;    /* dimmed version of main color */
  --text:         #201E1E;
  --grey1:        #5F5E58;
  --grey2:        #8D897C;
  --grey3:        #B4B3A7;
  --grey4:        #DAD7D2;
  --grey5:        #F0EFED;
  --background:   #ffffff;
}

html[data-theme="dark"]  {
  --accent:       #d14c47;    /* accent */
  --accent-dark:  #CD8A7A;    /* accent 2 (dark) */
  --main:         #4C6567;    /* main color */
  --main-dim:     #273335;    /* dimmed version of main color */
  --text:         #B4B3A7;
  --grey1:        #8D897C;
  --grey2:        #827F73;
  --grey3:        #76746A;
  --grey4:        #66645D;
  --grey5:        #4A4945;
  --background:   #201E1E;
  --shadow:       rgba(180,179,167,.3);
}
```

### **STEP 6.**

Customize the site fonts. Modify `/assets/css/main.css` as follows:

```CSS
...
  --font1: 'Lora', charter, Georgia, Cambria, 'Times New Roman', Times, serif;/* body text */
  --font2: 'Source Sans Pro', 'Helvetica Neue', Helvetica, Arial, sans-serif; /* headers and titles   */
  --font1-light:      400;
  --font1-regular:    400;
  --font1-bold:       600;
  --font2-light:      200;
  --font2-regular:    400;
  --font2-bold:       700;
...
```

If you change the fonts, you need to also modify `/_includes/head.html` as follows:
Uncomment and change the following line with your new fonts and font weights:

```HTML
<link href="https://fonts.googleapis.com/css?family=Lora:400,600|Source+Sans+Pro:200,400,700" rel="stylesheet">
```

Delete everything within `<style></style>` just before the line above:

```HTML
<style>
/* latin */
@font-face {
  font-family: 'Lora';
  ...
</style>
```

### **STEP 7.**

You will find example posts in your `/_posts/` directory. Go ahead and edit any post and re-build the site to see your changes, for github pages, this happens automatically with every commit. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention of `YYYY-MM-DD-name-of-post.md` and includes the necessary front matter. Take a look at any sample post to get an idea about how it works. If you already have a website built with Jekyll, simply copy over your posts to migrate to Adam Blog 2.0.

The front matter options for each post are:

```YAML
---
layout: post #ensure this one stays like this
read_time: true # calculate and show read time based on number of words
show_date: true # show the date of the post
title:  Your Blog Post Title
date:   XXXX-XX-XX XX:XX:XX XXXX
description: "The description of your blog post"
img: # the path for the hero image, from the image folder (if the image is directly on the image folder, just the filename is needed)
tags: [tags, of, your, post]
author: Your Name
github: username/reponame/ # set this to show a github button on the post
toc: yes # leave empty or erase for no table of contents
---
```

Edit your blogpost using markdown. [Here is a good guide about how to use it.](https://www.markdownguide.org/)

### **STEP 7.**

Delete images inside of ``/assets/img/posts/`` and upload your own images for your posts.

### **STEP 8.**

Make sure Github Pages are turned on in the repository settings, and pointing to the main or master branch (where you cloned this repo).

## Additional documentation

### Directory Structure

If you are familiar with Jekyll, then the Adam Blog 2.0 directory structure shouldn't be too difficult to navigate. The following some highlights of the differences you might notice between the default directory structure. More information on what these folders and files do can be found in the [Jekyll documentation site](https://jekyllrb.com/docs/structure/).

```bash
Adam Blog 2.0/
├── _includes                  # Theme includes
├── _layouts                   # Theme layouts (see below for details)
├── _posts                     # Where all your posts will go
├── assets                     # Style sheets and images are found here
|  ├── css                     # Style sheets go here
|  |  └── _sass                # Folder containing SCSS files
|  |  └── main.css             # Main SCSS file
|  |  └── highlighter          # Style sheet for code syntax highlighting
|  └── img                     # 
|     └── posts                # Images go here
├── _pages                     # Website pages (that are not posts)
├── _config.yml                # Site settings
├── Gemfile                    # Ruby Gemfile for managing Jekyll plugins
├── index.html                 # Home page
├── LICENSE.md                 # License for this theme
├── README.md                  # Includes all of the documentation for this theme
├── feed.xml                   # Generates atom file which Jekyll points to
├── 404.html                   # custom and responsive 404 page
├── all-posts.json             # database of all posts used for infinite scroll
├── ipfs-404.html              # 404 page for IPFS
├── posts-by-tag.json          # database of posts by tag
├── robots.txt                 # SEO crawlers exclusion file
├── search.json                # database of posts used for search
└── sitemap.xml                # automatically generated sitemap for search engines
```

### Starting From Scratch

To completely start from scratch, simply delete all the files in the `_posts`, `assets/img/posts` folders, and add your own content. Everything in the `_config.yml` file can be edited to suit your needs. Also change the `favicon.ico` file to your own favicon.

### Click to tweet

If you have a tweetable quote in your blog post and wish to feature it as a click to tweet block, you just have to use the `<tweet></tweet>` tags, everything between them will be converted in a click to tweet box.

<img width="640px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/ctt-markdown.png?raw=true">

<img width="640px" src="https://github.com/the-mvm/the-mvm.github.io/blob/main/assets/img/template_screenshots/ctt-render.png?raw=true">

### Google Analytics

It is possible to track your site statistics through [Google Analytics](https://www.google.com/analytics/). Similar to Disqus, you will have to create an account for Google Analytics, and enter the correct Google ID for your site under `google-ID` in the `_config.yml` file. More information on [how to set up Google Analytics](https://michaelsoolee.com/google-analytics-jekyll/).

### Atom Feed

Atom is supported by default through [jekyll-feed](https://github.com/jekyll/jekyll-feed). With jekyll-feed, you can set configuration variables such as 'title', 'description', and 'author', in the `_config.yml` file.

Your atom feed file will be live at `https://your.site/feed.xml` [example](https://the-mvm.github.io/feed.xml).

### Social Media Icons

All social media icons are courtesy of [Font Awesome](http://fontawesome.io/). You can change which icons appear, as well as the account that they link to, in the `_config.yml` file.

### MathJax

Adam Blog 2.0 comes out of the box with [MathJax](https://www.mathjax.org/), which allows you to display mathematical equations in your posts through the use of [LaTeX](http://www.andy-roberts.net/writing/latex/mathematics_1). Just add `Mathjax: yes` in the frontmatter of your post.

```markdown
<p style="text-align:center">
\(\theta_{t+1} = \theta_{t} - \dfrac{\eta}{\sqrt{\hat{v}_t} + \epsilon} \hat{m}_t\).
</p>
```

![rendered mathjax](/assets/img/template_screenshots/MathjaxRendered.jpg)

### Syntax Highlighting

Adam Blog 2.0 provides syntax highlighting through [fenced code blocks](https://help.github.com/articles/creating-and-highlighting-code-blocks/). Syntax highlighting allows you to display source code in different colors and fonts depending on what programming language is being displayed. You can find the full list of supported programming languages [here](https://github.com/jneen/rouge/wiki/List-of-supported-languages-and-lexers). Another option is to embed your code through [Gist](https://en.support.wordpress.com/gist/).

You can choose the color theme for the syntax highlight in the `_config.yml` file:

```YAML
highlight_theme: syntax-base16.monokai.dark # select a theme for the code highlighter
```

See the [highlighter directory](https://github.com/the-mvm/the-mvm.github.io/tree/main/assets/css/highlighter) for reference on the options.

### Markdown

Jekyll offers support for GitHub Flavored Markdown, which allows you to format your posts using the [Markdown syntax](https://guides.github.com/features/mastering-markdown/).

## Everything Else

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll's GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

## Contributing

If you would like to make a feature request, or report a bug or typo in the documentation, then please [submit a GitHub issue](https://github.com/the-mvm/the-mvm.github.io/issues/new). If you would like to make a contribution, then feel free to [submit a pull request](https://help.github.com/articles/about-pull-requests/) - as a bonus, I will credit all contributors below! If this is your first pull request, it may be helpful to read up on the [GitHub Flow](https://guides.github.com/introduction/flow/) first.

Adam Blog 2.0 has been designed as a base for users to customize and fit to their own unique needs. Please keep this in mind when requesting features and/or submitting pull requests. Some examples of changes that I would love to see are things that would make the site easier to use, or better ways of doing things. Please avoid changes that do not benefit the majority of users.

## Questions?

This theme is completely free and open source software. You may use it however you want, as it is distributed under the [MIT License](http://choosealicense.com/licenses/mit/). If you are having any problems, any questions or suggestions, feel free to  [file a GitHub issue](https://github.com/the-mvm/the-mvm.github.io/issues/new).

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]: https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
