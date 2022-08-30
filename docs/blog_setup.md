# 基于GitHub Pages的个人博客搭建

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

#### ~~图片显示不出来~~

~~原因：如果使用typora编辑的markdown，那么在插入图片后，他的默认路径是 `/*.assets/*.png`~~

~~而这样GitHub在资源检索上比较严格，这样的路径在发布时是找不到图片的，需要加一个 `.`~~

~~即：`./*.assets/*.png`~~
