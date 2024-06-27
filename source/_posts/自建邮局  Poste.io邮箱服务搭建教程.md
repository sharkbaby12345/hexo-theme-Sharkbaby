<!DOCTYPE html>
<html data-server-rendered="true"><head><meta charset="UTF-8"> <meta name="viewport" content="width=device-width, initial-scale=1.0"> <meta http-equiv="Content-Type" content="text/html; charset=utf-8"> <title>自建邮局 | Poste.io邮箱服务搭建教程</title> <meta name="twitter:title" content="自建邮局 | Poste.io邮箱服务搭建教程"> <meta property="og:title" content="自建邮局 | Poste.io邮箱服务搭建教程"> <meta name="twitter:url" content="https://www.nodesek.com/post-20417-1"> <meta property="og:url" content="https://www.nodesek.com/post-20417-1"> <!----> <meta name="twitter:description" content="介绍poste.io邮件服务基于Docker搭建，用的是Haraka+Dovecot+SQLite邮件系统，能够轻易实现邮件收发、多域名控制、邮箱容量控制、邮件杀毒、邮件过滤以及Webmail等基础功能。同时，Poste还提供了投递统计分析、客户端自动适配、一键安装SSL、邮件转发、邮件别名、Catch-All等相"> <meta property="og:description" content="介绍poste.io邮件服务基于Docker搭建，用的是Haraka+Dovecot+SQLite邮件系统，能够轻易实现邮件收发、多域名控制、邮箱容量控制、邮件杀毒、邮件过滤以及Webmail等基础功能。同时，Poste还提供了投递统计分析、客户端自动适配、一键安装SSL、邮件转发、邮件别名、Catch-All等相"> <meta name="description" content="介绍poste.io邮件服务基于Docker搭建，用的是Haraka+Dovecot+SQLite邮件系统，能够轻易实现邮件收发、多域名控制、邮箱容量控制、邮件杀毒、邮件过滤以及Webmail等基础功能。同时，Poste还提供了投递统计分析、客户端自动适配、一键安装SSL、邮件转发、邮件别名、Catch-All等相"> <meta charset="UTF-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="theme-color" content="#3D6C45">
<link rel="icon" type="image/png" sizes="32x32" href="/static/image/favicon/favicon-32x32.png">
<link rel="icon" type="image/png" sizes="16x16" href="/static/image/favicon/favicon-16x16.png">
<link rel="icon" type="image/png" href="/static/image/favicon/favicon-32x32.png">

<link rel="apple-touch-icon" sizes="512x512" href="/static/image/favicon/android-chrome-512x512.png">
<link rel="apple-touch-icon" sizes="180x180" href="/static/image/favicon/apple-touch-icon.png">

<meta name="twitter:card" content="summary">
<meta name="twitter:image" content="/static/image/favicon/android-chrome-512x512.png">
<meta name="twitter:creator" content="@nodeseek">

<meta property="og:type" content="website">
<meta property="og:image" content="/static/image/favicon/android-chrome-512x512.png">
<meta property="og:site_name" content="NodeSeek">
<noscript>
    <style>
        body {
            visibility: visible !important;
        }
    </style>
</noscript>
<style>
</style> <!----> <link rel="stylesheet" href="/static/css/pure-min.e491538ad5d9dff64d669c12655644d0.css"> <!----> <link rel="stylesheet" href="/static/css/nsk-style.2e16a2e00eb2ef41a47f45c87c36fe44.css"> <!----> <!----> <script src="/static/js/icon.713aa9ceb948264faa914e99d47e85bb.js"></script> <script src="/static/js/markdown-it.min.5af4e7547c0782a8e1b5d62f67c679f6.js"></script> <script async="async" src="https://www.googletagmanager.com/gtag/js?id=G-W8C0VXL822"></script> <script>
    window.dataLayer = window.dataLayer || [];
    function gtag() { dataLayer.push(arguments); }
    gtag('js', new Date());

    gtag('config', 'G-W8C0VXL822');
  </script> <!----></head> <!----> <!----> <body class="bg1 light-layout"><header><div id="nsk-head" class="nsk-container"><strong class="site-title"><a href="/"><img src="/static/image/favicon/android-chrome-192x192.png" alt="logo" style="max-height: 36px;vertical-align: middle;"> <span style="vertical-align: middle;">NodeSeek</span><span class="beta-icon">beta</span></a></strong> <ul class="nav-menu"><!----> <!----> <li><a href="/categories/daily">日常</a></li><li><a href="/categories/tech">技术</a></li><li><a href="/categories/info">情报</a></li><li><a href="/categories/review">测评</a></li><li><a href="/categories/trade">交易</a></li><li><a href="/categories/carpool">拼车</a></li><li><a href="/categories/promotion">推广</a></li> <!----> <!----> <!----> <!----></ul> <form method="get" action="https://www.google.com/search" class="search-box pure-form" style="display:block;"><input type="text" id="search-site" value="site:www.nodeseek.com" name="q" style="width: 0;height:0;display: none;"> <input type="text" id="search-site2" name="q" value placeholder="Search" accesskey="/" aria-label="Enter your search term" title="输入搜索内容" role="searchbox" autocomplete="one-time-code" style="height: 30px;"> <svg class="iconpark-icon search-icon" style="width: 17px;height: 17px;color:#666"><use href="#search"></use></svg> <input type="submit" value style="display: none;"> <div class="search-hint"><a href="javascript:void(0)" class="search4post">search for post</a> <a href="javascript:void(0)" class="search4people">search for people</a> <a href="javascript:void(0)" class="googleSearch">use google search</a></div></form> <div class="color-theme-switcher"><svg class="iconpark-icon" style="width: 17px;height: 17px;"><use href="#sun-one"></use> <!----></svg></div></div> <!----></header> <section id="nsk-frame"><div id="nsk-body" class="nsk-container"><div id="nsk-body-left"><!----> <div class="nsk-post-wrapper"><div class="award-corner"><div title="推荐阅读" class="corner-triangle"></div></div> <div class="nsk-post"><div class="post-title"><h1><a href="/post-20417-1" class="post-title-link">自建邮局 | Poste.io邮箱服务搭建教程</a> <!----> <!----></h1></div> <div id="0" data-comment-id="275575" class="content-item" style="position: relative;"><div class="nsk-content-meta-info"><div class="avatar-wrapper"><a title="ookk" href="/space/6410"><img src="/avatar/6410.png" alt="ookk" class="avatar-normal"></a></div> <div><div class="author-info"><a href="/space/6410" class="author-name">ookk</a><span class="is-poster role-tag nsk-badge">楼主</span></div> <div class="content-info"><span class="date-created"><time title="2023-08-22 23:13:55" datetime="2023-08-22T15:13:55.000Z">310days ago</time></span> <!----> <span class="content-category"> in
                <a href="/categories/tech">技术</a></span></div></div> <div class="floor-link-wrapper"><!----> <!----> <a href="#0" class="floor-link">#0</a></div></div> <article class="post-content"><h4>介绍</h4>
