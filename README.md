
- 页脚部分请见`themes\butterfly\layout\includes\footer.pug`，包括页脚计时器、徽标、文字、布局等

- 封面图在主题配置文件`_config.butterfly.yml`的`default_cover`配置项，建议配置多项后随机刷出封面图

  ```yaml
  cover:
    # display the cover or not (是否顯示文章封面)
    index_enable: true
    aside_enable: true
    archives_enable: true
    # the position of cover in home page (封面顯示的位置)
    # left/right/both
    position: both
    # When cover is not set, the default cover is displayed (當沒有設置cover時，默認的封面顯示)
    default_cover:
      - https://source.fomal.cc/img/default_cover_14.webp
      - https://source.fomal.cc/img/default_cover_15.webp
      # ......
  ```

  

- 加载页面时中间的头像在`custom.css`约1300行附近，直接搜索替换成你自己的头像即可

  ```css
  /* heo 加载动画头像 */
  .loading-img {
    background: url(https://lskypro.acozycotage.net/LightPicture/2022/12/60e5d4e39da7c077.webp)
      no-repeat center center;
    background-size: cover;
  }
  ```

- 页脚时间由`fomal.js`控制，搜索以下代码，将网站诞生时间改为你自己的即可(示例：`2022-08-09`)

  ```js
  /* 页脚计时器 start */
  var now = new Date();
  function createtime() {
    // 当前时间
    now.setTime(now.getTime() + 1000);
    var start = new Date("08/01/2022 00:00:00"); // 旅行者1号开始计算的时间
    var dis = Math.trunc(23400000000 + ((now - start) / 1000) * 17); // 距离=秒数*速度 记住转换毫秒
    var unit = (dis / 149600000).toFixed(6);  // 天文单位
    // 网站诞生时间
    var grt = new Date("08/09/2022 00:00:00");
  ...
    let currentTimeHtml = "";
    (currentTimeHtml =
      hnum < 18 && hnum >= 9
        ? `<img class='boardsign' src='https://lskypro.acozycotage.net/Fomalhaut/badge/F小屋-科研摸鱼中.svg' title='什么时候能够实现财富自由呀~'><br> <div style="font-size:13px;font-weight:bold">本站居然运行了 ${dnum} 天 ${hnum} 小时 ${mnum} 分 ${snum} 秒 <i id="heartbeat" class='fas fa-heartbeat'></i> <br> 旅行者 1 号当前距离地球 ${dis} 千米，约为 ${unit} 个天文单位 🚀</div>`
        : `<img class='boardsign' src='https://lskypro.acozycotage.net/Fomalhaut/badge/F小屋-下班休息啦.svg' title='下班了就该开开心心地玩耍~'><br> <div style="font-size:13px;font-weight:bold">本站居然运行了 ${dnum} 天 ${hnum} 小时 ${mnum} 分 ${snum} 秒 <i id="heartbeat" class='fas fa-heartbeat'></i> <br> 旅行者 1 号当前距离地球 ${dis} 千米，约为 ${unit} 个天文单位 🚀</div>`),
      document.getElementById("workboard") &&
      (document.getElementById("workboard").innerHTML = currentTimeHtml);
  }
  ...
  
  /*页脚计时器 end */
  ```

  

