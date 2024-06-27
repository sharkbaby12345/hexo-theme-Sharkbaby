<!DOCTYPE html>
 <div class="nsk-post"><div class="post-title"><h1><a href="/post-20417-1" class="post-title-link">自建邮局 | Poste.io邮箱服务搭建教程</a> 
             ><h4>介绍</h4>
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
<p><img src="/source/img/邮局1.png" alt="邮局1.png"></p>
<ol start="2">
<li>配置 Let’s Encrypt 证书。(手动配置证书, 第1个填 .key, 后面2个填 .crt)
</li>
</ol>
<p><img src="/source/img/邮局2.png" alt="邮局2.png"><br>
<img src="/source/img/邮局3.png" alt="邮局3.png"></p>
<ol start="3">
<li>创建 dkim 密钥，生成 key，添加到 DNS 解析记录，就是上面最后一条解析 _s20160910378._domainkey.your-domain.com</li>
</ol>
<blockquote>
<p>左侧点击 Virtual domains 然后点击域名进行配置。</p>
</blockquote>
<blockquote>
<p>点击 DKIM keys，然后点击 Generate new key，生成 key，添加到 DNS 解析记录，就是上面最后一条解析 _s20160910378._domainkey.your-domain.com</p>
</blockquote>
<p><img src="/source/img/邮局4.png" alt="邮局4.png"></p>

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
