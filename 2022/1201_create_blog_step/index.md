# 记录搭建博客的详细步骤


> 博客差不多弄完了（主题还有很多功能配置有待发现），完整记录下整个过程吧，方便同样想搭建自己的博客的朋友参考，也方便自己后面有些操作忘记了再看吧

一开始的选择主题是`Leavelt`，评论系统是`utteranc`，
但是这俩都不太理想，`Leavelt`的代码块样式不是很好，`utteranc`不能楼中楼回复，也不能自适应黑夜模式

最后使用到的全部功能组件有：
```text
- Hugo          (博客系统)
- FixIt         (博客主题)
- giscus        (评论系统)
- algolia       (搜索引擎)
- GitHub Pages  (静态网站托管平台)
- Github Action (网站自动部署工具)
- 百度统计       (数据分析系统)
```

## 准备环境

### 安装Hugo & Git
{{< admonition info "注意" true >}}
Windows需要安装`extended`版本，主题需要

我是用的scoop包管理工具，有`hugo`和`hugo-extended`两个版本
{{< /admonition>}}
- 安装Hugo: https://gohugo.io/installation/
- 安装Git: https://git-scm.com/

### 创建站点
```shell
hugo new site blog
cd blog
git init 
# 添加FixIt主题
git submodule add https://github.com/hugo-fixit/FixIt.git themes/FixIt
```

> **目录结构**
```text
blog
├── archetypes      # 配置文章模板
├── assets          # 第三方库文件
├── config.toml     # 站点配置文件
├── content         # 文章页面内容
├── data            # 站点数据文件
├── layouts         # 页面布局源码，改造主题可不动主题源码
├── public          # 博客构建部署目录
├── static          # 静态文件存放
└── themes          # 主题目录
```

> **config.toml详细配置**
> 
> 主题给的默认配置怎么说呢，感觉坑有点多，踩了好久，这份配置文件结合了主题本身的配置，主题作者的配置，和一个网友的配置(网友已经忘记是谁了)

