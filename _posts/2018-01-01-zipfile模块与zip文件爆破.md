---
title: zipfile模块与zip文件爆破
categories:
- code-basic
tags:
- MyHistoryArticle
- code-basic
---
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>zipfile模块与zip文件爆破</title>
    <style type="text/css" media="all">
      body {
        margin: 0;
        font-family: "Helvetica Neue", Helvetica, Arial, "Hiragino Sans GB", sans-serif;
        font-size: 14px;
        line-height: 20px;
        color: #777;
        background-color: white;
      }
      .container {
        width: 700px;
        margin-right: auto;
        margin-left: auto;
      }

      .post {
        font-family: Georgia, "Times New Roman", Times, "SimSun", serif;
        position: relative;
        padding: 70px;
        bottom: 0;
        overflow-y: auto;
        font-size: 16px;
        font-weight: normal;
        line-height: 25px;
        color: #515151;
      }

      .post h1{
        font-size: 50px;
        font-weight: 500;
        line-height: 60px;
        margin-bottom: 40px;
        color: inherit;
      }

      .post p {
        margin: 0 0 35px 0;
      }

      .post img {
        border: 1px solid #D9D9D9;
      }

      .post a {
        color: #28A1C5;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="post">
        <h1 class="title">zipfile模块与zip文件爆破</h1>
        <div class="show-content">
          <p>关于zip文件的加密，你要知道<a href="https://baike.baidu.com/item/%E9%AB%98%E7%BA%A7%E5%8A%A0%E5%AF%86%E6%A0%87%E5%87%86/468774?fr=aladdin" target="_blank">AES加密</a> <br></p><blockquote>
<p>ZipFile.extract(member[path[pwd]])<br></p>
<p>将zip文档内的指定文件<b>解压到当前目录</b>。</p>
<p>参数member指定要解压的文件名称或对应的ZipInfo对象；</p>
<p>参数path指定了解析文件保存的文件夹；<b>参数pwd为解压密码</b></p>
</blockquote><p>先准备一个<a href="http://www.jianshu.com/p/189731be588f" target="_blank">zip加密压缩后的文件xx.zip，密码为123（你随意）</a> <br></p><div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2883590-15413c66bfc22fe9.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2883590-15413c66bfc22fe9.PNG?imageMogr2/auto-orient/strip" data-image-slug="15413c66bfc22fe9" data-width="381" data-height="178"><br><div class="image-caption">脚本<br>
</div>
</div><div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2883590-981f31d897b491dd.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2883590-981f31d897b491dd.PNG?imageMogr2/auto-orient/strip" data-image-slug="981f31d897b491dd" data-width="712" data-height="48"><br><div class="image-caption">运行后，显示密码不对</div>
</div><p>下面改改脚本</p><div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2883590-479982199521bcc9.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2883590-479982199521bcc9.PNG?imageMogr2/auto-orient/strip" data-image-slug="479982199521bcc9" data-width="620" data-height="237"><br><div class="image-caption">图3</div>
</div><p>运行脚本，结果没有任何输出。。</p><div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2883590-fb26fd13fa8a87e8.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2883590-fb26fd13fa8a87e8.PNG?imageMogr2/auto-orient/strip" data-image-slug="fb26fd13fa8a87e8" data-width="460" data-height="89"><br><div class="image-caption"></div>
</div><p>反复检查+抓耳挠腮，搞不清原因在哪。</p><p>经人提示，将11行，改为 print e。输出了密码全都是错误的。怎么回事！zip命令加密没有出错啊！字典文件里明明有正确密码啊！</p><div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2883590-8300069f1936b69d.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2883590-8300069f1936b69d.PNG?imageMogr2/auto-orient/strip" data-image-slug="8300069f1936b69d" data-width="707" data-height="142"><br><div class="image-caption"></div>
</div><p>真相只有一个，排除了所有不可能，那就是— —，zip命令加密 和 zipfile模块加并不是同一种加密！（此时我的郁闷无法言表）</p><p>zip加密还有几种方式？</p><p>为了了解这一点，打开7-zip软件，右键添加到压缩包，看到压缩方法，有LZMA、LZMA2、PPMd、BZip2，至少有4种压缩方式。</p><br><div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2883590-f3d1f4ea3d4ef081.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2883590-f3d1f4ea3d4ef081.PNG?imageMogr2/auto-orient/strip" data-image-slug="f3d1f4ea3d4ef081" data-width="391" data-height="331"><br><div class="image-caption"></div>
</div><br><h4>少年你对力量一无所知！</h4><p>看来，要想用脚本爆破，至少还要准确判断文件的压缩方法。。还能不能好好做渗透测试师了，软件工程要会、网络构建要懂、脚本要精通、密码学要熟练…… 我还是去搬砖吧。</p><p>书上这个例子经过实践证明完全没有卵用！（zipfile在windows下还工作不良），还是着重学习学习脚本怎么写吧！</p><p>上图3，就是平常做ctf 的样子，但是，还不够。要包装一下，让代码更有范。</p><div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2883590-d8a6d2b40ec5ce91.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2883590-d8a6d2b40ec5ce91.PNG?imageMogr2/auto-orient/strip" data-image-slug="d8a6d2b40ec5ce91" data-width="615" data-height="362"><br><div class="image-caption"></div>
</div><p>定义一个功能函数、一个主函数。函数extracFile负责一次又一次尝试用字典密码解压，有两个参数，需要解压的文件，和待尝试的解压密码。</p><p>最后两行调用主函数。</p><hr><p>增加线程功能，对<a href="http://www.jianshu.com/p/4293464c9e28" target="_blank">多线程</a> 应该比较熟悉了。<br></p><div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2883590-4b8146afbf962fef.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2883590-4b8146afbf962fef.PNG?imageMogr2/auto-orient/strip" data-image-slug="4b8146afbf962fef" data-width="670" data-height="331"><br><div class="image-caption"></div>
</div><p>增加可以用来用户交互的功能，使用模块optparse。<a href="http://blog.csdn.net/marksinoberg/article/details/51842197" target="_blank">参考</a></p><div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2883590-b0cac382742c9cd1.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2883590-b0cac382742c9cd1.PNG?imageMogr2/auto-orient/strip" data-image-slug="b0cac382742c9cd1" data-width="727" data-height="662"><br><div class="image-caption"></div>
</div><p>optparse模块还不会用，不要紧，接下来还要详细介绍该模块的。<br></p>
        </div>
      </div>
    </div>
  </body>
</html>