<p><a href="/jump?to=https%3A%2F%2Fposte.io%2F" target="_blank">poste.io</a> 邮件服务基于 Docker 搭建，用的是 Haraka + Dovecot + SQLite 邮件系统，能够轻易实现邮件收发、多域名控制、邮箱容量控制、邮件杀毒、邮件过滤以及 Webmail 等基础功能。同时，Poste 还提供了投递统计分析、客户端自动适配、一键安装SSL、邮件转发、邮件别名、Catch-All 等相当有用的功能。</p>
<h4>快速安装</h4>
<blockquote>
<p><a href="/jump?to=https%3A%2F%2Fposte.io%2F" target="_blank">poste.io</a>原生支持docker，占用资源较少，安装简单，适合个人使用。</p>
</blockquote>
<h5>dns配置</h5>
<blockquote>
<p>为了能够正常使用邮件服务，需要配置域名的 MX 记录，将邮件服务器的地址指向你的域名。下文以<code>mail.your-domain.com</code>为例。</p>
</blockquote>
<table>
<thead>
<tr>
<th style="text-align:center">记录类型</th>
<th style="text-align:center">主机记录</th>
<th style="text-align:center">记录值</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center">MX</td>
<td style="text-align:center">your-domain.com</td>
<td style="text-align:center">mail.your-domain.com</td>
</tr>
<tr>
<td style="text-align:center">TXT</td>
<td style="text-align:center">your-domain.com</td>
<td style="text-align:center">v=spf1 mx ~all</td>
</tr>
<tr>
<td style="text-align:center">A</td>
<td style="text-align:center">mail</td>
<td style="text-align:center">1.2.3.4 (your ip)</td>
</tr>
<tr>
<td style="text-align:center">TXT</td>
<td style="text-align:center">_dmarc</td>
<td style="text-align:center">v=DMARC1; p=none; pct=100; rua=mailto:<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="cda0aca4a18db4a2b8bfe0a9a2a0aca4a3e3aea2a0">[email&#160;protected]</a></td>
</tr>
<tr>
<td style="text-align:center">CNAME</td>
<td style="text-align:center">imap</td>
<td style="text-align:center">mail</td>
</tr>
<tr>
<td style="text-align:center">CNAME</td>
<td style="text-align:center">smtp</td>
<td style="text-align:center">mail</td>
</tr>
<tr>
<td style="text-align:center">CNAME</td>
<td style="text-align:center">pop</td>
<td style="text-align:center">mail</td>
</tr>
<tr>
<td style="text-align:center">TXT</td>
<td style="text-align:center">_s20160910378._domainkey.your-domain.com</td>
<td style="text-align:center">k=rsa;p=MII.........</td>
</tr>
</tbody>
</table>
<blockquote>
<p>最后还需要到 VPS 服务商处添加一个反向 DNS，也就是 rDNS 解析，把 IP 解析到 mail.your-domain.com 这个邮件域名就好了，这个为可选项，有些 VPS 商家不提供这种服务。</p>
</blockquote>
<blockquote>
<p>以上 DNS 解析，至少需要添加前面三个 A 解析和 MX 解析，后面几个解析为可选，不添加也能用。</p>
</blockquote>
<h5>修改VPS hostname</h5>
<pre><code class="language-bash">hostnamectl set-hostname mail.your-domain.com
</code></pre>
<pre><code class="language-bash"># 修改hosts文件
vim /etc/hosts
</code></pre>
<pre><code class="language-bash"># 添加一行
127.0.1.1 localhost.localdomain mail.your-domain.com
</code></pre>
<h5>安装docker</h5>
<pre><code class="language-bash"># 安装docker
curl -sSL https://get.docker.com/ | sh
# 启动docker
systemctl start docker
# 设置开机启动
systemctl enable docker
</code></pre>
<h5>安装poste.io</h5>
<blockquote>
<p>用docker compose安装,在要部署<code>poste.io</code>的目录下创建docker-compose.yml文件</p>
</blockquote>
<pre><code class="language-yaml">version: '3.7'

services:
  mailserver:
    image: analogic/poste.io
    hostname: mail.your-domain.com
    ports:
      - &quot;25:25&quot;
      - &quot;110:110&quot;
      - &quot;143:143&quot;
      - &quot;587:587&quot;
      - &quot;993:993&quot;
      - &quot;995:995&quot;
      - &quot;4190:4190&quot;
      - &quot;465:465&quot;
      - &quot;8808:80&quot;
      - &quot;8843:443&quot;
    environment:
      - <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="48040d1c1b0d060b1a11181c170d0509010475292c2521260831273d3a652c2725292126662b2725">[email&#160;protected]</a>
      - LETSENCRYPT_HOST=mail.your-domain.com
      - VIRTUAL_HOST=mail.your-domain.com
      - DISABLE_CLAMAV=TRUE
      - TZ=Asia/Shanghai
      - HTTPS=OFF
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - ./mail-data:/data
</code></pre>
<table>
<thead>
<tr>
<th>服务</th>
<th>端口</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>SMTP</td>
<td>25</td>
<td>SMTP 服务端口</td>
</tr>
<tr>
<td>IMAP</td>
<td>143</td>
<td>IMAP 服务端口</td>
</tr>
<tr>
<td>POP3</td>
<td>110</td>
<td>POP3 服务端口</td>
</tr>
<tr>
<td>SMTPS</td>
<td>465</td>
<td>SMTPS 服务端口</td>
</tr>
<tr>
<td>IMAPS</td>
<td>993</td>
<td>IMAPS 服务端口</td>
</tr>
<tr>
<td>POP3S</td>
<td>995</td>
<td>POP3S 服务端口</td>
</tr>
<tr>
<td>MSA</td>
<td>587</td>
<td>SMTP 端口主要由电子邮件客户端在 STARTTLS 和身份验证之后使用</td>
</tr>
<tr>
<td>Sieve</td>
<td>4190</td>
<td>远程筛子设置</td>
</tr>
<tr>
<td>Webmail</td>
<td>8808</td>
<td>Webmail 服务端口</td>
</tr>
<tr>
<td>Webmail</td>
<td>8843</td>
<td>Webmail 服务端口</td>
</tr>
</tbody>
</table>
<blockquote>
<p>请注意修改里面的域名和存储路径。</p>
</blockquote>
<blockquote>
<p>禁用反病毒功能（DISABLE_CLAMAV=TRUE）、禁用反垃圾邮件功能（DISABLE_RSPAMD=TRUE），可以大幅减低内存和CPU占用，请酌情设置禁用选项。</p>
</blockquote>
<blockquote>
<p>禁用WEB收发功能（DISABLE_ROUNDCUBE=TRUE），可以进一步减少资源占用，不过非必要不建议禁止。</p>
</blockquote>
<blockquote>
<p><code>8808</code>为http端口，可以根据自己的需求修改。</p>
</blockquote>
<pre><code class="language-bash"># 启动poste.io
docker-compose up -d
</code></pre>
<h5>配置NGINX反向代理</h5>
<pre><code class="language-bash">upstream poste_backend {
    server 127.0.0.1:8808;
}

server {
    listen 80;
    listen 443 ssl http2;
    server_name mail.your-domain.com;
    ssl_certificate /etc/nginx/conf.d/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/conf.d/ssl/key.pem;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    add_header Strict-Transport-Security &quot;max-age=31536000; includeSubdomains;&quot;;
    access_log /var/log/nginx/mail.log main;

    location ^~ /.well-known {
        proxy_pass http://poste_backend;
    }

    location / {
        proxy_pass http://poste_backend;
        proxy_set_header Host $host;
        proxy_intercept_errors off;
        # real-ip
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header REMOTE-HOST $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;

        # websocket
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_read_timeout 86400;
     
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    if ($server_port = 80 ) {
        return 301 https://$host$request_uri;
    }
}

</code></pre>
<h5>配置poste.io</h5>
<blockquote>
<p>通过浏览器访问<code>https://mail.your-domain.com</code>，进入poste.io的配置页面，按照提示进行配置即可。</p>
</blockquote>
<ol>
<li>1、设置管理员账户以及密码，然后进入后台管理页面。</li>
</ol>
<p><img src="https://img1.131213.xyz/file/083f086e6c0ea80ae6036.png" alt="083f086e6c0ea80ae6036.png"></p>
<ol start="2">
<li>配置 Let’s Encrypt 证书。</li>
</ol>
<p><img src="https://img1.131213.xyz/file/665601ef7b20bb48a4795.png" alt="665601ef7b20bb48a4795.png"><br>
<img src="https://img1.131213.xyz/file/a3b5cea8a6e3a6b61ef07.png" alt="a3b5cea8a6e3a6b61ef07.png"></p>
<ol start="3">
<li>创建 dkim 密钥，生成 key，添加到 DNS 解析记录，就是上面最后一条解析 _s20160910378._domainkey.your-domain.com</li>
</ol>
<blockquote>
<p>左侧点击 Virtual domains 然后点击域名进行配置。</p>
</blockquote>
<blockquote>
<p>点击 DKIM keys，然后点击 Generate new key，生成 key，添加到 DNS 解析记录，就是上面最后一条解析 _s20160910378._domainkey.your-domain.com</p>
</blockquote>
<p><img src="https://img1.131213.xyz/file/8b0c46665218fb98acd17.png" alt="8b0c46665218fb98acd17.png"></p>
<h4>配置邮件客户端</h4>
<blockquote>
<p>第三方客户端 <code>SMTP/IMAP/POP3</code> 配置</p>
</blockquote>
<table>
<thead>
<tr>
<th>协议</th>
<th>服务器地址</th>
<th>端口</th>
<th>SSL</th>
</tr>
</thead>
<tbody>
<tr>
<td>SMTP</td>
<td>mail.your-domain.com,smtp.your-domain.com</td>
<td>25, 465, 587</td>
<td>STARTTLS</td>
</tr>
<tr>
<td>IMAP</td>
<td>mail.your-domain.com,imap.your-domain.com</td>
<td>993, 143</td>
<td>STARTTLS</td>
</tr>
<tr>
<td>POP3</td>
<td>mail.your-domain.com,pop.your-domain.com</td>
<td>995, 110</td>
<td>STARTTLS</td>
</tr>
</tbody>
</table>
<p>参考：<a href="/jump?to=https%3A%2F%2Fposte.io%2Fdoc" target="_blank">https://poste.io/doc</a></p>
</article> <!----> <div class="comment-menu-mount"></div></div></div> <div class="comment-container"><div style="padding: 2px 0;display: flex;justify-content: flex-end;"><div class="nsk-pager post-top-pager"><div role="navigation" aria-label="pagination"><span aria-disabled="true" class="pager-prev"><div class="triangle-left"></div></span> <!----> <span href="/post-20417-1" aria-label="page1" aria-current="page" class="pager-pos pager-cur">1</span><a href="/post-20417-2" aria-label="page2" aria-current="page" class="pager-pos">2</a><a href="/post-20417-3" aria-label="page3" aria-current="page" class="pager-pos">3</a><a href="/post-20417-4" aria-label="page4" aria-current="page" class="pager-pos">4</a><a href="/post-20417-5" aria-label="page5" aria-current="page" class="pager-pos">5</a> <a href="/post-20417-12" class="pager-pos"><span class="ellipsis">..</span>12</a> <a href="/post-20417-2" rel="next" class="pager-next"><div class="triangle-right"></div></a></div></div></div> <ul class="comments"><li data-comment-id="281039" id="26" class="content-item"><div class="nsk-content-meta-info"><div class="avatar-wrapper"><a title="墨鱼" href="/space/3996"><img src="/avatar/3996.png" alt="墨鱼" class="avatar-normal"></a></div> <div><div class="author-info"><a href="/space/3996" class="author-name">墨鱼</a><!----></div> <div class="content-info"><span class="date-created"><time title="2023-08-25 11:17:22" datetime="2023-08-25T03:17:22.000Z">307days ago</time></span> <!----> <!----></div></div> <div class="floor-link-wrapper"><!----> <div class="hot-badge"></div> <a href="#26" class="floor-link">#26</a></div></div> <article class="post-content"><p>学习一下</p>
</article> <!----> <div class="comment-menu-mount"></div></li><li data-comment-id="275578" id="1" class="content-item"><div class="nsk-content-meta-info"><div class="avatar-wrapper"><a title="--" href="/space/2413"><img src="/avatar/2413.png" alt="--" class="avatar-normal"></a></div> <div><div class="author-info"><a href="/space/2413" class="author-name">--</a><!----></div> <div class="content-info"><span class="date-created"><time title="2023-08-22 23:16:56" datetime="2023-08-22T15:16:56.000Z">310days ago</time></span> <!----> <!----></div></div> <div class="floor-link-wrapper"><!----> <!----> <a href="#1" class="floor-link">#1</a></div></div> <article class="post-content"><p>顶</p>
</article> <!----> <div class="comment-menu-mount"></div></li><li data-comment-id="275586" id="2" class="content-item"><div class="nsk-content-meta-info"><div class="avatar-wrapper"><a title="ratneo" href="/space/5405"><img src="/avatar/5405.png" alt="ratneo" class="avatar-normal"></a></div> <div><div class="author-info"><a href="/space/5405" class="author-name">ratneo</a><!----></div> <div class="content-info"><span class="date-created"><time title="2023-08-22 23:21:04" datetime="2023-08-22T15:21:04.000Z">310days ago</time></span> <!----> <!----></div></div> <div class="floor-link-wrapper"><!----> <!----> <a href="#2" class="floor-link">#2</a></div></div> <article class="post-content"><p>赞！！好文章</p>
</article> <!----> <div class="comment-menu-mount"></div></li><li data-comment-id="275592" id="3" class="content-item"><div class="nsk-content-meta-info"><div class="avatar-wrapper"><a title="yuniee" href="/space/2668"><img src="/avatar/2668.png" alt="yuniee" class="avatar-normal"></a></div> <div><div class="author-info"><a href="/space/2668" class="author-name">yuniee</a><!----></div> <div class="content-info"><span class="date-created"><time title="2023-08-22 23:23:14" datetime="2023-08-22T15:23:14.000Z">310days ago</time></span> <!----> <!----></div></div> <div class="floor-link-wrapper"><!----> <!----> <a href="#3" class="floor-link">#3</a></div></div> <article class="post-content"><p>禁了反病毒反垃圾后啥最低配置要求才能使用</p>
</article> <!----> <div class="comment-menu-mount"></div></li><li data-comment-id="275597" id="4" class="content-item"><div class="nsk-content-meta-info"><div class="avatar-wrapper"><a title="Tony" href="/space/1566"><img src="/avatar/1566.png" alt="Tony" class="avatar-normal"></a></div> <div><div class="author-info"><a href="/space/1566" class="author-name">Tony</a><!----></div> <div class="content-info"><span class="date-created"><time title="2023-08-22 23:24:55" datetime="2023-08-22T15:24:55.000Z">310days ago</time></span> <!----> <!----></div></div> <div class="floor-link-wrapper"><!----> <!----> <a href="#4" class="floor-link">#4</a></div></div> <article class="post-content"><p>这个比Mailu.io占用小吗</p>
</article> <!----> <div class="comment-menu-mount"></div></li><li data-comment-id="275603" id="5" class="content-item"><div class="nsk-content-meta-info"><div class="avatar-wrapper"><a title="cvip" href="/space/6425"><img src="/avatar/6425.png" alt="cvip" class="avatar-normal"></a></div> <div><div class="author-info"><a href="/space/6425" class="author-name">cvip</a><!----></div> <div class="content-info"><span class="date-created"><time title="2023-08-22 23:26:31" datetime="2023-08-22T15:26:31.000Z">310days ago</time></span> <!----> <!----></div></div> <div class="floor-link-wrapper"><!----> <!----> <a href="#5" class="floor-link">#5</a></div></div> <article class="post-content"><p>收藏了</p>
</article> <!----> <div class="comment-menu-mount"></div></li><li data-comment-id="275607" id="6" class="content-item"><div class="nsk-content-meta-info"><div class="avatar-wrapper"><a title="alice" href="/space/6605"><img src="/avatar/6605.png" alt="alice" class="avatar-normal"></a></div> <div><div class="author-info"><a href="/space/6605" class="author-name">alice</a><!----></div> <div class="content-info"><span class="date-created"><time title="2023-08-22 23:28:44" datetime="2023-08-22T15:28:44.000Z">310days ago</time></span> <!----> <!----></div></div> <div class="floor-link-wrapper"><!----> <!----> <a href="#6" class="floor-link">#6</a></div></div> <article class="post-content"><p>收藏了</p>
</article> <!----> <div class="comment-menu-mount"></div></li><li data-comment-id="275610" id="7" class="content-item"><div class="nsk-content-meta-info"><div class="avatar-wrapper"><a title="Veff" href="/space/2995"><img src="/avatar/2995.png" alt="Veff" class="avatar-normal"></a></div> <div><div class="author-info"><a href="/space/2995" class="author-name">Veff</a><!----></div> <div class="content-info"><span class="date-created"><time title="2023-08-22 23:29:50" datetime="2023-08-22T15:29:50.000Z">310days ago</time></span> <!----> <!----></div></div> <div class="floor-link-wrapper"><!----> <!----> <a href="#7" class="floor-link">#7</a></div></div> <article class="post-content"><p>Mark一下</p>
</article> <!----> <div class="comment-menu-mount"></div></li><li data-comment-id="275623" id="8" class="content-item"><div class="nsk-content-meta-info"><div class="avatar-wrapper"><a title="ookk" href="/space/6410"><img src="/avatar/6410.png" alt="ookk" class="avatar-normal"></a></div> <div><div class="author-info"><a href="/space/6410" class="author-name">ookk</a><span class="is-poster role-tag nsk-badge">楼主</span></div> <div class="content-info"><span class="date-created"><time title="2023-08-22 23:32:48" datetime="2023-08-22T15:32:48.000Z">310days ago</time></span> <!----> <!----></div></div> <div class="floor-link-wrapper"><!----> <!----> <a href="#8" class="floor-link">#8</a></div></div> <article class="post-content"><p>@yuniee <a href="/post-20417-1#3">#3</a></p>
<p>我tx轻量2h2g的占用</p>
<p><img src="https://img1.131213.xyz/file/d6d03dc80ac0b268dbc05.png" alt="d6d03dc80ac0b268dbc05.png"></p>
</article> <!----> <div class="comment-menu-mount"></div></li><li data-comment-id="275631" id="9" class="content-item"><div class="nsk-content-meta-info"><div class="avatar-wrapper"><a title="yuniee" href="/space/2668"><img src="/avatar/2668.png" alt="yuniee" class="avatar-normal"></a></div> <div><div class="author-info"><a href="/space/2668" class="author-name">yuniee</a><!----></div> <div class="content-info"><span class="date-created"><time title="2023-08-22 23:34:49" datetime="2023-08-22T15:34:49.000Z">310days ago</time></span> <!----> <!----></div></div> <div class="floor-link-wrapper"><!----> <!----> <a href="#9" class="floor-link">#9</a></div></div> <article class="post-content"><p>@ookk <a href="/post-20417-1#8">#8</a>  <img class="sticker" src="/static/image/sticker/xhj/003.png" loading="lazy" alt="xhj003"> 谢谢</p>
</article> <!----> <div class="comment-menu-mount"></div></li><li data-comment-id="275675" id="10" class="content-item"><div class="nsk-content-meta-info"><div class="avatar-wrapper"><a title="shuang76" href="/space/539"><img src="/avatar/539.png" alt="shuang76" class="avatar-normal"></a></div> <div><div class="author-info"><a href="/space/539" class="author-name">shuang76</a><!----></div> <div class="content-info"><span class="date-created"><time title="2023-08-23 00:03:43" datetime="2023-08-22T16:03:43.000Z">309days ago</time></span> <!----> <!----></div></div> <div class="floor-link-wrapper"><!----> <!----> <a href="#10" class="floor-link">#10</a></div></div> <article class="post-content"><p>感谢分享</p>
</article> <!----> <div class="comment-menu-mount"></div></li></ul> <div style="padding: 2px 0;display: flex;justify-content: flex-end;"><div class="nsk-pager post-bottom-pager"><div role="navigation" aria-label="pagination"><span aria-disabled="true" class="pager-prev"><div class="triangle-left"></div></span> <!----> <span href="/post-20417-1" aria-label="page1" aria-current="page" class="pager-pos pager-cur">1</span><a href="/post-20417-2" aria-label="page2" aria-current="page" class="pager-pos">2</a><a href="/post-20417-3" aria-label="page3" aria-current="page" class="pager-pos">3</a><a href="/post-20417-4" aria-label="page4" aria-current="page" class="pager-pos">4</a><a href="/post-20417-5" aria-label="page5" aria-current="page" class="pager-pos">5</a> <a href="/post-20417-12" class="pager-pos"><span class="ellipsis">..</span>12</a> <a href="/post-20417-2" rel="next" class="pager-next"><div class="triangle-right"></div></a></div></div></div></div> <div class="login-notes" style="padding: 5px;"><a href="/signIn.html" class="Popup" style="color:#2ea44f">登录</a> 或者 <a href="/register.html" style="color:#2ea44f">注册</a>
        后评论.
    </div> <!----></div> <!----> <!----> <!----> <!----> <!----> <!----> <!----> <!----> <!----></div> <div id="nsk-right-panel-container"><div class="nsk-panel"><h4>你好啊，陌生人!</h4> <div class="small-margin">我的朋友，看起来你是新来的，如果想参与到讨论中，点击下面的按钮！</div> <div class="small-margin"><a href="/signIn.html" rel="nofollow" class="btn" style="color:white;margin-right:5px;">登录</a> <a href="/register.html" rel="nofollow" class="btn" style="color:white">注册</a></div></div> <!----> <!----> <div class="nsk-panel"><h4 aria-level="2"><svg class="iconpark-icon"><use href="#rocket-one"></use></svg> <span>快捷功能区</span></h4> <ul role="nav"><li><a href="/award"><svg class="iconpark-icon"><use href="#diamonds"></use></svg> <span>推荐阅读</span></a></li> <li><a href="/ruling"><svg class="iconpark-icon"><use href="#balance-two"></use></svg> <span>管理记录</span></a></li> <li><a href="/lucky"><svg class="iconpark-icon"><use href="#optimize"></use></svg> <span>幸运抽奖</span></a></li> <li><a href="/invite"><svg class="iconpark-icon"><use href="#key"></use></svg> <span>邀请好友</span></a></li> <li><a href="/providers"><svg class="iconpark-icon"><use href="#cooperative-handshake"></use></svg> <span>合作商家</span></a></li> <li><a href="/friends"><svg class="iconpark-icon"><use href="#link"></use></svg> <span>友站链接</span></a></li> <!----></ul></div> <!----> <div class="nsk-panel"><h4 aria-level="2"><svg class="iconpark-icon"><use href="#all-application-73gj9dff"></use></svg> <span>所有版块</span></h4> <ul><li><a href="/categories/daily"><svg class="iconpark-icon"><use href="#tea"></use></svg> <span>日常</span></a></li><li><a href="/categories/tech"><svg class="iconpark-icon"><use href="#formula"></use></svg> <span>技术</span></a></li><li><a href="/categories/info"><svg class="iconpark-icon"><use href="#receiver"></use></svg> <span>情报</span></a></li><li><a href="/categories/review"><svg class="iconpark-icon"><use href="#dashboard-one"></use></svg> <span>测评</span></a></li><li><a href="/categories/trade"><svg class="iconpark-icon"><use href="#dollar"></use></svg> <span>交易</span></a></li><li><a href="/categories/expose"><svg class="iconpark-icon"><use href="#face-recognition"></use></svg> <span>曝光</span></a></li><li><a href="/categories/carpool"><svg class="iconpark-icon"><use href="#car"></use></svg> <span>拼车</span></a></li><li><a href="/categories/life"><svg class="iconpark-icon"><use href="#oval-love-two"></use></svg> <span>生活</span></a></li><li><a href="/categories/photo-share"><svg class="iconpark-icon"><use href="#pic-one"></use></svg> <span>贴图</span></a></li><li><a href="/categories/promotion"><svg class="iconpark-icon"><use href="#hold-interface"></use></svg> <span>推广</span></a></li><li><a href="/categories/dev"><svg class="iconpark-icon"><use href="#terminal"></use></svg> <span>Dev</span></a></li><li><a href="/categories/meaningless"><svg class="iconpark-icon"><use href="#texture"></use></svg> <span>无意义</span></a></li><li><a href="/categories/sandbox"><svg class="iconpark-icon"><use href="#experiment"></use></svg> <span>沙盒</span></a></li></ul></div> <div class="nsk-panel"><h4 aria-level="2">📈用户数目📈</h4> <div style="padding: 5px 20px;">
            目前论坛共有17139位seeker
        </div> <h4 aria-level="2">🎉欢迎新用户🎉</h4> <div class="nsk-new-member-board"><a href="/space/17139" class="new-member-item"><img src="/avatar/17139.png" alt="youyang970" class="avatar-normal"> <div title="youyang970">
                    youyang970
                </div></a><a href="/space/17138" class="new-member-item"><img src="/avatar/17138.png" alt="GETVG" class="avatar-normal"> <div title="GETVG">
                    GETVG
                </div></a><a href="/space/17137" class="new-member-item"><img src="/avatar/17137.png" alt="erbanku" class="avatar-normal"> <div title="erbanku">
                    erbanku
                </div></a><a href="/space/17136" class="new-member-item"><img src="/avatar/17136.png" alt="yifaer" class="avatar-normal"> <div title="yifaer">
                    yifaer
                </div></a> <div style="width:100%;margin:5px 0"></div> <a href="/space/17135" class="new-member-item"><img src="/avatar/17135.png" alt="biubiu957" class="avatar-normal"> <div title="biubiu957">
                        biubiu957
                    </div></a><a href="/space/17134" class="new-member-item"><img src="/avatar/17134.png" alt="xuezhe123" class="avatar-normal"> <div title="xuezhe123">
                        xuezhe123
                    </div></a><a href="/space/17133" class="new-member-item"><img src="/avatar/17133.png" alt="663" class="avatar-normal"> <div title="663">
                        663
                    </div></a><a href="/space/17132" class="new-member-item"><img src="/avatar/17132.png" alt="luck8889" class="avatar-normal"> <div title="luck8889">
                        luck8889
                    </div></a></div></div></div></div> <!----> <!----> <!----> <!----> <!----> <!----> <!----> <!----> <!----> <!----> <!----> <!----></section> <div id="fast-nav-button-group"><div id="back-to-top" class="nav-item-btn"><svg class="iconpark-icon"><use href="#up"></use></svg></div> <div id="back-to-parent" class="nav-item-btn"><svg class="iconpark-icon" style="width: 24px; height: 24px;"><use href="#back"></use></svg></div> <div id="back-to-bottom" class="nav-item-btn"><svg class="iconpark-icon"><use href="#down"></use></svg></div></div> <footer><div class="contain"><div class="col pc"><div class="group-head-link">相关网站</div> <ul><a href="https://lowendtalk.com/" target="_blank" rel="noopener external nofollow"><li>LowEndTalk</li></a> <a href="https://lowendspirit.com/" target="_blank" rel="noopener external nofollow"><li>LowEndSpirit</li></a> <a href="https://hostloc.com/" target="_blank" rel="noopener external nofollow"><li>HostLoc</li></a> <a href="https://serverhunter.com/" target="_blank" rel="noopener external nofollow"><li>ServerHunter</li></a></ul></div> <div class="col pc"><div class="group-head-link">站内导航</div> <ul><a href="/about"><li>关于本站</li></a> <a href="/termsofservice"><li>隐私协议</li></a> <a href="https://rss.nodeseek.com"><li>RSS订阅</li></a> <a href="/sitemap.xml"><li>sitemap</li></a></ul></div> <div class="col"><div class="group-head-link">商业推广</div> <ul><a href="/post-6797-1"><li>商家申请规则</li></a> <a href="/post-6800-1"><li>Premium Provider</li></a> <a href="/providers"><li>合作商家展示</li></a></ul></div> <!----> <div class="col pc"><div class="group-head-link">其他平台</div> <ul><a href="https://t.me/nodeseekc" target="_blank" rel="noopener external nofollow"><li>电报频道</li></a> <a href="https://t.me/nodeseekg" target="_blank" rel="noopener external nofollow"><li>电报群组</li></a></ul></div> <div class="col social"><div class="group-head-link">联系我们</div> <ul><li><a href="https://t.me/nodeseek" target="_blank" rel="noopener external nofollow"><img src="/static/image/telegram-app.svg" width="34" style="width: 32px;"></a></li> <a href="/cdn-cgi/l/email-protection#7a361615031e3a14151e1f091f1f1154191517"><li><img src="/static/image/email-colored.svg" width="32" style="width: 32px;"></li></a></ul></div> <div class="foot"><div style="text-align:center;color:#999;padding: 5px 0;"> Copyright © 2022 - 2024 All rights Reserved</div></div></div></footer> <!----> <script data-cfasync="false" src="/cdn-cgi/scripts/5c5dd728/cloudflare-static/email-decode.min.js"></script><script>
    function thumbImg() {
      //do nothing
    }
    function resizeIframe(obj) {
      obj.style.height = obj.getBoundingClientRect().width / 16 * 9 + 'px'
    }
  </script> <script id="temp-script" type="application/json">eyJ1c2VyIjpudWxsLCJhbGxDYXRlZ29yeSI6W3sia2V5IjoiZGFpbHkiLCJjbl90ZXh0Ijoi5pel5bi4IiwiaWNvbiI6InRlYSIsInNob3dJbk5hdiI6dHJ1ZSwiYWRtaW5Pbmx5IjpmYWxzZSwiY3VzdG9taXphYmxlIjp0cnVlLCJzaG93SW5JbmRleCI6dHJ1ZX0seyJrZXkiOiJ0ZWNoIiwiY25fdGV4dCI6IuaKgOacryIsImljb24iOiJmb3JtdWxhIiwic2hvd0luTmF2Ijp0cnVlLCJhZG1pbk9ubHkiOmZhbHNlLCJjdXN0b21pemFibGUiOnRydWUsInNob3dJbkluZGV4Ijp0cnVlfSx7ImtleSI6ImluZm8iLCJjbl90ZXh0Ijoi5oOF5oqlIiwiaWNvbiI6InJlY2VpdmVyIiwic2hvd0luTmF2Ijp0cnVlLCJhZG1pbk9ubHkiOmZhbHNlLCJjdXN0b21pemFibGUiOnRydWUsInNob3dJbkluZGV4Ijp0cnVlfSx7ImtleSI6InJldmlldyIsImNuX3RleHQiOiLmtYvor4QiLCJpY29uIjoiZGFzaGJvYXJkLW9uZSIsInNob3dJbk5hdiI6dHJ1ZSwiYWRtaW5Pbmx5IjpmYWxzZSwiY3VzdG9taXphYmxlIjp0cnVlLCJzaG93SW5JbmRleCI6dHJ1ZX0seyJrZXkiOiJ0cmFkZSIsImNuX3RleHQiOiLkuqTmmJMiLCJpY29uIjoiZG9sbGFyIiwic2hvd0luTmF2Ijp0cnVlLCJhZG1pbk9ubHkiOmZhbHNlLCJjdXN0b21pemFibGUiOnRydWUsInNob3dJbkluZGV4Ijp0cnVlfSx7ImtleSI6ImV4cG9zZSIsImNuX3RleHQiOiLmm53lhYkiLCJpY29uIjoiZmFjZS1yZWNvZ25pdGlvbiIsInNob3dJbk5hdiI6ZmFsc2UsImFkbWluT25seSI6ZmFsc2UsImN1c3RvbWl6YWJsZSI6dHJ1ZSwic2hvd0luSW5kZXgiOnRydWV9LHsia2V5IjoiY2FycG9vbCIsImNuX3RleHQiOiLmi7zovaYiLCJpY29uIjoiY2FyIiwic2hvd0luTmF2Ijp0cnVlLCJhZG1pbk9ubHkiOmZhbHNlLCJjdXN0b21pemFibGUiOnRydWUsInNob3dJbkluZGV4Ijp0cnVlfSx7ImtleSI6ImxpZmUiLCJjbl90ZXh0Ijoi55Sf5rS7IiwiaWNvbiI6Im92YWwtbG92ZS10d28iLCJzaG93SW5OYXYiOmZhbHNlLCJhZG1pbk9ubHkiOmZhbHNlLCJjdXN0b21pemFibGUiOnRydWUsInNob3dJbkluZGV4IjpmYWxzZX0seyJrZXkiOiJwaG90by1zaGFyZSIsImNuX3RleHQiOiLotLTlm74iLCJpY29uIjoicGljLW9uZSIsInNob3dJbk5hdiI6ZmFsc2UsImFkbWluT25seSI6ZmFsc2UsImN1c3RvbWl6YWJsZSI6dHJ1ZSwic2hvd0luSW5kZXgiOnRydWV9LHsia2V5IjoicHJvbW90aW9uIiwiY25fdGV4dCI6IuaOqOW5vyIsImljb24iOiJob2xkLWludGVyZmFjZSIsInNob3dJbk5hdiI6dHJ1ZSwiYWRtaW5Pbmx5IjpmYWxzZSwiY3VzdG9taXphYmxlIjpmYWxzZSwic2hvd0luSW5kZXgiOmZhbHNlfSx7ImtleSI6ImRldiIsImNuX3RleHQiOiJEZXYiLCJpY29uIjoidGVybWluYWwiLCJzaG93SW5OYXYiOmZhbHNlLCJhZG1pbk9ubHkiOmZhbHNlLCJjdXN0b21pemFibGUiOnRydWUsInNob3dJbkluZGV4Ijp0cnVlfSx7ImtleSI6Im1lYW5pbmdsZXNzIiwiY25fdGV4dCI6IuaXoOaEj+S5iSIsImljb24iOiJ0ZXh0dXJlIiwic2hvd0luTmF2IjpmYWxzZSwiYWRtaW5Pbmx5Ijp0cnVlLCJjdXN0b21pemFibGUiOmZhbHNlLCJzaG93SW5JbmRleCI6ZmFsc2V9LHsia2V5Ijoic2FuZGJveCIsImNuX3RleHQiOiLmspnnm5IiLCJpY29uIjoiZXhwZXJpbWVudCIsInNob3dJbk5hdiI6ZmFsc2UsImFkbWluT25seSI6ZmFsc2UsImN1c3RvbWl6YWJsZSI6ZmFsc2UsInNob3dJbkluZGV4IjpmYWxzZX1dLCJwb3N0RGF0YSI6eyJjYXRlZ29yeSI6InRlY2giLCJjYXRlZ29yeVdvcmQiOiLmioDmnK8iLCJjYXRlZ29yeUxpbmsiOiIvY2F0ZWdvcmllcy90ZWNoIiwicG9zdElkIjoyMDQxNywicG9zdFBhZ2UiOjEsInJhbmsiOjAsInRpdGxlIjoi6Ieq5bu66YKu5bGAIHwgUG9zdGUuaW/pgq7nrrHmnI3liqHmkK3lu7rmlZnnqIsiLCJ2aWV3cyI6IjEwODkxIiwicG9zdFBhZ2VDb3VudCI6MTIsImNvbGxlY3Rpb25Db3VudCI6MTkwLCJjb2xsZWN0ZWQiOmZhbHNlLCJvcCI6eyJ1aWQiOjY0MTAsIm5hbWUiOiJvb2trIn0sImxvY2tlZCI6MCwiYXdhcmQiOjEsImNvbW1lbnRzIjpbeyJjb21tZW50SWQiOjI3NTU3NSwiaG90IjpmYWxzZSwicGluZWQiOmZhbHNlLCJmbG9vckluZGV4IjowLCJwb3N0ZXIiOnsibmFtZSI6Im9va2siLCJ1aWQiOjY0MTAsImluZm8iOiLmpbzkuLsiLCJhdmF0YXIiOiIvYXZhdGFyLzY0MTAucG5nIiwicHJvZmlsZSI6Ii9zcGFjZS82NDEwIiwiaXNPcCI6dHJ1ZSwiaXNNZSI6ZmFsc2UsInJvbGVzIjpbXX0sInRpbWUiOnsiY3JlYXRlZERhdGUiOiIyMDIzLTA4LTIyVDE1OjEzOjU1LjAwMFoiLCJjcmVhdGVkRGF0ZUZvcm1hdGVkIjoiMjAyMy0wOC0yMiAyMzoxMzo1NSIsImVkaXRlZERhdGUiOm51bGwsImVkaXRlZERhdGVGb3JtYXRlZCI6bnVsbCwiY3JlYXRlZERhdGVSZWwiOiIzMTBkYXlzIGFnbyIsImVkaXRlZERhdGVSZWwiOm51bGx9LCJtYXJrZG93biI6IiMjIyDku4vnu41cblxuW3Bvc3RlLmlvXShodHRwczovL3Bvc3RlLmlvLykg6YKu5Lu25pyN5Yqh5Z+65LqOIERvY2tlciDmkK3lu7rvvIznlKjnmoTmmK8gSGFyYWthICsgRG92ZWNvdCArIFNRTGl0ZSDpgq7ku7bns7vnu5/vvIzog73lpJ/ovbvmmJPlrp7njrDpgq7ku7bmlLblj5HjgIHlpJrln5/lkI3mjqfliLbjgIHpgq7nrrHlrrnph4/mjqfliLbjgIHpgq7ku7bmnYDmr5LjgIHpgq7ku7bov4fmu6Tku6Xlj4ogV2VibWFpbCDnrYnln7rnoYDlip/og73jgILlkIzml7bvvIxQb3N0ZSDov5jmj5DkvpvkuobmipXpgJLnu5/orqHliIbmnpDjgIHlrqLmiLfnq6/oh6rliqjpgILphY3jgIHkuIDplK7lronoo4VTU0zjgIHpgq7ku7bovazlj5HjgIHpgq7ku7bliKvlkI3jgIFDYXRjaC1BbGwg562J55u45b2T5pyJ55So55qE5Yqf6IO944CCXG5cblxuXG4jIyMg5b+r6YCf5a6J6KOFXG5cbj4gW3Bvc3RlLmlvXShodHRwczovL3Bvc3RlLmlvLynljp/nlJ/mlK/mjIFkb2NrZXLvvIzljaDnlKjotYTmupDovoPlsJHvvIzlronoo4XnroDljZXvvIzpgILlkIjkuKrkurrkvb/nlKjjgIJcblxuIyMjIyBkbnPphY3nva5cblxuPiDkuLrkuobog73lpJ/mraPluLjkvb/nlKjpgq7ku7bmnI3liqHvvIzpnIDopoHphY3nva7ln5/lkI3nmoQgTVgg6K6w5b2V77yM5bCG6YKu5Lu25pyN5Yqh5Zmo55qE5Zyw5Z2A5oyH5ZCR5L2g55qE5Z+f5ZCN44CC5LiL5paH5LulYG1haWwueW91ci1kb21haW4uY29tYOS4uuS+i+OAglxuXG58IOiusOW9leexu+WeiyB8IOS4u+acuuiusOW9lSB8IOiusOW9leWAvCB8XG58IDotLS0tLS06IHwgOi0tLS0tLTogfCA6LS0tLTogfFxufCAgIE1YICAgICB8ICAgIHlvdXItZG9tYWluLmNvbSAgICAgfCAgbWFpbC55b3VyLWRvbWFpbi5jb20gIHxcbnwgICBUWFQgICAgfCAgICB5b3VyLWRvbWFpbi5jb20gICAgIHwgIHY9c3BmMSBteCB+YWxsICB8XG58ICAgQSAgICAgIHwgICAgbWFpbCAgICAgICAgICAgICAgICB8ICAxLjIuMy40ICh5b3VyIGlwKSAgfFxufCAgIFRYVCAgICB8ICAgIF9kbWFyYyAgICAgICAgICAgICAgfCAgdj1ETUFSQzE7IHA9bm9uZTsgcGN0PTEwMDsgcnVhPW1haWx0bzptYWlsQHlvdXItZG9tYWluLmNvbSAgfFxufCAgIENOQU1FICB8ICAgIGltYXAgICAgICAgICAgICAgICAgfCAgbWFpbCAgfFxufCAgIENOQU1FICB8ICAgIHNtdHAgICAgICAgICAgICAgICAgfCAgbWFpbCAgfFxufCAgIENOQU1FICB8ICAgIHBvcCAgICAgICAgICAgICAgICAgfCAgbWFpbCAgfFxufCAgIFRYVCAgICB8ICAgIF9zMjAxNjA5MTAzNzguX2RvbWFpbmtleS55b3VyLWRvbWFpbi5jb21cdCAgICAgICAgICB8ICBrPXJzYTtwPU1JSS4uLi4uLi4uLiAgfFxuXG5cbj4g5pyA5ZCO6L+Y6ZyA6KaB5YiwIFZQUyDmnI3liqHllYblpITmt7vliqDkuIDkuKrlj43lkJEgRE5T77yM5Lmf5bCx5pivIHJETlMg6Kej5p6Q77yM5oqKIElQIOino+aekOWIsCBtYWlsLnlvdXItZG9tYWluLmNvbSDov5nkuKrpgq7ku7bln5/lkI3lsLHlpb3kuobvvIzov5nkuKrkuLrlj6/pgInpobnvvIzmnInkupsgVlBTIOWVhuWutuS4jeaPkOS+m+i/meenjeacjeWKoeOAglxuXG4+IOS7peS4iiBETlMg6Kej5p6Q77yM6Iez5bCR6ZyA6KaB5re75Yqg5YmN6Z2i5LiJ5LiqIEEg6Kej5p6Q5ZKMIE1YIOino+aekO+8jOWQjumdouWHoOS4quino+aekOS4uuWPr+mAie+8jOS4jea3u+WKoOS5n+iDveeUqOOAglxuXG4jIyMjIOS/ruaUuVZQUyBob3N0bmFtZVxuXG5gYGBiYXNoXG5ob3N0bmFtZWN0bCBzZXQtaG9zdG5hbWUgbWFpbC55b3VyLWRvbWFpbi5jb21cbmBgYFxuXG5gYGBiYXNoXG4jIOS/ruaUuWhvc3Rz5paH5Lu2XG52aW0gL2V0Yy9ob3N0c1xuYGBgXG5cbmBgYGJhc2hcbiMg5re75Yqg5LiA6KGMXG4xMjcuMC4xLjEgbG9jYWxob3N0LmxvY2FsZG9tYWluIG1haWwueW91ci1kb21haW4uY29tXG5gYGBcblxuIyMjIyDlronoo4Vkb2NrZXJcblxuYGBgYmFzaFxuIyDlronoo4Vkb2NrZXJcbmN1cmwgLXNTTCBodHRwczovL2dldC5kb2NrZXIuY29tLyB8IHNoXG4jIOWQr+WKqGRvY2tlclxuc3lzdGVtY3RsIHN0YXJ0IGRvY2tlclxuIyDorr7nva7lvIDmnLrlkK/liqhcbnN5c3RlbWN0bCBlbmFibGUgZG9ja2VyXG5gYGBcblxuIyMjIyDlronoo4Vwb3N0ZS5pb1xuXG4+IOeUqGRvY2tlciBjb21wb3Nl5a6J6KOFLOWcqOimgemDqOe9smBwb3N0ZS5pb2DnmoTnm67lvZXkuIvliJvlu7pkb2NrZXItY29tcG9zZS55bWzmlofku7ZcblxuXG5gYGB5YW1sXG52ZXJzaW9uOiAnMy43J1xuXG5zZXJ2aWNlczpcbiAgbWFpbHNlcnZlcjpcbiAgICBpbWFnZTogYW5hbG9naWMvcG9zdGUuaW9cbiAgICBob3N0bmFtZTogbWFpbC55b3VyLWRvbWFpbi5jb21cbiAgICBwb3J0czpcbiAgICAgIC0gXCIyNToyNVwiXG4gICAgICAtIFwiMTEwOjExMFwiXG4gICAgICAtIFwiMTQzOjE0M1wiXG4gICAgICAtIFwiNTg3OjU4N1wiXG4gICAgICAtIFwiOTkzOjk5M1wiXG4gICAgICAtIFwiOTk1Ojk5NVwiXG4gICAgICAtIFwiNDE5MDo0MTkwXCJcbiAgICAgIC0gXCI0NjU6NDY1XCJcbiAgICAgIC0gXCI4ODA4OjgwXCJcbiAgICAgIC0gXCI4ODQzOjQ0M1wiXG4gICAgZW52aXJvbm1lbnQ6XG4gICAgICAtIExFVFNFTkNSWVBUX0VNQUlMPWFkbWluQHlvdXItZG9tYWluLmNvbVxuICAgICAgLSBMRVRTRU5DUllQVF9IT1NUPW1haWwueW91ci1kb21haW4uY29tXG4gICAgICAtIFZJUlRVQUxfSE9TVD1tYWlsLnlvdXItZG9tYWluLmNvbVxuICAgICAgLSBESVNBQkxFX0NMQU1BVj1UUlVFXG4gICAgICAtIFRaPUFzaWEvU2hhbmdoYWlcbiAgICAgIC0gSFRUUFM9T0ZGXG4gICAgdm9sdW1lczpcbiAgICAgIC0gL2V0Yy9sb2NhbHRpbWU6L2V0Yy9sb2NhbHRpbWU6cm9cbiAgICAgIC0gLi9tYWlsLWRhdGE6L2RhdGFcbmBgYFxuXG58IOacjeWKoSB8IOerr+WPoyB8IOivtOaYjiB8XG58IC0tLSB8IC0tLSB8IC0tLSB8XG58IFNNVFAgfCAyNSB8IFNNVFAg5pyN5Yqh56uv5Y+jIHxcbnwgSU1BUCB8IDE0MyB8IElNQVAg5pyN5Yqh56uv5Y+jIHxcbnwgUE9QMyB8IDExMCB8IFBPUDMg5pyN5Yqh56uv5Y+jIHxcbnwgU01UUFMgfCA0NjUgfCBTTVRQUyDmnI3liqHnq6/lj6MgfFxufCBJTUFQUyB8IDk5MyB8IElNQVBTIOacjeWKoeerr+WPoyB8XG58IFBPUDNTIHwgOTk1IHwgUE9QM1Mg5pyN5Yqh56uv5Y+jIHxcbnwgTVNBICB8IDU4NyB8IFNNVFAg56uv5Y+j5Li76KaB55Sx55S15a2Q6YKu5Lu25a6i5oi356uv5ZyoIFNUQVJUVExTIOWSjOi6q+S7vemqjOivgeS5i+WQjuS9v+eUqCB8XG58U2lldmUgIHwgNDE5MCB8IOi/nOeoi+etm+WtkOiuvue9riB8XG58IFdlYm1haWwgfCA4ODA4IHwgV2VibWFpbCDmnI3liqHnq6/lj6MgfFxufCBXZWJtYWlsIHwgODg0MyB8IFdlYm1haWwg5pyN5Yqh56uv5Y+jIHxcblxuXG4+IOivt+azqOaEj+S/ruaUuemHjOmdoueahOWfn+WQjeWSjOWtmOWCqOi3r+W+hOOAglxuXG4+IOemgeeUqOWPjeeXheavkuWKn+iDve+8iERJU0FCTEVfQ0xBTUFWPVRSVUXvvInjgIHnpoHnlKjlj43lnoPlnL7pgq7ku7blip/og73vvIhESVNBQkxFX1JTUEFNRD1UUlVF77yJ77yM5Y+v5Lul5aSn5bmF5YeP5L2O5YaF5a2Y5ZKMQ1BV5Y2g55So77yM6K+36YWM5oOF6K6+572u56aB55So6YCJ6aG544CCXG5cbj4g56aB55SoV0VC5pS25Y+R5Yqf6IO977yIRElTQUJMRV9ST1VORENVQkU9VFJVRe+8ie+8jOWPr+S7pei/m+S4gOatpeWHj+Wwkei1hOa6kOWNoOeUqO+8jOS4jei/h+mdnuW/heimgeS4jeW7uuiuruemgeatouOAglxuXG4+IGA4ODA4YOS4umh0dHDnq6/lj6PvvIzlj6/ku6XmoLnmja7oh6rlt7HnmoTpnIDmsYLkv67mlLnjgIJcblxuXG5gYGBiYXNoXG4jIOWQr+WKqHBvc3RlLmlvXG5kb2NrZXItY29tcG9zZSB1cCAtZFxuYGBgXG5cblxuIyMjIyDphY3nva5OR0lOWOWPjeWQkeS7o+eQhlxuXG5gYGBiYXNoXG51cHN0cmVhbSBwb3N0ZV9iYWNrZW5kIHtcbiAgICBzZXJ2ZXIgMTI3LjAuMC4xOjg4MDg7XG59XG5cbnNlcnZlciB7XG4gICAgbGlzdGVuIDgwO1xuICAgIGxpc3RlbiA0NDMgc3NsIGh0dHAyO1xuICAgIHNlcnZlcl9uYW1lIG1haWwueW91ci1kb21haW4uY29tO1xuICAgIHNzbF9jZXJ0aWZpY2F0ZSAvZXRjL25naW54L2NvbmYuZC9zc2wvY2VydC5wZW07XG4gICAgc3NsX2NlcnRpZmljYXRlX2tleSAvZXRjL25naW54L2NvbmYuZC9zc2wva2V5LnBlbTtcbiAgICBzc2xfc2Vzc2lvbl90aW1lb3V0IDVtO1xuICAgIHNzbF9jaXBoZXJzIEVDREhFLVJTQS1BRVMxMjgtR0NNLVNIQTI1NjpFQ0RIRTpFQ0RIOkFFUzpISUdIOiFOVUxMOiFhTlVMTDohTUQ1OiFBREg6IVJDNDtcbiAgICBzc2xfcHJvdG9jb2xzIFRMU3YxIFRMU3YxLjEgVExTdjEuMjtcbiAgICBzc2xfcHJlZmVyX3NlcnZlcl9jaXBoZXJzIG9uO1xuICAgIGFkZF9oZWFkZXIgU3RyaWN0LVRyYW5zcG9ydC1TZWN1cml0eSBcIm1heC1hZ2U9MzE1MzYwMDA7IGluY2x1ZGVTdWJkb21haW5zO1wiO1xuICAgIGFjY2Vzc19sb2cgL3Zhci9sb2cvbmdpbngvbWFpbC5sb2cgbWFpbjtcblxuICAgIGxvY2F0aW9uIF5+IC8ud2VsbC1rbm93biB7XG4gICAgICAgIHByb3h5X3Bhc3MgaHR0cDovL3Bvc3RlX2JhY2tlbmQ7XG4gICAgfVxuXG4gICAgbG9jYXRpb24gLyB7XG4gICAgICAgIHByb3h5X3Bhc3MgaHR0cDovL3Bvc3RlX2JhY2tlbmQ7XG4gICAgICAgIHByb3h5X3NldF9oZWFkZXIgSG9zdCAkaG9zdDtcbiAgICAgICAgcHJveHlfaW50ZXJjZXB0X2Vycm9ycyBvZmY7XG4gICAgICAgICMgcmVhbC1pcFxuICAgICAgICBwcm94eV9zZXRfaGVhZGVyIFgtUmVhbC1JUCAkcmVtb3RlX2FkZHI7XG4gICAgICAgIHByb3h5X3NldF9oZWFkZXIgWC1Gb3J3YXJkZWQtRm9yICRwcm94eV9hZGRfeF9mb3J3YXJkZWRfZm9yO1xuICAgICAgICBwcm94eV9zZXRfaGVhZGVyIFJFTU9URS1IT1NUICRyZW1vdGVfYWRkcjtcbiAgICAgICAgcHJveHlfc2V0X2hlYWRlciBYLUZvcndhcmRlZC1Qcm90byAkc2NoZW1lO1xuXG4gICAgICAgICMgd2Vic29ja2V0XG4gICAgICAgIHByb3h5X3NldF9oZWFkZXIgVXBncmFkZSAkaHR0cF91cGdyYWRlO1xuICAgICAgICBwcm94eV9zZXRfaGVhZGVyIENvbm5lY3Rpb24gJ3VwZ3JhZGUnO1xuICAgICAgICBwcm94eV9yZWFkX3RpbWVvdXQgODY0MDA7XG4gICAgIFxuICAgIH1cblxuICAgIGVycm9yX3BhZ2UgICA1MDAgNTAyIDUwMyA1MDQgIC81MHguaHRtbDtcbiAgICBsb2NhdGlvbiA9IC81MHguaHRtbCB7XG4gICAgICAgIHJvb3QgICAvdXNyL3NoYXJlL25naW54L2h0bWw7XG4gICAgfVxuXG4gICAgaWYgKCRzZXJ2ZXJfcG9ydCA9IDgwICkge1xuICAgICAgICByZXR1cm4gMzAxIGh0dHBzOi8vJGhvc3QkcmVxdWVzdF91cmk7XG4gICAgfVxufVxuXG5gYGBcblxuIyMjIyDphY3nva5wb3N0ZS5pb1xuXG4+IOmAmui/h+a1j+iniOWZqOiuv+mXrmBodHRwczovL21haWwueW91ci1kb21haW4uY29tYO+8jOi/m+WFpXBvc3RlLmlv55qE6YWN572u6aG16Z2i77yM5oyJ54Wn5o+Q56S66L+b6KGM6YWN572u5Y2z5Y+v44CCXG5cbjEuIDHjgIHorr7nva7nrqHnkIblkZjotKbmiLfku6Xlj4rlr4bnoIHvvIznhLblkI7ov5vlhaXlkI7lj7DnrqHnkIbpobXpnaLjgIJcblxuIVswODNmMDg2ZTZjMGVhODBhZTYwMzYucG5nXShodHRwczovL2ltZzEuMTMxMjEzLnh5ei9maWxlLzA4M2YwODZlNmMwZWE4MGFlNjAzNi5wbmcpXG5cblxuMi4g6YWN572uIExldOKAmXMgRW5jcnlwdCDor4HkuabjgIJcblxuIVs2NjU2MDFlZjdiMjBiYjQ4YTQ3OTUucG5nXShodHRwczovL2ltZzEuMTMxMjEzLnh5ei9maWxlLzY2NTYwMWVmN2IyMGJiNDhhNDc5NS5wbmcpXG4hW2EzYjVjZWE4YTZlM2E2YjYxZWYwNy5wbmddKGh0dHBzOi8vaW1nMS4xMzEyMTMueHl6L2ZpbGUvYTNiNWNlYThhNmUzYTZiNjFlZjA3LnBuZylcblxuMy4g5Yib5bu6IGRraW0g5a+G6ZKl77yM55Sf5oiQIGtlee+8jOa3u+WKoOWIsCBETlMg6Kej5p6Q6K6w5b2V77yM5bCx5piv5LiK6Z2i5pyA5ZCO5LiA5p2h6Kej5p6QIF9zMjAxNjA5MTAzNzguX2RvbWFpbmtleS55b3VyLWRvbWFpbi5jb21cblxuPiDlt6bkvqfngrnlh7sgVmlydHVhbCBkb21haW5zIOeEtuWQjueCueWHu+Wfn+WQjei/m+ihjOmFjee9ruOAglxuXG4+IOeCueWHuyBES0lNIGtleXPvvIznhLblkI7ngrnlh7sgR2VuZXJhdGUgbmV3IGtlee+8jOeUn+aIkCBrZXnvvIzmt7vliqDliLAgRE5TIOino+aekOiusOW9le+8jOWwseaYr+S4iumdouacgOWQjuS4gOadoeino+aekCBfczIwMTYwOTEwMzc4Ll9kb21haW5rZXkueW91ci1kb21haW4uY29tXG5cbiFbOGIwYzQ2NjY1MjE4ZmI5OGFjZDE3LnBuZ10oaHR0cHM6Ly9pbWcxLjEzMTIxMy54eXovZmlsZS84YjBjNDY2NjUyMThmYjk4YWNkMTcucG5nKVxuXG5cbiMjIyDphY3nva7pgq7ku7blrqLmiLfnq69cblxuPiDnrKzkuInmlrnlrqLmiLfnq68gYFNNVFAvSU1BUC9QT1AzYCDphY3nva5cblxuXG58IOWNj+iuriB8IOacjeWKoeWZqOWcsOWdgCB8IOerr+WPoyB8IFNTTCB8XG58IC0tLSB8IC0tLSB8IC0tLSB8IC0tLSB8XG58IFNNVFAgfCBtYWlsLnlvdXItZG9tYWluLmNvbSxzbXRwLnlvdXItZG9tYWluLmNvbSB8IDI1LCA0NjUsIDU4NyB8IFNUQVJUVExTIHxcbnwgSU1BUCB8IG1haWwueW91ci1kb21haW4uY29tLGltYXAueW91ci1kb21haW4uY29tIHwgOTkzLCAxNDNcdCB8IFNUQVJUVExTIHxcbnwgUE9QMyB8IG1haWwueW91ci1kb21haW4uY29tLHBvcC55b3VyLWRvbWFpbi5jb20gIHwgOTk1LCAxMTAgfCBTVEFSVFRMUyB8XG5cblxuXG5cbuWPguiAg++8mltodHRwczovL3Bvc3RlLmlvL2RvY10oaHR0cHM6Ly9wb3N0ZS5pby9kb2MpIiwibGlrZWQiOmZhbHNlLCJkaXNsaWtlZCI6ZmFsc2UsImxpa2VDb3VudCI6MTUsImRpc2xpa2VDb3VudCI6MSwic2lnbmF0dXJlIjoiIiwiYmxvY2tlZCI6ZmFsc2V9LHsiY29tbWVudElkIjoyODEwMzksImhvdCI6dHJ1ZSwicGluZWQiOmZhbHNlLCJmbG9vckluZGV4IjoyNiwicG9zdGVyIjp7Im5hbWUiOiLloqjpsbwiLCJ1aWQiOjM5OTYsImluZm8iOiIiLCJhdmF0YXIiOiIvYXZhdGFyLzM5OTYucG5nIiwicHJvZmlsZSI6Ii9zcGFjZS8zOTk2IiwiaXNPcCI6ZmFsc2UsImlzTWUiOmZhbHNlLCJyb2xlcyI6W119LCJ0aW1lIjp7ImNyZWF0ZWREYXRlIjoiMjAyMy0wOC0yNVQwMzoxNzoyMi4wMDBaIiwiY3JlYXRlZERhdGVGb3JtYXRlZCI6IjIwMjMtMDgtMjUgMTE6MTc6MjIiLCJlZGl0ZWREYXRlIjpudWxsLCJlZGl0ZWREYXRlRm9ybWF0ZWQiOm51bGwsImNyZWF0ZWREYXRlUmVsIjoiMzA3ZGF5cyBhZ28iLCJlZGl0ZWREYXRlUmVsIjpudWxsfSwibWFya2Rvd24iOiLlrabkuaDkuIDkuIsiLCJsaWtlZCI6ZmFsc2UsImRpc2xpa2VkIjpmYWxzZSwibGlrZUNvdW50IjoyLCJkaXNsaWtlQ291bnQiOjAsInNpZ25hdHVyZSI6IiIsImJsb2NrZWQiOmZhbHNlfSx7ImNvbW1lbnRJZCI6Mjc1NTc4LCJob3QiOmZhbHNlLCJwaW5lZCI6ZmFsc2UsImZsb29ySW5kZXgiOjEsInBvc3RlciI6eyJuYW1lIjoiLS0iLCJ1aWQiOjI0MTMsImluZm8iOiIiLCJhdmF0YXIiOiIvYXZhdGFyLzI0MTMucG5nIiwicHJvZmlsZSI6Ii9zcGFjZS8yNDEzIiwiaXNPcCI6ZmFsc2UsImlzTWUiOmZhbHNlLCJyb2xlcyI6W119LCJ0aW1lIjp7ImNyZWF0ZWREYXRlIjoiMjAyMy0wOC0yMlQxNToxNjo1Ni4wMDBaIiwiY3JlYXRlZERhdGVGb3JtYXRlZCI6IjIwMjMtMDgtMjIgMjM6MTY6NTYiLCJlZGl0ZWREYXRlIjpudWxsLCJlZGl0ZWREYXRlRm9ybWF0ZWQiOm51bGwsImNyZWF0ZWREYXRlUmVsIjoiMzEwZGF5cyBhZ28iLCJlZGl0ZWREYXRlUmVsIjpudWxsfSwibWFya2Rvd24iOiLpobYiLCJsaWtlZCI6ZmFsc2UsImRpc2xpa2VkIjpmYWxzZSwibGlrZUNvdW50IjowLCJkaXNsaWtlQ291bnQiOjAsInNpZ25hdHVyZSI6IiIsImJsb2NrZWQiOmZhbHNlfSx7ImNvbW1lbnRJZCI6Mjc1NTg2LCJob3QiOmZhbHNlLCJwaW5lZCI6ZmFsc2UsImZsb29ySW5kZXgiOjIsInBvc3RlciI6eyJuYW1lIjoicmF0bmVvIiwidWlkIjo1NDA1LCJpbmZvIjoiIiwiYXZhdGFyIjoiL2F2YXRhci81NDA1LnBuZyIsInByb2ZpbGUiOiIvc3BhY2UvNTQwNSIsImlzT3AiOmZhbHNlLCJpc01lIjpmYWxzZSwicm9sZXMiOltdfSwidGltZSI6eyJjcmVhdGVkRGF0ZSI6IjIwMjMtMDgtMjJUMTU6MjE6MDQuMDAwWiIsImNyZWF0ZWREYXRlRm9ybWF0ZWQiOiIyMDIzLTA4LTIyIDIzOjIxOjA0IiwiZWRpdGVkRGF0ZSI6bnVsbCwiZWRpdGVkRGF0ZUZvcm1hdGVkIjpudWxsLCJjcmVhdGVkRGF0ZVJlbCI6IjMxMGRheXMgYWdvIiwiZWRpdGVkRGF0ZVJlbCI6bnVsbH0sIm1hcmtkb3duIjoi6LWe77yB77yB5aW95paH56ugIiwibGlrZWQiOmZhbHNlLCJkaXNsaWtlZCI6ZmFsc2UsImxpa2VDb3VudCI6MCwiZGlzbGlrZUNvdW50IjowLCJzaWduYXR1cmUiOiIiLCJibG9ja2VkIjpmYWxzZX0seyJjb21tZW50SWQiOjI3NTU5MiwiaG90IjpmYWxzZSwicGluZWQiOmZhbHNlLCJmbG9vckluZGV4IjozLCJwb3N0ZXIiOnsibmFtZSI6Inl1bmllZSIsInVpZCI6MjY2OCwiaW5mbyI6IiIsImF2YXRhciI6Ii9hdmF0YXIvMjY2OC5wbmciLCJwcm9maWxlIjoiL3NwYWNlLzI2NjgiLCJpc09wIjpmYWxzZSwiaXNNZSI6ZmFsc2UsInJvbGVzIjpbXX0sInRpbWUiOnsiY3JlYXRlZERhdGUiOiIyMDIzLTA4LTIyVDE1OjIzOjE0LjAwMFoiLCJjcmVhdGVkRGF0ZUZvcm1hdGVkIjoiMjAyMy0wOC0yMiAyMzoyMzoxNCIsImVkaXRlZERhdGUiOm51bGwsImVkaXRlZERhdGVGb3JtYXRlZCI6bnVsbCwiY3JlYXRlZERhdGVSZWwiOiIzMTBkYXlzIGFnbyIsImVkaXRlZERhdGVSZWwiOm51bGx9LCJtYXJrZG93biI6IuemgeS6huWPjeeXheavkuWPjeWeg+WcvuWQjuWVpeacgOS9jumFjee9ruimgeaxguaJjeiDveS9v+eUqCIsImxpa2VkIjpmYWxzZSwiZGlzbGlrZWQiOmZhbHNlLCJsaWtlQ291bnQiOjAsImRpc2xpa2VDb3VudCI6MCwic2lnbmF0dXJlIjoiIiwiYmxvY2tlZCI6ZmFsc2V9LHsiY29tbWVudElkIjoyNzU1OTcsImhvdCI6ZmFsc2UsInBpbmVkIjpmYWxzZSwiZmxvb3JJbmRleCI6NCwicG9zdGVyIjp7Im5hbWUiOiJUb255IiwidWlkIjoxNTY2LCJpbmZvIjoiIiwiYXZhdGFyIjoiL2F2YXRhci8xNTY2LnBuZyIsInByb2ZpbGUiOiIvc3BhY2UvMTU2NiIsImlzT3AiOmZhbHNlLCJpc01lIjpmYWxzZSwicm9sZXMiOltdfSwidGltZSI6eyJjcmVhdGVkRGF0ZSI6IjIwMjMtMDgtMjJUMTU6MjQ6NTUuMDAwWiIsImNyZWF0ZWREYXRlRm9ybWF0ZWQiOiIyMDIzLTA4LTIyIDIzOjI0OjU1IiwiZWRpdGVkRGF0ZSI6bnVsbCwiZWRpdGVkRGF0ZUZvcm1hdGVkIjpudWxsLCJjcmVhdGVkRGF0ZVJlbCI6IjMxMGRheXMgYWdvIiwiZWRpdGVkRGF0ZVJlbCI6bnVsbH0sIm1hcmtkb3duIjoi6L+Z5Liq5q+UTWFpbHUuaW/ljaDnlKjlsI/lkJciLCJsaWtlZCI6ZmFsc2UsImRpc2xpa2VkIjpmYWxzZSwibGlrZUNvdW50IjowLCJkaXNsaWtlQ291bnQiOjAsInNpZ25hdHVyZSI6IiIsImJsb2NrZWQiOmZhbHNlfSx7ImNvbW1lbnRJZCI6Mjc1NjAzLCJob3QiOmZhbHNlLCJwaW5lZCI6ZmFsc2UsImZsb29ySW5kZXgiOjUsInBvc3RlciI6eyJuYW1lIjoiY3ZpcCIsInVpZCI6NjQyNSwiaW5mbyI6IiIsImF2YXRhciI6Ii9hdmF0YXIvNjQyNS5wbmciLCJwcm9maWxlIjoiL3NwYWNlLzY0MjUiLCJpc09wIjpmYWxzZSwiaXNNZSI6ZmFsc2UsInJvbGVzIjpbXX0sInRpbWUiOnsiY3JlYXRlZERhdGUiOiIyMDIzLTA4LTIyVDE1OjI2OjMxLjAwMFoiLCJjcmVhdGVkRGF0ZUZvcm1hdGVkIjoiMjAyMy0wOC0yMiAyMzoyNjozMSIsImVkaXRlZERhdGUiOm51bGwsImVkaXRlZERhdGVGb3JtYXRlZCI6bnVsbCwiY3JlYXRlZERhdGVSZWwiOiIzMTBkYXlzIGFnbyIsImVkaXRlZERhdGVSZWwiOm51bGx9LCJtYXJrZG93biI6IuaUtuiXj+S6hiIsImxpa2VkIjpmYWxzZSwiZGlzbGlrZWQiOmZhbHNlLCJsaWtlQ291bnQiOjAsImRpc2xpa2VDb3VudCI6MCwic2lnbmF0dXJlIjoiIiwiYmxvY2tlZCI6ZmFsc2V9LHsiY29tbWVudElkIjoyNzU2MDcsImhvdCI6ZmFsc2UsInBpbmVkIjpmYWxzZSwiZmxvb3JJbmRleCI6NiwicG9zdGVyIjp7Im5hbWUiOiJhbGljZSIsInVpZCI6NjYwNSwiaW5mbyI6IiIsImF2YXRhciI6Ii9hdmF0YXIvNjYwNS5wbmciLCJwcm9maWxlIjoiL3NwYWNlLzY2MDUiLCJpc09wIjpmYWxzZSwiaXNNZSI6ZmFsc2UsInJvbGVzIjpbXX0sInRpbWUiOnsiY3JlYXRlZERhdGUiOiIyMDIzLTA4LTIyVDE1OjI4OjQ0LjAwMFoiLCJjcmVhdGVkRGF0ZUZvcm1hdGVkIjoiMjAyMy0wOC0yMiAyMzoyODo0NCIsImVkaXRlZERhdGUiOm51bGwsImVkaXRlZERhdGVGb3JtYXRlZCI6bnVsbCwiY3JlYXRlZERhdGVSZWwiOiIzMTBkYXlzIGFnbyIsImVkaXRlZERhdGVSZWwiOm51bGx9LCJtYXJrZG93biI6IuaUtuiXj+S6hiIsImxpa2VkIjpmYWxzZSwiZGlzbGlrZWQiOmZhbHNlLCJsaWtlQ291bnQiOjAsImRpc2xpa2VDb3VudCI6MCwic2lnbmF0dXJlIjoiIiwiYmxvY2tlZCI6ZmFsc2V9LHsiY29tbWVudElkIjoyNzU2MTAsImhvdCI6ZmFsc2UsInBpbmVkIjpmYWxzZSwiZmxvb3JJbmRleCI6NywicG9zdGVyIjp7Im5hbWUiOiJWZWZmIiwidWlkIjoyOTk1LCJpbmZvIjoiIiwiYXZhdGFyIjoiL2F2YXRhci8yOTk1LnBuZyIsInByb2ZpbGUiOiIvc3BhY2UvMjk5NSIsImlzT3AiOmZhbHNlLCJpc01lIjpmYWxzZSwicm9sZXMiOltdfSwidGltZSI6eyJjcmVhdGVkRGF0ZSI6IjIwMjMtMDgtMjJUMTU6Mjk6NTAuMDAwWiIsImNyZWF0ZWREYXRlRm9ybWF0ZWQiOiIyMDIzLTA4LTIyIDIzOjI5OjUwIiwiZWRpdGVkRGF0ZSI6bnVsbCwiZWRpdGVkRGF0ZUZvcm1hdGVkIjpudWxsLCJjcmVhdGVkRGF0ZVJlbCI6IjMxMGRheXMgYWdvIiwiZWRpdGVkRGF0ZVJlbCI6bnVsbH0sIm1hcmtkb3duIjoiTWFya+S4gOS4iyIsImxpa2VkIjpmYWxzZSwiZGlzbGlrZWQiOmZhbHNlLCJsaWtlQ291bnQiOjAsImRpc2xpa2VDb3VudCI6MCwic2lnbmF0dXJlIjoiIiwiYmxvY2tlZCI6ZmFsc2V9LHsiY29tbWVudElkIjoyNzU2MjMsImhvdCI6ZmFsc2UsInBpbmVkIjpmYWxzZSwiZmxvb3JJbmRleCI6OCwicG9zdGVyIjp7Im5hbWUiOiJvb2trIiwidWlkIjo2NDEwLCJpbmZvIjoi5qW85Li7IiwiYXZhdGFyIjoiL2F2YXRhci82NDEwLnBuZyIsInByb2ZpbGUiOiIvc3BhY2UvNjQxMCIsImlzT3AiOnRydWUsImlzTWUiOmZhbHNlLCJyb2xlcyI6W119LCJ0aW1lIjp7ImNyZWF0ZWREYXRlIjoiMjAyMy0wOC0yMlQxNTozMjo0OC4wMDBaIiwiY3JlYXRlZERhdGVGb3JtYXRlZCI6IjIwMjMtMDgtMjIgMjM6MzI6NDgiLCJlZGl0ZWREYXRlIjpudWxsLCJlZGl0ZWREYXRlRm9ybWF0ZWQiOm51bGwsImNyZWF0ZWREYXRlUmVsIjoiMzEwZGF5cyBhZ28iLCJlZGl0ZWREYXRlUmVsIjpudWxsfSwibWFya2Rvd24iOiJAeXVuaWVlIFsjM10oaHR0cHM6Ly93d3cubm9kZXNlZWsuY29tL3Bvc3QtMjA0MTctMSMzKSBcblxu5oiRdHjovbvph48yaDJn55qE5Y2g55SoXG5cbiFbZDZkMDNkYzgwYWMwYjI2OGRiYzA1LnBuZ10oaHR0cHM6Ly9pbWcxLjEzMTIxMy54eXovZmlsZS9kNmQwM2RjODBhYzBiMjY4ZGJjMDUucG5nKSIsImxpa2VkIjpmYWxzZSwiZGlzbGlrZWQiOmZhbHNlLCJsaWtlQ291bnQiOjAsImRpc2xpa2VDb3VudCI6MCwic2lnbmF0dXJlIjoiIiwiYmxvY2tlZCI6ZmFsc2V9LHsiY29tbWVudElkIjoyNzU2MzEsImhvdCI6ZmFsc2UsInBpbmVkIjpmYWxzZSwiZmxvb3JJbmRleCI6OSwicG9zdGVyIjp7Im5hbWUiOiJ5dW5pZWUiLCJ1aWQiOjI2NjgsImluZm8iOiIiLCJhdmF0YXIiOiIvYXZhdGFyLzI2NjgucG5nIiwicHJvZmlsZSI6Ii9zcGFjZS8yNjY4IiwiaXNPcCI6ZmFsc2UsImlzTWUiOmZhbHNlLCJyb2xlcyI6W119LCJ0aW1lIjp7ImNyZWF0ZWREYXRlIjoiMjAyMy0wOC0yMlQxNTozNDo0OS4wMDBaIiwiY3JlYXRlZERhdGVGb3JtYXRlZCI6IjIwMjMtMDgtMjIgMjM6MzQ6NDkiLCJlZGl0ZWREYXRlIjpudWxsLCJlZGl0ZWREYXRlRm9ybWF0ZWQiOm51bGwsImNyZWF0ZWREYXRlUmVsIjoiMzEwZGF5cyBhZ28iLCJlZGl0ZWREYXRlUmVsIjpudWxsfSwibWFya2Rvd24iOiJAb29rayBbIzhdKGh0dHBzOi8vd3d3Lm5vZGVzZWVrLmNvbS9wb3N0LTIwNDE3LTEjOCkgIDp4aGowMDM6IOiwouiwoiIsImxpa2VkIjpmYWxzZSwiZGlzbGlrZWQiOmZhbHNlLCJsaWtlQ291bnQiOjAsImRpc2xpa2VDb3VudCI6MCwic2lnbmF0dXJlIjoiIiwiYmxvY2tlZCI6ZmFsc2V9LHsiY29tbWVudElkIjoyNzU2NzUsImhvdCI6ZmFsc2UsInBpbmVkIjpmYWxzZSwiZmxvb3JJbmRleCI6MTAsInBvc3RlciI6eyJuYW1lIjoic2h1YW5nNzYiLCJ1aWQiOjUzOSwiaW5mbyI6IiIsImF2YXRhciI6Ii9hdmF0YXIvNTM5LnBuZyIsInByb2ZpbGUiOiIvc3BhY2UvNTM5IiwiaXNPcCI6ZmFsc2UsImlzTWUiOmZhbHNlLCJyb2xlcyI6W119LCJ0aW1lIjp7ImNyZWF0ZWREYXRlIjoiMjAyMy0wOC0yMlQxNjowMzo0My4wMDBaIiwiY3JlYXRlZERhdGVGb3JtYXRlZCI6IjIwMjMtMDgtMjMgMDA6MDM6NDMiLCJlZGl0ZWREYXRlIjpudWxsLCJlZGl0ZWREYXRlRm9ybWF0ZWQiOm51bGwsImNyZWF0ZWREYXRlUmVsIjoiMzA5ZGF5cyBhZ28iLCJlZGl0ZWREYXRlUmVsIjpudWxsfSwibWFya2Rvd24iOiLmhJ/osKLliIbkuqsiLCJsaWtlZCI6ZmFsc2UsImRpc2xpa2VkIjpmYWxzZSwibGlrZUNvdW50IjowLCJkaXNsaWtlQ291bnQiOjAsInNpZ25hdHVyZSI6IiIsImJsb2NrZWQiOmZhbHNlfV19LCJjb21tbWVudFBlclBhZ2UiOjEwLCJlbmFibGVDdXN0b21lZFN0eWxlIjp0cnVlfQ==</script> <script>
    (function () {
      function b64DecodeUnicode(str) {
        // Going backwards: from bytestream, to percent-encoding, to original string.
        return decodeURIComponent(atob(str).split('').map(function (c) {
          return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
        }).join(''));
      }
      let el = document.getElementById('temp-script')
      let jsonText = el.textContent;
      if (jsonText) {
        window.__config__ = JSON.parse(b64DecodeUnicode(jsonText))
      }
      el.remove()
    })()
  </script> <script src="/static/js/msc-script.b7ef53c0ce24e1808be482faaf36d542.js"></script> <!----> <!----> <!----> <!----> <!----> <script src="/static/js/index.b1b9979cb7f74b70c297.js"></script> <!----> <!----> <!----> <!----> <!----> <!----> <!----> <!----> <!----> <!----> <!----> <!----> <script src="/static/js/markdownEditor.086c2b20051af4db289c.js" async="async"></script><script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'89a6a358b80ab003',t:'MTcxOTUwMzkwMi4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script><script defer src="https://static.cloudflareinsights.com/beacon.min.js/vcd15cbe7772f49c399c6a5babf22c1241717689176015" integrity="sha512-ZpsOmlRQV6y907TI0dKBHq9Md29nnaEIPlkf84rnaERnq6zvWvPUqr2ft8M1aS28oN72PdrCzSjY4U6VaAw1EQ==" data-cf-beacon='{"rayId":"89a6a358b80ab003","version":"2024.4.1","token":"71bd7c08f35548deaaf0712f464fe37f"}' crossorigin="anonymous"></script>
</body></html>