```toml
baseURL = "https://heelie.github.io/"
title = "Jay's blog"
theme = "FixIt"
LanguageCode = "zh-CN"
defaultContentLanguage = "zh-cn"

paginate = 12

enableEmoji = true
hasCJKLanguage = true
preserveTaxonomyNames = true

[menu]
  [[menu.main]]
    identifier = "posts"
    # you can add extra information before the name (HTML format is supported), such as icons
    pre = ""
    # you can add extra information after the name (HTML format is supported), such as icons
    post = ""
    name = "文章"
    url = "/posts/"
    # title will be shown when you hover on this menu link
    title = ""
    weight = 1
    # FixIt 0.2.14 | NEW add user-defined content to menu items
    [menu.main.params]
      # add css class to a specific menu item
      class = ""
      # whether set as a draft menu item whose function is similar to a draft post/page
      draft = false
      # FixIt 0.2.16 | NEW add fontawesome icon to a specific menu item
      icon = "fa-solid fa-archive"
      # FixIt 0.2.16 | NEW set menu item type, optional values: ["mobile", "desktop"]
      type = ""
  [[menu.main]]
    identifier = "categories"
    pre = ""
    post = ""
    name = "分类"
    url = "/categories/"
    title = ""
    weight = 2
    [menu.main.params]
      icon = "fa-solid fa-th"
  [[menu.main]]
    identifier = "tags"
    pre = ""
    post = ""
    name = "标签"
    url = "/tags/"
    title = ""
    weight = 3
    [menu.main.params]
      icon = "fa-solid fa-tags"
  [[menu.main]]
    identifier = "favorites"
    pre = ""
    post = ""
    name = "收藏"
    url = "/favorites/"
    title = ""
    weight = 4
    [menu.main.params]
      icon = "fa-solid fa-book-bookmark"
  [[menu.main]]
    identifier = "friends"
    pre = ""
    post = ""
    name = "友链"
    url = "/friends/"
    title = "友情链接"
    weight = 5
    [menu.main.params]
      icon = "fa-solid fa-users"
  [[menu.main]]
    identifier = "about"
    pre = ""
    post = ""
    name = "关于我"
    url = "/about/"
    title = ""
    weight = 6
    [menu.main.params]
      icon = "fa-solid fa-person"

# -------------------------------------------------------------------------------------
# Theme Core Configuration Settings
# -------------------------------------------------------------------------------------

[params]
  # FixIt 0.2.15 | CHANGED FixIt theme version
  version = "0.2.X" # e.g. "0.2.X", "0.2.15", "v0.2.15" etc.
  # site description
  description = "Getting Things Done."
  # site keywords
  keywords = ["Hugo", "FixIt"]
  # site default theme ["light", "dark", "auto"]
  defaultTheme = "auto"
  # public git repo url only then enableGitInfo is true
  gitRepo = ""
  # FixIt 0.1.1 | NEW which hash function used for SRI, when empty, no SRI is used
  # ["sha256", "sha384", "sha512", "md5"]
  fingerprint = ""
  # FixIt 0.2.0 | NEW date format
  dateFormat = "2006-01-02"
  # website images for Open Graph and Twitter Cards
  images = []
  # FixIt 0.2.12 | NEW enable PWA
  enablePWA = false
  # FixIt 0.2.14 | NEW whether to add external Icon for external links automatically
  externalIcon = false
  # FixIt 0.2.14 | NEW FixIt will, by default, inject a theme meta tag in the HTML head on the home page only.
  # You can turn it off, but we would really appreciate if you don’t, as this is a good way to watch FixIt's popularity on the rise.
  disableThemeInject = false

  # FixIt 0.2.0 | NEW App icon config
  [params.app]
    # optional site title override for the app when added to an iOS home screen or Android launcher
    title = "FixIt"
    # whether to omit favicon resource links
    noFavicon = false
    # modern SVG favicon to use in place of older style .png and .ico files
    svgFavicon = ""
    # Safari mask icon color
    iconColor = "#5bbad5"
    # Windows v8-10 tile color
    tileColor = "#da532c"
    # FixIt 0.2.12 | CHANGED Android browser theme color
    [params.app.themeColor]
      light = "#f8f8f8"
      dark = "#252627"

  # FixIt 0.2.0 | NEW Search config
  [params.search]
    enable = true
    # type of search engine ["lunr", "algolia"]
    type = "algolia"
    # max index length of the chunked content
    contentLength = 4000
    # placeholder of the search bar
    placeholder = ""
    # FixIt 0.2.1 | NEW max number of results length
    maxResultLength = 10
    # FixIt 0.2.3 | NEW snippet length of the result
    snippetLength = 30
    # FixIt 0.2.1 | NEW HTML tag name of the highlight part in results
    highlightTag = "em"
    # FixIt 0.2.4 | NEW whether to use the absolute URL based on the baseURL in search index
    absoluteURL = false
    [params.search.algolia]
      index = ""
      appID = ""
      searchKey = ""

  # Header config
  [params.header]
    # FixIt 0.2.13 | CHANGED desktop header mode ["sticky", "normal", "auto"]
    desktopMode = "sticky"
    # FixIt 0.2.13 | CHANGED mobile header mode ["sticky", "normal", "auto"]
    mobileMode = "auto"
    # FixIt 0.2.0 | NEW Header title config
    [params.header.title]
      # URL of the LOGO
      logo = "/favicon.png"
      # title name
      name = "Jay's"
      # you can add extra information before the name (HTML format is supported), such as icons
      pre = ""
      # you can add extra information after the name (HTML format is supported), such as icons
      post = ""
      # FixIt 0.2.5 | NEW whether to use typeit animation for title name
      typeit = false
    # FixIt 0.2.12 | NEW Header subtitle config
    [params.header.subtitle]
      # subtitle name
      name = "Blog"
      # whether to use typeit animation for subtitle name
      typeit = false

  # Footer config
  [params.footer]
    enable = true
    # FixIt 0.2.0 | NEW whether to show Hugo and theme info
    hugo = false
    # FixIt 0.2.0 | NEW whether to show copyright info
    copyright = true
    # FixIt 0.2.0 | NEW whether to show the author
    author = true
    # Site creation year
    since = 2022
    # FixIt 0.2.14 | NEW Site creation time
    siteTime = "" # e.g. "2021-12-18T16:15:22+08:00"
    # FixIt 0.2.14 | NEW whether to show total word count of site content
    wordCount = true
    # FixIt 0.2.12 | NEW Public network security only in China (HTML format is supported)
    gov = ""
    # ICP info only in China (HTML format is supported)
    icp = ""
    # license info (HTML format is supported)
    license = '<a rel="license external nofollow noopener noreferrer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>'

  # FixIt 0.2.0 | NEW Section (all posts) page config
  [params.section]
    # special amount of posts in each section page
    paginate = 20
    # date format (month and day)
    dateFormat = "01-02"
    # amount of RSS pages
    rss = 10
    # FixIt 0.2.13 | NEW recently updated posts settings
    [params.section.recentlyUpdated]
      enable = false
      rss = false
      days = 30
      maxCount = 10

  # FixIt 0.2.0 | NEW List (category or tag) page config
  [params.list]
    # special amount of posts in each list page
    paginate = 20
    # date format (month and day)
    dateFormat = "01-02"
    # amount of RSS pages
    rss = 10

  # FixIt 0.2.17 | NEW TagCloud config for tags page
  [params.tagcloud]
    enable = false
    min = 14 # Minimum font size in px
    max = 32 # Maximum font size in px
    peakCount = 10 # Maximum count of posts per tag
    orderby = "name" # Order of tags, optional values: ["name", "count"]

  # Home page config
  [params.home]
    # FixIt 0.2.0 | NEW amount of RSS pages
    rss = 10
    # Home page profile
    [params.home.profile]
      enable = true
      # Gravatar Email for preferred avatar in home page
      gravatarEmail = ""
      # URL of avatar shown in home page
      avatarURL = "/favicon.png"
      # FixIt 0.2.17 | NEW identifier of avatar menu link
      avatarMenu = ""
      # FixIt 0.2.7 | CHANGED title shown in home page (HTML format is supported)
      title = ""
      # subtitle shown in home page
      subtitle = "Getting Things Done."
      # whether to use typeit animation for subtitle
      typeit = true
      # whether to show social links
      social = true
      # FixIt 0.2.0 | NEW disclaimer (HTML format is supported)
      disclaimer = ""
    # Home page posts
    [params.home.posts]
      enable = false
      # special amount of posts in each home posts page
      paginate = 6

  # FixIt 0.2.16 | CHANGED Social config about the author
  [params.social]
    GitHub = "Heelie"
    Linkedin = ""
    Twitter = ""
    Instagram = ""
    Facebook = ""
    Telegram = ""
    Medium = ""
    Gitlab = ""
    Youtubelegacy = ""
    Youtubecustom = ""
    Youtubechannel = ""
    Tumblr = ""
    Quora = ""
    Keybase = ""
    Pinterest = ""
    Reddit = ""
    Codepen = ""
    FreeCodeCamp = ""
    Bitbucket = ""
    Stackoverflow = ""
    Weibo = ""
    Odnoklassniki = ""
    VK = ""
    Flickr = ""
    Xing = ""
    Snapchat = ""
    Soundcloud = ""
    Spotify = ""
    Bandcamp = ""
    Paypal = ""
    Fivehundredpx = ""
    Mix = ""
    Goodreads = ""
    Lastfm = ""
    Foursquare = ""
    Hackernews = ""
    Kickstarter = ""
    Patreon = ""
    Steam = ""
    Twitch = ""
    Strava = ""
    Skype = ""
    Whatsapp = ""
    Zhihu = ""
    Douban = ""
    Angellist = ""
    Slidershare = ""
    Jsfiddle = ""
    Deviantart = ""
    Behance = ""
    Dribbble = ""
    Wordpress = ""
    Vine = ""
    Googlescholar = ""
    Researchgate = ""
    Mastodon = ""
    Thingiverse = ""
    Devto = ""
    Gitea = ""
    XMPP = ""
    Matrix = ""
    Bilibili = ""
    ORCID = ""
    Liberapay = ""
    Ko-Fi = ""
    BuyMeaCoffee = ""
    Linktree = ""
    QQ = ""
    QQGroup = "" # https://qun.qq.com/join.html
    Diaspora = ""
    CSDN = ""
    Discord = ""
    DiscordInvite = ""
    Lichess = ""
    Pleroma = ""
    Kaggle = ""
    MediaWiki= ""
    Plume = ""
    HackTheBox = ""
    RootMe = ""
    Feishu = ""
    TryHackMe = ""
    Phone = ""
    Email = ""
    RSS = true

  # FixIt 0.2.0 | CHANGED Page config
  [params.page]
    # FixIt 0.2.0 | NEW whether to hide a page from home page
    hiddenFromHomePage = false
    # FixIt 0.2.0 | NEW whether to hide a page from search results
    hiddenFromSearch = false
    # FixIt 0.2.0 | NEW whether to enable twemoji
    twemoji = false
    # whether to enable lightgallery
    lightgallery = false
    # FixIt 0.2.0 | NEW whether to enable the ruby extended syntax
    ruby = true
    # FixIt 0.2.0 | NEW whether to enable the fraction extended syntax
    fraction = true
    # FixIt 0.2.0 | NEW whether to enable the fontawesome extended syntax
    fontawesome = true
    # license info (HTML format is supported)
    license = '<a rel="license external nofollow noopener noreferrer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>'
    # whether to show link to Raw Markdown content of the content
    linkToMarkdown = true
    # FixIt 0.2.4 | NEW whether to show the full text content in RSS
    rssFullText = false
    # FixIt 0.2.13 | NEW Page style ["narrow", "normal", "wide", ...]
    pageStyle = "narrow"
    # FixIt 0.2.14 | NEW Gravatar is force-used as the author's avatar
    gravatarForce = false
    # FixIt 0.2.17 | CHANGED Auto Bookmark Support
    # If true, save the reading progress when closing the page.
    autoBookmark = false
    # FixIt 0.2.17 | NEW whether to enable wordCount
    wordCount = true
    # FixIt 0.2.17 | NEW whether to enable readingTime
    readingTime = true
    # FixIt 0.2.17 | NEW end of post flag
    endFlag = ""

    # FixIt 0.2.15 | NEW Repost config
    [params.page.repost]
      enable = false
      url = ""
    # FixIt 0.2.0 | NEW Table of the contents config
    [params.page.toc]
      # whether to enable the table of the contents
      enable = true
      # FixIt 0.2.9 | NEW whether to keep the static table of the contents in front of the post
      keepStatic = false
      # whether to make the table of the contents in the sidebar automatically collapsed
      auto = true
      # FixIt 0.2.13 | NEW position of TOC ["left", "right"]
      position = "right"
    # FixIt 0.2.13 | NEW Display a message at the beginning of an article to warn the reader that its content might be expired
    [params.page.expirationReminder]
      enable = false
      # Display the reminder if the last modified time is more than 90 days ago
      reminder = 90
      # Display warning if the last modified time is more than 180 days ago
      warning = 180
      # If the article expires, close the comment or not
      closeComment = false
    # FixIt 0.2.16 | CHANGED KaTeX mathematical formulas (https://katex.org)
    [params.page.math]
      enable = true
      # default inline delimiter is $ ... $ and \( ... \)
      inlineLeftDelimiter = ""
      inlineRightDelimiter = ""
      # default block delimiter is $$ ... $$, \[ ... \], \begin{equation} ... \end{equation} and some other functions
      blockLeftDelimiter = ""
      blockRightDelimiter = ""
      # KaTeX extension copy_tex
      copyTex = true
      # KaTeX extension mhchem
      mhchem = true
    # FixIt 0.2.0 | NEW Code config
    [params.page.code]
      # whether to show the copy button of the code block
      copy = true
      # FixIt 0.2.13 | NEW whether to show the edit button of the code block
      edit = true
      # the maximum number of lines of displayed code by default
      maxShownLines = 10
    # FixIt 0.2.14 | NEW Post edit
    [params.page.edit]
      enable = false
      # FixIt 0.2.15 | CHANGED Link for fork & edit
      # url = "/edit/branch-name/subdirectory-name" # base on `params.gitRepo`
      # url = "https://github.com/user-name/repo-name/edit/branch-name/subdirectory-name" # full url
      url = ""
    # FixIt 0.2.0 | NEW Mapbox GL JS config (https://docs.mapbox.com/mapbox-gl-js)
    [params.page.mapbox]
      # access token of Mapbox GL JS
      accessToken = ""
      # style for the light theme
      lightStyle = "mapbox://styles/mapbox/light-v9"
      # style for the dark theme
      darkStyle = "mapbox://styles/mapbox/dark-v9"
      # whether to add NavigationControl
      navigation = true
      # whether to add GeolocateControl
      geolocate = true
      # whether to add ScaleControl
      scale = true
      # whether to add FullscreenControl
      fullscreen = true
    # FixIt 0.2.17 | NEW Donate (Sponsor) settings
    [params.page.reward]
      enable = false
      animation = false
      # position relative to post footer, optional values: ["before", "after"]
      position = "after"
      # comment = "Buy me a coffee"
      [params.page.reward.ways]
        # wechatpay = "/images/wechatpay.png"
        # alipay = "/images/alipay.png"
        # paypal = "/images/paypal.png"
        # bitcoin = "/images/bitcoin.png"
    # FixIt 0.2.0 | CHANGED social share links in post page
    [params.page.share]
      enable = false
      Twitter = false
      Facebook = false
      Linkedin = false
      Whatsapp = false
      Pinterest = false
      Tumblr = false
      HackerNews = false
      Reddit = false
      VK = false
      Buffer = false
      Xing = false
      Line = false
      Instapaper = false
      Pocket = false
      Digg = false
      Stumbleupon = false
      Flipboard = false
      Weibo = true
      Renren = false
      Myspace = false
      Blogger = false
      Baidu = false
      Odnoklassniki = false
      Evernote = false
      Skype = false
      Trello = false
      Mix = false
    # FixIt 0.2.15 | CHANGED Comment config
    [params.page.comment]
      enable = true
      # FixIt 0.2.13 | NEW Artalk comment config (https://artalk.js.org/)
      [params.page.comment.artalk]
        enable = false
        server = "https://yourdomain/api/"
        site = "默认站点"
        placeholder = ""
        noComment = ""
        sendBtn = ""
        editorTravel = true
        flatMode = "auto"
        maxNesting = 3
        # It take effect when `params.page.lightgallery` is enabled
        lightgallery = false
        locale = "" # FixIt 0.2.15 | NEW
      # FixIt 0.1.1 | NEW Disqus comment config (https://disqus.com)
      [params.page.comment.disqus]
        enable = false
        # Disqus shortname to use Disqus in posts
        shortname = ""
      # FixIt 0.1.1 | NEW Gitalk comment config (https://github.com/gitalk/gitalk)
      [params.page.comment.gitalk]
        enable = false
        owner = ""
        repo = ""
        clientId = ""
        clientSecret = ""
      # Valine comment config (https://github.com/xCss/Valine)
      [params.page.comment.valine]
        enable = false
        appId = ""
        appKey = ""
        placeholder = ""
        avatar = "mp"
        meta= ""
        pageSize = 10
        lang = ""
        visitor = true
        recordIP = true
        highlight = true
        enableQQ = false
        serverURLs = ""
        # FixIt 0.2.6 | NEW emoji data file name, default is "google.yml"
        # ["apple.yml", "google.yml", "facebook.yml", "twitter.yml"]
        # located in "themes/FixIt/assets/lib/valine/emoji/" directory
        # you can store your own data files in the same path under your project:
        # "assets/lib/valine/emoji/"
        emoji = ""
        commentCount = true # FixIt 0.2.13 | NEW
      # FixIt 0.2.16 | CHANGED Waline comment config (https://waline.js.org)
      [params.page.comment.waline]
        enable = false
        serverURL = ""
        pageview = false # FixIt 0.2.15 | NEW
        emoji = ["//unpkg.com/@waline/emojis@1.1.0/weibo"]
        meta = ["nick", "mail", "link"]
        requiredMeta = []
        login = "enable"
        wordLimit = 0
        pageSize = 10
        imageUploader = false # FixIt 0.2.15 | NEW
        highlighter = false # FixIt 0.2.15 | NEW
        comment = false # FixIt 0.2.15 | NEW
        texRenderer = false # FixIt 0.2.16 | NEW
        search = false # FixIt 0.2.16 | NEW
        recaptchaV3Key = "" # FixIt 0.2.16 | NEW
      # Facebook comment config (https://developers.facebook.com/docs/plugins/comments)
      [params.page.comment.facebook]
        enable = false
        width = "100%"
        numPosts = 10
        appId = ""
        languageCode = ""
      # FixIt 0.2.0 | NEW Telegram comments config (https://comments.app)
      [params.page.comment.telegram]
        enable = false
        siteID = ""
        limit = 5
        height = ""
        color = ""
        colorful = true
        dislikes = false
        outlined = false
      # FixIt 0.2.0 | NEW Commento comment config (https://commento.io)
      [params.page.comment.commento]
        enable = false
      # FixIt 0.2.5 | NEW Utterances comment config (https://utteranc.es)
      [params.page.comment.utterances]
        enable = false
        # owner/repo
        repo = ""
        issueTerm = "pathname"
        label = ""
        lightTheme = "github-light"
        darkTheme = "github-dark"
      # FixIt 0.2.13 | NEW Twikoo comment config (https://twikoo.js.org/)
      [params.page.comment.twikoo]
        enable = false
        envId = ""
        region = ""
        path = ""
        visitor = true
        commentCount = true
        # It take effect when `params.page.lightgallery` is enabled
        lightgallery = false
      # FixIt 0.2.14 | NEW Giscus comments config
      [params.page.comment.giscus]
        enable = true
        repo = ""
        repoId = ""
        category = ""
        categoryId = ""
        mapping = "pathname"
        reactionsEnabled = "1"
        emitMetadata = "0"
        inputPosition = "bottom" # top, bottom
        lightTheme = "light"
        darkTheme = "dark"
        lazyLoad = true
    # FixIt 0.2.7 | NEW Third-party library config
    [params.page.library]
      [params.page.library.css]
        # someCSS = "some.css"
        # located in "assets/"
        # Or
        # someCSS = "https://cdn.example.com/some.css"
      [params.page.library.js]
        # someJavascript = "some.js"
        # located in "assets/"
        # Or
        # someJavascript = "https://cdn.example.com/some.js"
    # FixIt 0.2.10 | CHANGED Page SEO config
    [params.page.seo]
      # image URL
      images = []
      # Publisher info
      [params.page.seo.publisher]
        name = ""
        logoUrl = ""

  # FixIt 0.2.5 | NEW TypeIt config
  [params.typeit]
    # typing speed between each step (measured in milliseconds)
    speed = 100
    # blinking speed of the cursor (measured in milliseconds)
    cursorSpeed = 1000
    # character used for the cursor (HTML format is supported)
    cursorChar = "|"
    # cursor duration after typing finishing (measured in milliseconds, "-1" means unlimited)
    duration = -1

  # FixIt 0.2.15 | NEW Mermaid config
  [params.mermaid]
    # For values, see https://mermaid-js.github.io/mermaid/#/Setup?id=theme
    themes = ["neutral", "dark"]

  # FixIt 0.2.12 | NEW PanguJS config
  [params.pangu]
    # For Chinese writing
    enable = false

  # FixIt 0.2.12 | NEW Watermark config
  # Detail config see https://github.com/Lruihao/watermark#readme
  [params.watermark]
    enable = false
    # watermark's text (HTML format is supported)
    content = ""
    # watermark's transparency
    opacity = 0.1
    # parent of watermark's container
    appendTo = ".wrapper>main"
    # watermark's width. unit: px
    width = 150
    # watermark's height. unit: px
    height = 20
    # row spacing of watermarks. unit: px
    rowSpacing = 60
    # col spacing of watermarks. unit: px
    colSpacing = 30
    # watermark's tangent angle. unit: deg
    rotate = 15
    # watermark's fontSize. unit: rem
    fontSize = 0.85
    # FixIt 0.2.13 | NEW watermark's fontFamily
    fontFamily = "inherit"

  # FixIt 0.2.12 | NEW Busuanzi count
  [params.ibruce]
    enable = false
    # Enable in post meta
    enablePost = false

  # Site verification code config for Google/Bing/Yandex/Pinterest/Baidu/360/Sogou
  [params.verification]
    google = ""
    bing = ""
    yandex = ""
    pinterest = ""
    baidu = ""
    so = ""
    sogou = ""

  # FixIt 0.2.10 | NEW Site SEO config
  [params.seo]
    # image URL
    image = ""
    # thumbnail URL
    thumbnailUrl = ""

  # FixIt 0.2.0 | NEW Analytics config
  [params.analytics]
    enable = false
    # Google Analytics
    [params.analytics.google]
      id = ""
      # whether to anonymize IP
      anonymizeIP = true
    # Fathom Analytics
    [params.analytics.fathom]
      id = ""
      # server url for your tracker if you're self hosting
      server = ""

  # FixIt 0.2.7 | NEW Cookie consent config
  [params.cookieconsent]
    enable = false
    # text strings used for Cookie consent banner
    [params.cookieconsent.content]
      message = ""
      dismiss = ""
      link = ""

  # FixIt 0.2.7 | CHANGED CDN config for third-party library files
  [params.cdn]
    # CDN data file name, disabled by default ["jsdelivr.yml", "unpkg.yml", ...]
    # located in "themes/FixIt/assets/data/cdn/" directory
    # you can store your own data files in the same path under your project: "assets/data/cdn/"
    data = ""

  # FixIt 0.2.8 | NEW Compatibility config
  [params.compatibility]
    # whether to use Polyfill.io to be compatible with older browsers
    polyfill = false
    # whether to use object-fit-images to be compatible with older browsers
    objectFit = false

  # FixIt 0.2.14 | NEW GitHub banner in the top-right or top-left corner
  [params.githubCorner]
    enable = false
    permalink = "https://github.com/hugo-fixit/FixIt"
    title = "View source on GitHub"
    position = "right" # ["left", "right"]

  # FixIt 0.2.14 | NEW Gravatar config
  [params.gravatar]
    # Gravatar host, default: "www.gravatar.com"
    host = "www.gravatar.com" # ["cn.gravatar.com", "gravatar.loli.net", ...]
    style = "" # ["", "mp", "identicon", "monsterid", "wavatar", "retro", "blank", "robohash"]

  # FixIt 0.2.16 | NEW Back to top
  [params.backToTop]
    enable = true
    # Scroll percent label in b2t button
    scrollpercent = false

  # FixIt 0.2.16 | NEW Reading progress bar
  [params.readingProgress]
    enable = false
    # Available values: ["left", "right"]
    start = "left"
    # Available values: ["top", "bottom"]
    position = "top"
    reversed = false
    light = ""
    dark = ""
    height = "2px"

  # FixIt 0.2.17 | NEW Define custom file paths
  # Create your custom files in site directory `layouts/partials/custom` and uncomment needed files below
  [params.customFilePath]
    # aside = "custom/aside.html"
    # profile = "custom/profile.html"
    # footer = "custom/footer.html"

  # FixIt 0.2.15 | NEW Developer options
  [params.dev]
    enable = false
    # Check for updates
    c4u = false
    # Please do not expose to public!
    githubToken = ""
    # Mobile Devtools config
    [params.dev.mDevtools]
      enable = false
      # "vConsole", "eruda" supported
      type = "vConsole"

# -------------------------------------------------------------------------------------
# Hugo version required
# -------------------------------------------------------------------------------------
[module]
  [module.hugoVersion]
    extended = true
    min = "0.84.0"

# Markup related configuration in Hugo
# Hugo 解析文档的配置
[markup]
  # Syntax Highlighting (https://gohugo.io/content-management/syntax-highlighting)
  # 语法高亮设置 (https://gohugo.io/content-management/syntax-highlighting)
  [markup.highlight]
    codeFences = true
    guessSyntax = true
    lineNos = true
    lineNumbersInTable = true
    # false is a necessary configuration (https://github.com/dillonzq/LoveIt/issues/158)
    # false 是必要的设置 (https://github.com/dillonzq/LoveIt/issues/158)
    noClasses = false
  # Goldmark is from Hugo 0.60 the default library used for Markdown
  # Goldmark 是 Hugo 0.60 以来的默认 Markdown 解析库
  [markup.goldmark]
    [markup.goldmark.extensions]
      definitionList = true
      footnote = true
      linkify = true
      strikethrough = true
      table = true
      taskList = true
      typographer = true
    [markup.goldmark.renderer]
      # whether to use HTML tags directly in the document
      # 是否在文档中直接使用 HTML 标签
      unsafe = true
  # Table Of Contents settings
  # 目录设置
  [markup.tableOfContents]
    startLevel = 2
    endLevel = 6

[author]
  name = "Jay"
  email = ""
  link = "https://github.com/Heelie"

# Sitemap config
# 网站地图配置
[sitemap]
  changefreq = "weekly"
  filename = "sitemap.xml"
  priority = 0.5

# Permalinks config (https://gohugo.io/content-management/urls/#permalinks)
# Permalinks 配置 (https://gohugo.io/content-management/urls/#permalinks)
[Permalinks]
# posts = ":year/:month/:filename"
  posts = ":year/:filename"

# Privacy config (https://gohugo.io/about/hugo-and-gdpr/)
# 隐私信息配置 (https://gohugo.io/about/hugo-and-gdpr/)
[privacy]
  # privacy of the Google Analytics (replaced by params.analytics.google)
  # Google Analytics 相关隐私 (被 params.analytics.google 替代)
  [privacy.googleAnalytics]
  # ...
  [privacy.twitter]
    enableDNT = true
  [privacy.youtube]
    privacyEnhanced = true

# Options to make output .md files
# 用于输出 Markdown 格式文档的设置
[mediaTypes]
  [mediaTypes."text/plain"]
    suffixes = ["md"]

# Options to make output .md files
# 用于输出 Markdown 格式文档的设置
[outputFormats.MarkDown]
  mediaType = "text/plain"
  isPlainText = true
  isHTML = false

# Options to make hugo output files
# 用于 Hugo 输出文档的设置
[outputs]
  home = ["HTML", "RSS", "JSON"]
  page = ["HTML", "MarkDown"]
  section = ["HTML", "RSS"]
  taxonomy = ["HTML", "RSS"]
  taxonomyTerm = ["HTML"]
```

