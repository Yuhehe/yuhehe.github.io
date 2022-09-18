# 基于GitHub Pages的个人博客搭建

## 写在前面

本博客是基于GitHub Pages，通过[MkDocs](https://www.mkdocs.org/)搭建的，使用了[mkdocs-material](https://github.com/squidfunk/mkdocs-material)主题做美化。

PS：这两者都有简单易懂的入门教程，学习能力强的同学可以直接根据教程来完成搭建

整个搭建流程主要参考了：[BV1hL4y1w72r](https://www.bilibili.com/video/BV1hL4y1w72r?spm_id_from=333.999.0.0&vd_source=23234b658a22fdfa67ca0b2b2e1e0398)，感谢大佬

## 搭建步骤

### 关于GitHub Pages

这个是GitHub自身，将仓库内容发布成网页的功能，原理参考官方文档或者[BV1hL4y1w72r](https://www.bilibili.com/video/BV1hL4y1w72r?spm_id_from=333.999.0.0&vd_source=23234b658a22fdfa67ca0b2b2e1e0398)即可

### 必要内容安装

>   MKDocs和Material都是基于python的，所以在进行本地预览时需要有python环境的支持
>
>   有环境使用洁癖的同学可以先安装anaconda，在conda中通过创建环境，避免 ”污染“ 本地环境

1.  MKDocs安装

    ```bash
    $ pip install mkdocs
    ```

2.  MKDocs-Material安装

    ```bash
    $ pip install mkdocs-material
    ```

都安装好后就可以进行下一步操作了

### 架构搭建

1.  创建一个mkdocs项目，并进入

    ```bash
    $ mkdocs new my-project
    $ cd my-project
    ```

    这里my-project为项目名字，可以自行指定。

    ```bash
    $ mkdocs serve
    INFO    -  Building documentation...
    INFO    -  Cleaning site directory
    [I 160402 15:50:43 server:271] Serving on http://127.0.0.1:8000
    [I 160402 15:50:43 handlers:58] Start watching changes
    [I 160402 15:50:43 handlers:60] Start detecting changes
    ```

    这里使用了serve，目的是使mkdocs开启服务，并将目标主页在本地构建，如果构建成功，则会出现命令行后面的log显示，点击host就可以看到成功构建的主页

2.  使用material主题

    在刚刚使用mkdocs生成的`mkdocs.yml`中，添加

    ```yaml
    theme:
      name: material
    ```

    再次使用``mkdocs build``或者`mkdocs serve`即可看到基于material主题的博客界面了

    如果在GitHub Pages上看到的界面还是原始的mkdocs，那么删除生成的index.md即可

3.  创建Action流

    因为在上传新修改发布的博客内容时，都需要在云端重新build一遍，这时我们就需要通过GitHub Actions来帮助我们完成这一个操作。

    在mkdocs创建的文件夹下，创建`.github/workflows/*.yml`，自行指定名字

    在`yml`文件中，将如下内容写入

    ```yaml
    name: publish site
    on: # 在什么时候触发工作流
      push: # 在从本地main分支被push到GitHub仓库时
        branches:
          - main
      pull_request: # 在main分支合并别人提的pr时
        branches:
          - main
    jobs: # 工作流的具体内容
      deploy:
        runs-on: ubuntu-latest # 创建一个新的云端虚拟机 使用最新Ubuntu系统
        steps:
          - uses: actions/checkout@v2 # 先checkout到main分支
          - uses: actions/setup-python@v2 # 再安装Python3和相关环境
            with:
              python-version: 3.x
          - run: pip install mkdocs-material # 使用pip包管理工具安装mkdocs-material
          - run: mkdocs gh-deploy --force # 使用mkdocs-material部署gh-pages分支
    ```

    具体内容可以参考

    [Publishing your site - Material for MkDocs (squidfunk.github.io)](https://squidfunk.github.io/mkdocs-material/publishing-your-site/#with-github-actions)

    [BV1hL4y1w72r](https://www.bilibili.com/video/BV1hL4y1w72r?spm_id_from=333.999.0.0&vd_source=23234b658a22fdfa67ca0b2b2e1e0398)

    

## 常见错误

### anaconda安装

#### pip报错：

> ValueError: check_hostname requires server_hostname

pip检测到使用代理，不安全，将梯子关掉即可

#### pip下载过慢：

> 参考链接：https://blog.csdn.net/uu_189340/article/details/123972245

使用镜像源下载

使用pip下载时，只需要在下载库的后面加上 `-i`，并添加上镜像源网站就好，如下面下载numpy为例：

```bash
pip install numpy==1.21.4 -i https://pypi.tuna.tsinghua.edu.cn/simple/
```

国内常见的镜像源：

```bash
阿里云 http://mirrors.aliyun.com/pypi/simple/
豆瓣 http://pypi.douban.com/simple/
清华 https://pypi.tuna.tsinghua.edu.cn/simple/
国科大 http://pypi.mirrors.ustc.edu.cn/simple/
```

#### conda install 和 pip install 区别：

> 参考链接：https://www.zhihu.com/question/395145313

### 发布后

#### 图片显示不出来

原因：如果使用typora编辑的markdown，那么在插入图片后，他的默认路径是 `/*.assets/*.png`

而GitHub在资源检索上比较严格，这样的路径在发布时是找不到图片的，需要加一个 `.`

即：`./*.assets/*.png`