- 控制台字符画，在`fomal.js`找到以下代码，并进行相应的替换，字符画可以到：[Text to ASCII Art Generator (TAAG)](https://patorjk.com/software/taag/#p=display&f=Graffiti&t=Type%20Something%20)生成

  ```js
  /* 控制台输出字符画 start */
  var now1 = new Date();
  
  function createtime1() {
    var grt = new Date("08/09/2022 00:00:00"); //此处修改你的建站时间或者网站上线时间
    now1.setTime(now1.getTime() + 250);
    var days = (now1 - grt) / 1000 / 60 / 60 / 24;
    var dnum = Math.floor(days);
  
    var ascll = [
      `欢迎来到Fomalhaut🥝の小家!`,
      `Future is now 🍭🍭🍭`,
      `
          
  ███████  ██████  ███    ███  █████  ██      ██   ██  █████  ██    ██ ████████ 
  ██      ██    ██ ████  ████ ██   ██ ██      ██   ██ ██   ██ ██    ██    ██    
  █████   ██    ██ ██ ████ ██ ███████ ██      ███████ ███████ ██    ██    ██    
  ██      ██    ██ ██  ██  ██ ██   ██ ██      ██   ██ ██   ██ ██    ██    ██    
  ██       ██████  ██      ██ ██   ██ ███████ ██   ██ ██   ██  ██████     ██   
                                                
  `,
      "小站已经苟活",
      dnum,
      "天啦!",
      "©2022 By Fomalhaut",
    ];
  
    setTimeout(
      console.log.bind(
        console,
        `\n%c${ascll[0]} %c ${ascll[1]} %c ${ascll[2]} %c${ascll[3]}%c ${ascll[4]}%c ${ascll[5]}\n\n%c ${ascll[6]}\n`,
        "color:#39c5bb",
        "",
        "color:#39c5bb",
        "color:#39c5bb",
        "",
        "color:#39c5bb",
        ""
      )
    );
  }
  
  createtime1();
  
  function createtime2() {
    var ascll2 = [`NCC2-036`, `调用前置摄像头拍照成功，识别为「大聪明」`, `Photo captured: `, ` 🤪 `];
  
    setTimeout(
      console.log.bind(
        console,
        `%c ${ascll2[0]} %c ${ascll2[1]} %c \n${ascll2[2]} %c\n${ascll2[3]}`,
        "color:white; background-color:#10bcc0",
        "",
        "",
        'background:url("https://unpkg.zhimg.com/anzhiyu-assets@latest/image/common/tinggge.gif") no-repeat;font-size:450%'
      )
    );
  
    setTimeout(console.log.bind(console, "%c WELCOME %c 欢迎光临，大聪明", "color:white; background-color:#23c682", ""));
  
    setTimeout(
      console.warn.bind(
        console,
        "%c ⚡ Powered by Fomalhaut🥝 %c 你正在访问Fomalhaut🥝の小家",
        "color:white; background-color:#f0ad4e",
        ""
      )
    );
  
    setTimeout(console.log.bind(console, "%c W23-12 %c 系统监测到你已打开控制台", "color:white; background-color:#4f90d9", ""));
    setTimeout(
      console.warn.bind(console, "%c S013-782 %c 你现在正处于监控中", "color:white; background-color:#d9534f", "")
    );
  }
  createtime2();
  ...
  /* 控制台输出字符画 end */
  ```


- 加载头像见`themes\butterfly\source\css\_custom\custom.css`下的：

  ```css
  .loading-img {
    background: url(https://lskypro.acozycotage.net/LightPicture/2022/12/60e5d4e39da7c077.webp)
      no-repeat center center;
    background-size: cover;
  }
  ```

- 文章打赏彩蛋，见主题配置文件：`_config.butterfly.yml`

  ```yml
  # Sponsor/reward
  reward:
    enable: true
    coinAudio: https://npm.elemecdn.com/akilar-candyassets@1.0.36/audio/aowu.m4a
    QR_code:
      - img: https://tuchuang.voooe.cn/images/2023/01/04/2.webp
        link:
        text: 微信
      - img: https://tuchuang.voooe.cn/images/2023/01/04/20f8e49805975b8f8.webp
        link:
        text: 支付宝
  ```

- 哔哔页面样式部分：见`source\personal\bb\index.md`：

  ```markdown
  ---
  title: 唠叨
  date: 2022-09-08 23:08:13
  comments: false
  ---
  
  <style>
  /* 哔哔页面 */
  #bibi button {
    color: #fff;
    border: 0;
    margin: 20px auto;
    border-radius: 0.3125rem;
    display: block;
    padding: 0 1rem;
    height: 40px;
    font-weight: 500;
    text-align: center;
    transition: all 0.5s ease-out;
    background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
    background-size: 1000% 1000%;
    animation: Gradient 60s linear infinite;
    outline: 0;
  }
  
  #bibi .bb-info {
    font-weight: 700;
    font-size: 22px;
  }
  
  #bibi .bb-card {
    padding: 15px;
    border-radius: 10px;
    background: rgba(255, 255, 255, 0.1);
    border: 1px solid #a5a5a5ee;
    margin-top: 20px;
    transition: all 0.25s;
    user-select: none;
    width: calc(48% - 7px);
    margin: 10px;
  }
  
  @media screen and (max-width: 800px) {
    #bibi .bb-card {
    width: 100%;
    }
  }
  
  #bibi .bb-card:hover {
    box-shadow: 0 5px 10px 8px #07111b29;
    transform: translateY(-3px);
  }
  
  #bibi .card-header {
    display: flex;
    align-items: center;
  }
  
  #bibi .card-header .avatar {
    width: 32px;
    height: 32px;
    border-radius: 50%;
    margin-right: 10px;
    border-radius: 20px;
    overflow: hidden;
  }
  
  #bibi .card-header svg {
    height: 20px;
    width: 20px;
    margin-left: 5px;
  }
  
  #bibi .card-header .card-time {
    font-size: 12px;
    text-shadow: #d9d9d9 0 0 1px, #fffffb 0 0 1px, #fffffb 0 0 2px;
    margin-left: 10px;
  }
  
  #bibi .card-content {
    padding: 10px 0;
    white-space: pre-wrap;
  }
  
  #bibi .card-footer {
    display: flex;
    padding-bottom: 10px;
  }
  
  #bibi .card-footer .card-label {
    border-radius: 5px;
    padding: 0 5px;
    font-weight: 550;
    border-radius: 5px;
    box-shadow: inset 0 -1px 0 rgb(27 31 35 / 12%);
    font-size: 14px;
    user-select: none;
    margin-right: 10px;
  }
  
  div#bb_loading img{
    border-radius: 15px;
  }
  
  #bb-main {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: flex-start;
  }
  
  </style>
  
  <script src="/js/bibi.js"></script>
  
  <div id="bibi">
  <div class="bb-info"></div><div id="bb-main"></div>
  </div>
  
  ```

- themes/butterfly/layout/includes/footer.pug
- themes/butterfly/source/css/_custom/custom.css
- source/js/fomal.js
- 欢迎信息地理位置显示，这个需要配置自己的 key，类似的还有 bibi 的配置、朋友圈等配置均需要参考相关文档改成自己的API

- 网站图标为根目录的`favicon.ico`，替换为你自己的图标即可
- 个人信息卡片的图标和菜单栏等图标，参考[博客魔改教程总结(二)](https://www.fomal.cc/posts/5389e93f.html)中的第4-7项
- 其余配置项基本与Butterfly兼容，参考[Butterfly官方文档](https://butterfly.js.org/)即可