{{< admonition info "注意" true >}}
重要配置项

搜索引擎参数配置：params.search.algolia

评论系统参数配置：params.page.comment.giscus
{{< /admonition >}}

### 创建文章
```shell
hugo new posts/file.md
```

> 随后可以编辑`posts/file.md`文件，头参数中修改`draft = false`，书写markdown格式文本即可

### 本地启动

```shell
hugo server
```

> 如果只是预览文章，上面的命令为`development`环境，评论系统、CDN、fingerprint都不会启用，需要启用这些功能，则需要使用下面的命令

```shell
hugo server --environment production
```

### 本地构建生成静态文件

```
hugo -v --gc --minify
```

生成的供部署的文件默认在`public`目录下

### 配置评论系统

[giscus](https://giscus.app/zh-CN) 是由 [GitHub Discussions](https://docs.github.com/en/discussions) 驱动的评论系统

1. 在github创建对应的blog仓库

2. 仓库的Settings -> General -> Features项，勾选`Discussions`开启

3. 然后最主要是下面四个参数，其中`repo`为自己的仓库，`category`一般为`Announcements`，仅供自己和`giscus`修改，剩下两个参数比较麻烦

   ```text
   repo = "heelie/heelie.github.io"
   repoId = ""
   category = "Announcements"
   categoryId = ""
   ```

4. 通过github提供的[graphQL API](https://docs.github.com/en/graphql/overview/explorer)查询剩下俩个参数

   ![github_graphql_overview_explorer](/img/1201/github_graphql_overview_explorer.png)

   > 图片看不清，或者左边不知道怎么选择，可以直接复制下面的把name改为自己的仓库即可
   
   ```graphQL
   {
     viewer {
       login
       repository(name: "heelie.github.io") {
         id
         name
         discussionCategory(slug: "Announcements") {
           id
           name
         }
       }
     }
   }
   ```
   
5. 最后把参数填入配置文件，不出意外文章底部就会出现评论模块了

### 配置搜索引擎

[algolia](https://www.algolia.com/)搜索引擎配置不是很复杂，创建索引，然后拿到所有的API Keys就可以了

1. Github一键登录

   ![algolia_login](/img/1201/algolia_login.png)

2. 登陆之后跳转到仪表盘界面，留意一下有一个选项叫`API Keys`，点击左下角 Settings 

   ![algolia_dashboard](/img/1201/algolia_dashboard.png)

3. 设置界面选择 Applications 

   ![algolia_settings](/img/1201/algolia_settings.png)

4. Applications界面应该默认就已经有了一个 application ，直接改名就可以使用了，或者不改名都行

   ![algolia_applications](/img/1201/algolia_applications.png)

5. 修改完名称之后点击左下角 Settings 上的 Data Sources ，选择 Indices

   ![algolia_data_sources](/img/1201/algolia_data_sources.png)

6. 选择 Create Index 创建一个索引(配置参数要用，需要记录下来)

7. 然后回到首页，点击上面提到的 API Keys ，拿到 `Application ID`、`Search-Only API Key`、`Admin API Key`，其中创建的索引、`Application ID`和`Search-Only API Key`需要填入配置文件，因为algolia的records需要自己上传，所以`Admin API Key`在做 Github Action 自动上传records时需要用到

   ![algolia_api_keys](/img/1201/algolia_api_keys.png)

8. 到此，只需要等上传了records之后就可以做搜索操作了

### 配置百度统计

登陆到百度统计，创建一个站点，随后点击顶部的使用设置，在站点列表中，点击获取代码

然后在本地博客站点的`assets`目录中创建一个`js/custom.js`，然后把百度统计获取到的代码去掉前后的\<script>标签，写入`custom.js`文件中即可

### 配置自动部署

在博客根目录创建`.github/workflows/deploy.yml`文件

```yml
name: github pages release

on:
  push:
    branches:
      - main  # Set a branch that will trigger a deployment
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-22.04
    steps:
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: latest
          extended: true

      - name: Build
        run: hugo -v --gc --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          personal_token: ${{ secrets.PERSONAL_TOKEN }}
          external_repository: heelie/heelie.github.io
          publish_dir: ./public
          publish_branch: main

      - name: Upload Records
        uses: iChochy/Algolia-Upload-Records@main
        env:
          APPLICATION_ID: ${{secrets.ALGOLIA_APPLICATION_ID}}
          ADMIN_API_KEY: ${{secrets.ALGOLIA_ADMIN_API_KEY}}
          INDEX_NAME: ${{secrets.ALGOLIA_INDEX_NAME}}
          FILE_PATH: ./public/index.json
```

yml文件需要的参数配置在对应的仓库设置中

![github_settings_secrets_actions](/img/1201/github_settings_secrets_actions.png)

algolia相关的参数在上面都已经获取到了，其中`PERSONAL_TOKEN`为github个人设置中生成的token

不出意外可以直接点这里直达[Github Tokens](https://github.com/settings/tokens)设置页面，需要在登陆状态

1. 点击Generate new token

   ![github_settings_tokens](/img/1201/github_settings_tokens.png)

2. Note随便写，Expiration闲麻烦的话直接选择No expiration（选择其他项需要记住按时更新token，不然token过期失效了自动部署脚本就会部署失败了），最后勾选workflow项

   ![github_new_personal_access_token](/img/1201/github_new_personal_access_token.png)

3. 然后直接滑到最后点击 Generate token ，点击之后一定要复制保存生成的token，离开页面之后就无法查看了，然后把token保存到仓库配置PERSONAL_TOKEN即可

   ![github_create_personal_access_token](/img/1201/github_create_personal_access_token.png)

<br/>

# **至此，整个博客的相关搭建操作基本就结束了，开始你的写作吧！**



---

> 作者: [Jay](https://github.com/Heelie)  
> URL: https://heelie.github.io/2022/1201_create_blog_step/  

