# 个人博客搭建

## 前提准备

### anaconda安装

#### pip报错：

>   ValueError: check_hostname requires server_hostname

pip检测到使用代理，不安全，将梯子关掉即可

#### pip下载过慢：

>   参考链接：https://blog.csdn.net/uu_189340/article/details/123972245

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

>   参考链接：https://www.zhihu.com/question/395145313