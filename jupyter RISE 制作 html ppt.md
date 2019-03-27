# jupyter RISE 制作 html ppt
PPT 是个强大的工具，但是笔者的 PPT 制作技术不咋地，所以之前的分享习惯使用 jupyter notebook + RISE，这样使用简单的 markdown 格式加上代码就足够做一次代码分享了。由于是纯文本格式的，我们可以放到 git 仓库里做版本控制。甚至你可以在网页里直接运行 python 代码。做好的分享还可以用 jupyter 导出成 pdf，html等格式分享出去。

安装和使用比较简单，你可以使用 pip 或者其他工具安装。 
```
pip install jupyter    # or python -m pip install jupyter 
```
```

pip install RISE    # https://github.com/damianavila/RISE
jupyter-nbextension install rise --py --sys-prefix
jupyter-nbextension enable rise --py --sys-prefix
```

```
jupyter notebook    # 换到切项目根目录下执行
```
视频里演示

Jupyter notebook RISE 演示

https://zhuanlan.zhihu.com/p/47418033
